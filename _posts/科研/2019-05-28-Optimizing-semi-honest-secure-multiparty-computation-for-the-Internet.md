### contribution

提出了

### 论文的假设

1. Our protocol is based on the multiparty garbled-circuit approach

```paper
[5] D. Beaver, S. Micali, and P. Rogaway. The round complexity of secure protocols. In the 22nd STOC, pages 503–513, 1990.
```

1. Semi-honest

### 难点

1. 多方的计算和通信效率大大降低
2. In contrast, in the multiparty case, it is not possible for one party to construct the garbled circuit on its own. This is due to the fact that if that party colludes with one of the parties evaluating the circuit, then the parties’ inputs will all be revealed (since the party who constructed the circuit knows all of the keys).
