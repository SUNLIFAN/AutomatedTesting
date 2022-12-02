# Experiment Result

总数据集按照 5 : 1 来划分训练集和测试集，训练集又按照 5 : 1 来划分得到训练集和验证集。

## CodeT5 original

在原数据集上的结果:

bleu-4: 89.33
em: 14.4385

在 cpp 数据集上的效果:

avg src len: 33, avg trg len: 45, max src len: 97, max trg len: 122

bleu-4： 35.02

exact match: 0.00

这应该是因为 CodeT5 原来用来进行微调做 code refinement 的数据集是 Java 数据集，而测试的数据集是 C++, 不同的编程语言之间由于关键字或者某些语法的差异都会导致 exact match 失败。

在 py 数据集上的效果:

avg src len: 32, avg trg len: 45, max src len: 80, max trg len: 101

bleu-4: 36.93

em: 0.00

可以看出得到的效果基本上跟在 cpp 的测试集上效果差不多，exact match 基本没有，原因应该也是微调的语言和测试集使用的语言的差异，导致要达到 exact match 基本不太可能。

## CodeBert

使用原数据集训练得到的通用模型在通用测试集上的结果:

bleu-4: 49.03

exact match: 17.7215

在 cpp 数据集上的结果:

bleu-4: 3.09

exact match: 0.00

在 py 数据集上的结果:

bleu-4: 3.09

exact match: 0.00

这里的 bleu-4 比 CodeT5 明显差很多的原因可能是因为 CodeBert 微调的代码序列更短，仅针对缺陷局部进行修复，面对长的输入序列表现很差。

## AutoFix

#### with prompt:

16 epoch, bleu: 83.01, em: 23.5294

16 epoch, bleu: 82.19, em: 23.5294

18 epoch, bleu: 83.74, em: 25.4904

17 epoch, bleu: 83.18, em: 22.5490

24 epoch, bleu: 83.94, em: 22.5490

avg epoch: 18.2

avg bleu: 83.212

avg em: 23.53

example history: 

epoch 0: 70.38 0.0

epoch 1: 76.56 0.0

epoch 2: 78.05 0.0

epoch 3: 79.4 0.0

epoch 4: 79.75 7.4

epoch 5: 80.87 12.963

epoch 6: 80.82 14.81

epoch 7: 81.52 18.5185

epoch 8: 81.61 18.5185

epoch 9: 81.78 18.5185

epoch 10: 82.49 18.5185

epoch 11: 83.02 20.3704

epoch 12: 81.94 24.0794

epoch 13: 83.28 24.0741

epoch 14: 81.99 22.2222

epoch 15: 82.75 24.0741

epoch 16: 82.4 24.0741

epoch 17: 83.6 22.2222

#### without prompt:

16 epoch, bleu: 83.19, em: 21.56

15 epoch, bleu: 82.29, em: 21.56

20 epoch bleu: 83.28, em: 22.54

16 epoch bleu: 83.86, em: 21.56

22 epoch bleu: 83.54, em: 22.549

avg epoch: 17.8 epoch

avg bleu: 83.232

avg em: 21.95

example history:

