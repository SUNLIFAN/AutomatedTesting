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

test:

bleu-4: 83.1

exact match:23.5294

bleu-4: 

history:

0 bleu: 71, em: 0
1 bleu: 75.81, em: 0
2 bleu: 77.66, em: 1.8519
3 bleu: 79.73, em: 1.8519
4 bleu: 79.05, em: 1.8519
5 bleu: 81.01, em: 14.8148
6 bleu: 81.92, em: 14.8148
7 bleu: 81.57, em: 18.5185
8 bleu: 81.7, em: 18.5185
9 bleu: 82.07, em: 18.5185
10 bleu: 82.9, em: 20.3704
11 bleu: 82.09, em: 22.2222
12 bleu: 82.73, em: 24.0741
13 bleu: 82.34, em: 22.2222
14 bleu: 82.23, em: 22.2222
15 bleu: 82.22, em: 22.2222
16 bleu: 82.51, em: 24.0741
17 bleu: 82.45, em: 24.0741
18 bleu: 82.43, em: 24.0741

6 epochs no higher bleu, early stop

#### without prompt:

best:

bleu-4: 83.64

exact match: 22.54

这组应该有点异常，测试集比验证集好太多

26 个 epoch 退出