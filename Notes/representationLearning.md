# Zero-Shot Program Representation Learning

## Introduction

这篇文章要解决的问题是如何在数据很少的情况下利用在别的语言语料上预训练的模型来得到其他语言的表示，主要思想就是通过调整输入的形式，把 downstream task 转化成 pretraind task 的形式

近年来，出现了一些新的语言（文中以 solidity 为例，主要用于 smart contract），这些语言迅速发展，因此对它们的表示学习是被迫切需要的，但是由于某些语言的标记需要领域知识，因此无法收集足够多的数据来做监督学习。减缓这个问题的方法是使用在别的语言上预训练的模型，然后进行微调。

然而，由于 pretrained task 和 downstream task 存在一定差距，微调是比较困难的，更困难的是当我们在新的语言上也没有足够的数据，或者根本没有可用的数据来进行微调。

文中提出了使用 prompt learning的方式来解决这个问题。

> Prompt learning adapts the downstream task to the same form as that in pre-training through accompanying trainable prompt tokens with the PLM input.

## Approach

### Problem Definition

$x = \lbrace x_1, x_2, ..., x_N\rbrace \in \mathbb{X}$ 是一个代码片段，程序表示学习的目标就是学习一个映射 $f_\theta:\mathbb{X}\to \mathbb{R}^d$,即把程序片段表示为一个 d 维的向量。

### Model architecture

模型的 pipeline 是这样的: 首先把下游任务的输入转换成预训练任务的输入，方法是往输入里面插入 prompt 和 [MASK]. 以上述步骤处理得到的结果作为输入，在预训练任务上重新训练模型. 最后用一个 verbalizer 来转换为最后的输出.

