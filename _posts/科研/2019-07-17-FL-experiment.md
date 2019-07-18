---
layout: post
title: Federated Learning Experiment
category: 科研
tags: FederatedLearning 联邦学习 论文 笔记
keywords: 
description:
---

## 在单台设备下模拟实验

### Federated Learning for image classification

#### Before we start

```python
from __future__ import absolute_import, division, print_function
# Python提供了__future__模块，把下一个新版本的特性导入到当前版本，于是我们就可以在当前版本中测试一些新版本的特性
import collections # Python内建的一个集合模块，提供了许多有用的集合类。

from six.moves import range #处理python2 和 3的函数有位置变化的
import numpy as np # 数组矩阵运算等科学计算库
import tensorflow as tf

import tensorflow_federated as tff

np.random.seed(0)

tf.compat.v1.enable_v2_behavior() # 1.x和2.x版本兼容

tff.federated_computation(lambda: 'Hello, World!')()
```

#### Preparing the input data

为了方便训练，在TFF库中已经添加了常用数据库的FL版本，e.g. MNIST。数据集使用[leaf](https://github.com/TalwalkarLab/leaf)进行处理

```python
#@test {"output": "ignore"} 获取mnist联合训练版本
emnist_train, emnist_test = tff.simulation.datasets.emnist.load_data()
```

dataset returned by load.data() are instances of tff.simulation.ClientData. 允许您枚举用户集，构造表示特定用户数据的tf.data.Dataset，以及查询各个元素的结构。

```python
len(emnist_train.client_ids)

emnist_train.output_types

example_dataset = emnist_train.create_tf_dataset_for_client(
    emnist_train.client_ids[0])

example_element = iter(example_dataset).next()

example_element['label'].numpy()

#@test {"output": "ignore"}
from matplotlib import pyplot as plt
plt.imshow(example_element['pixels'].numpy(), cmap='gray', aspect='equal')
plt.grid('off')
_ = plt.show()
```

生成的数据已经是tf.data.Dataset. 对其数据集转换完成预处理

```python
NUM_EPOCHS = 10
BATCH_SIZE = 20
SHUFFLE_BUFFER = 500


def preprocess(dataset):

  def element_fn(element):
    return collections.OrderedDict([
        ('x', tf.reshape(element['pixels'], [-1])),
        ('y', tf.reshape(element['label'], [1])),
    ])

  return dataset.repeat(NUM_EPOCHS).map(element_fn).shuffle(
      SHUFFLE_BUFFER).batch(BATCH_SIZE)
```

```python
# @test {"output": "ignore"}
preprocessed_example_dataset = preprocess(example_dataset)

sample_batch = tf.nest.map_structure(
    lambda x: x.numpy(), iter(preprocessed_example_dataset).next())

sample_batch
```

为了模拟每个客户端数据，我们可以用list或者tf.dataset，在这里本来就是dataset了，所以可以直接使用

```python
def make_federated_data(client_data, client_ids):
  return [preprocess(client_data.create_tf_dataset_for_client(x))
          for x in client_ids]
```

我们处于模拟环境中，所有数据都是本地可用的。通常情况下，在运行模拟时，我们只需对客户的随机子集进行抽样，以参与每轮培训，每轮培训通常不同。正如你可以通过研究联邦平均算法的论文所发现的那样，在每轮中随机抽样的客户子集的系统中实现收敛可能需要一段时间，并且必须运行数百轮是不切实际的。我们要做的是对客户端集进行一次采样，并在多轮中重复使用相同的集合以加速收敛（故意过度拟合这些少数用户的数据）。我们将其作为练习让读者修改本教程以模拟随机抽样。

```python
#@test {"output": "ignore"}
NUM_CLIENTS = 3

sample_clients = emnist_train.client_ids[0:NUM_CLIENTS]

federated_train_data = make_federated_data(emnist_train, sample_clients)

len(federated_train_data), federated_train_data[0]
```

#### Creating a model with Keras

将Keras上模型适配FL

```python
def create_compiled_keras_model():
  model = tf.keras.models.Sequential([
      tf.keras.layers.Dense(
          10, activation=tf.nn.softmax, kernel_initializer='zeros', input_shape=(784,))])
  
  model.compile(
      loss=tf.keras.losses.SparseCategoricalCrossentropy(),
      optimizer=tf.keras.optimizers.SGD(learning_rate=0.02),
      metrics=[tf.keras.metrics.SparseCategoricalAccuracy()])
  return model
```

为了使用任何带有TFF的模型，需要将其包装在tff.learning.Model接口的实例中，该接口公开方法以标记模型的正向传递，元数据属性等，类似于Keras，但也引入了额外的元素，例如控制计算联合度量的过程的方法。我们暂时不要担心这个问题;如果你有一个我们上面刚刚定义的编译过的Keras模型，你可以通过调用tff.learning.from_compiled_keras_model让TFF为你包装它，将模型和样本数据批量作为参数传递，如下所示。

```python
def model_fn():
  keras_model = create_compiled_keras_model()
  return tff.learning.from_compiled_keras_model(keras_model, sample_batch)
```

#### Training the model on federated data

现在我们有一个包装为tff.learning.Model的模型用于TFF，我们可以通过调用辅助函数tff.learning.build_federated_averaging_process让TFF构造一个联合平均算法，如下所示。参数需要是构造函数（例如上面的model_fn），而不是已经构造的实例，因此模型的构造可以在由TFF控制的上下文中发生

```python
#@test {"output": "ignore"}
iterative_process = tff.learning.build_federated_averaging_process(model_fn)
```

TFF构建了一对联合计算并将它们打包成tff.utils.IterativeProcess，其中这些计算可作为一对初始化和下一个属性使用。联合计算是TFF内部语言中可以表达各种联合算法的程序。在这种情况下，生成并打包到iterative_process中的两个计算实现联合平均。

```python
#@test {"output": "ignore"}
str(iterative_process.initialize.type_signature)
```

虽然上面的类型签名可能起初看起来有点神秘，但您可以认识到服务器状态包含一个模型（MNIST的初始模型参数将分发给所有设备），以及optimizer_state（由服务器维护的附加信息， 例如用于超参数调度表的轮数等。

```python
state = iterative_process.initialize()
```

接下来，这对联合计算中的第二个表示单轮联合平均，其包括将服务器状态（包括模型参数）推送到客户端，对其本地数据进行设备上培训，收集和平均模型更新 ，并在服务器上生成新的更新模型。
从概念上讲，您可以将下一步视为具有如下功能类型签名。
SERVER_STATE，FEDERATED_DATA  - > SERVER_STATE，TRAINING_METRICS
特别是，应该考虑next（）不是作为在服务器上运行的函数，而是作为整个分散计算的声明性功能表示 - 一些输入由服务器（SERVER_STATE）提供，但每个参与 设备提供自己的本地数据集。
让我们进行一轮培训，并将结果可视化。 我们可以将上面已经生成的联合数据用于用户样本。

```python
#@test {"timeout": 600, "output": "ignore"}
state, metrics = iterative_process.next(state, federated_train_data)
print('round  1, metrics={}'.format(metrics))
```

让我们再进行几轮。 如前所述，通常在此时，您将从每轮随机选择的新用户样本中选择一部分模拟数据，以模拟用户不断进出的实际部署，但在这里， 为了演示，我们将重用相同的用户，以便系统快速收敛。

```python
#@test {"skip": true}
for round_num in range(2, 11):
  state, metrics = iterative_process.next(state, federated_train_data)
  print('round {:2d}, metrics={}'.format(round_num, metrics))
```

#### Customizing the model implementation

使用Keras重新构建模型

```python
MnistVariables = collections.namedtuple(
    'MnistVariables', 'weights bias num_examples loss_sum accuracy_sum')
```

```python
def create_mnist_variables():
  return MnistVariables(
      weights = tf.Variable(
          lambda: tf.zeros(dtype=tf.float32, shape=(784, 10)),
          name='weights',
          trainable=True),
      bias = tf.Variable(
          lambda: tf.zeros(dtype=tf.float32, shape=(10)),
          name='bias',
          trainable=True),
      num_examples = tf.Variable(0.0, name='num_examples', trainable=False),
      loss_sum = tf.Variable(0.0, name='loss_sum', trainable=False),
      accuracy_sum = tf.Variable(0.0, name='accuracy_sum', trainable=False))
```

### Federated learning 高阶API

#### 接口

These interfaces are defined primarily in the tff.learning namespace

+ model接口
  + Classes and helper functions 允许将已存在的模型封装和TFF一起使用
  + tff.learning.from_keras_model #封装模型和封装函数类似
  + 也可以通过tff.learning.Model子类实现定制
+ Federated Computation Builders接口
  + Helper functions that construct federated computations for training or evaluation, using your existing models.
  + 辅助函数，使用现有模型构建用于培训或评估的联合计算。
+ Datasets
  + 可以在Python中下载和访问的封装好的数据集，用于模拟联合学习方案。 虽然联邦学习设计用于分散数据，无法简单地在集中位置下载，但在研究和开发阶段，使用可在本地下载和操作的数据进行初始实验通常很方便。

#### local aggregation

+ 该模型首先构造tf.Variables以保存聚合，例如批次数或处理的示例数，每批或每例损失的总和等。
+ TFF多次在模型上调用forward_pass方法，依次在后续批量的客户端数据上调用，这允许您更新包含各种聚合的变量作为副作用。
+ 最后，TFF在您的模型上调用report_local_outputs方法，以允许您的模型将其收集的所有摘要统计信息编译为一组紧凑的度量标准，以供客户端导出。 例如，您的模型代码可以将损失总和除以处理的示例数量，以输出平均损失等。
  
#### Federated aggregation

+ 初始模型和培训所需的任何参数由服务器分发给将参与一轮培训或评估的客户端子集。
+ 在每个客户端上，独立地并行地，在本地数据批次流上重复调用模型代码以生成一组新的模型参数（在培训时），以及一组新的本地度量标准，如上所述（这是本地的 聚合）。
+ TFF运行分布式聚合协议，以在整个系统中累积和聚合模型参数和本地导出的指标。 在Model的federated_output_computation中，使用TFF自己的联合计算语言（不在TensorFlow中）以声明方式表示此逻辑。 有关聚合API的更多信息，请参阅自定义算法教程。

