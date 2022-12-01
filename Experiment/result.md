# Experiment Result

总数据集按照 5 : 1 来划分训练集和测试集，训练集又按照 5 : 1 来划分得到训练集和验证集。

## CodeT5 original

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

bleu-4: 80.35

exact match: 17.6471
