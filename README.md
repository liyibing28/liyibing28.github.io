button属性修改在bootstrap.css里面
button显示修改在-include/footer.html和base.js里面

liyibing的博客，有关postcrossing，技术，生活

```flow
st=>start: Start
e=>end: End
op1=>operation: My Operation
sub1=>subroutine: My Subroutine
cond=>condition: Yes or No?
io=>inputoutput: catch something...
st->op1->cond
cond(yes)->io->e
cond(no)->sub1(right)->op1
```