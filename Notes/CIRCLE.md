# ISSTA 2022 CIRCLE: Continual Repair across Programming Languages

## 摘要和引言部分

基于深度学习的自动程序修复从已有的程序语料学习修复模式。

DL-Based APR 包含两个部分(encoder-decoder 架构):

- encoder: 抽取缺陷代码和必要上下文的语义并表示为一个固定长度的向量
- decoder: 接受 encoder 编码的向量输出正确的代码

把 APR 作为一个 Neural Machine Translation 任务

一些现有的方法：

- 采用经典的 NMT 模型
- 使用两个 encoder 分别来编码 buggy code 和 上下文
- 编码器中使用 GPT

现有方法的局限：

- 跨语言能力受限
- 离线学习的方式，无法持续接受新的数据，这对 APR 在真实场景的应用造成了阻碍；传统 APR 模型在新的任务上学习之后会遗忘在之前的训练过程中学到的知识。

CIRCLE Solution:

- T5-base model、prompt template、re-repairing 机制
- rehearsal method 和 elastic regularization

prompt: 

prompt 是一串 token，这些 token 会被插入到输入中，来把初始任务表示为语言模型的训练任务的形式。prompt 填补了预训练任务和 downstream task 之间的 gap，使微调变得更容易。本文参考了 Raffel et al. 和 Khashabi et al. ，设计了一组前缀 prompt

Continual Learning: $f_t$ 是在第 $t$ 个任务上训练得到的模型，接收第 $t+1$ 个任务的数据集 $D_{t+1}$来更新 $f_t$，期望得到 $f_{t+1}$ 在第 $t+1$ 个任务上的表现尽量好。

Continual Learning 和 MultiTask Learning 的区别是后者每次新加一个任务就需要在全部的历史数据集上进行重新训练。MultiTask Learning 适用于那些比较稳定的任务，而 Continual Learning 适用于那些数据持续迭代的任务。Continual Learning 的主要挑战是遗忘效应，即忘记之前在历史数据集上学到的知识。

解决遗忘效应的三个主要手段是: Rehearsal，Regularization，Architectural method.

Rehearsal 方法采集历史数据集的某一部分，在后续的训练中会继续使用；

Regularization 方法对模型参数的更新做限制，限制的模型的复杂度；

architectural 方法尝试动态调整模型的模块，模型的参数会随着任务的增长而动态扩增。

CIRCLE 采用的是 rehearsal 和 regularization 的混合方法。

## CIRCLE 的方法

![](./imgs/overview.png)

CIRCLE 包括两个 stage，train 和 test. 在 train stage 先使用 prompt function 把输入转化成 fill-in-the-blank 的形式。我们的训练集包括当前 task 的数据集和从历史数据集中采样的部分数据。然后这些数据被输入到 T5-based model 进行训练。我们保留了原始的 token 来建立词典，而不是像某些方法使用 BPE 算法来得到新的词典。接着 T5-based APR model 根据 prompted 的输入生成一系列候选的修补方案，使用损失函数来进行评估。

为了缓解遗忘效应，在更新参数的时候需要小心。因此，我们使用了 EWC 正则化来计算每个参数对之前的任务的重要性，在更新参数的时候，需要避免对重要的参数做太多更新。最后基于 loss 和 EWC 正则化来更新参数。

当训练收敛的时候，我们通过一种特殊的方法来选择当前任务的训练集的一部分子集，存到存储中，作为未来再次使用的数据。

在推理阶段，APR model 接受所有的 buggy code 输入然后进行修复，生成候选的修补方案。在多语言的场景下，模型很可能生成不同语言中语义相近的关键字，比如 C 的 NULL 和 python 的 None。

我们简单地通过映射的方法对输出做修正。

## 基于 Prompt 的数据表示

输入包括两个部分：buggy code 和 surrounding context

传统的方法通过分别 encode 这两个部分然后把嵌入向量拼起来。

最近 Raffle et al. 提出了一种新的方法，通过把不同的输入拼上某些前缀来得到 prompted 输入，这种方法被证明有利于 downstream task 的微调。

根据这种思想，我们手工设计了 prompted template 来把 buggy code 和 context 转换称 fill-in-the-blank 的形式。

用 "Buggy Line: " 和 "Context: " 来指示 buggy code 和 context，使用 "the fixed code is: " 来指示根据前面的输入生成修复后的代码。 

由于 T5 是在自然语言上的 fill-in-the-blank 进行与训练，因此在 prompted 的输入上进行微调是更加自然的。
