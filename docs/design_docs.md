# APR Tools For Deep Learning Framework design docs

## Target Problem

1. 人工智能框架数据量少
2. 涉及多个语言

对于 1，考虑的方向: Prompt based learning, transfer learning

对于 2，考虑 CIRCLE 一文中使用的方法，学习之后进行一些 refine

## 数据集的提取和解析

通用缺陷修复数据集和框架缺陷修复数据集都提供好了，直接下载。

https://box.nju.edu.cn/d/31372e01ac624d798700/

上述链接下载得到的数据集分为两个部分:

- 通用缺陷数据集
- 框架缺陷数据集

### 通用缺陷数据集

格式太奇怪了，找了别的处理好的开源数据集来用。

### 框架缺陷数据集

问了助教，这个数据是他去 GitHub 上爬的，没处理过.....

source 是 commit 之前的代码段，（就是有 bug 的） target 是 commit 之后的代码段（修复的）。

source：缺陷修复之前的代码片段

target：缺陷修复之后的代码片段

分别有 py 和 cpp 的代码

#### 数据集准备

##### 数据集清洗

目前的构想:

数据集太烂了，需要手工处理一下，分工大概是**一个人至少标 600 个有效数据，py 300 个 cpp 300 个.**

处理之后的数据集格式: 

2 个 feature（即两个列，名字就按下面这个来）:

buggy_code: **错误语句及其上下文**（前后至多各两个语句，如果前面只有一个或者没有也行，但是不能前后都没有）

fixed_code: **正确语句及其上下文**(前后至多各两个语句，原则同上，和 buggy_code 上下文最好一致,如果实在不行也没关系)

- 不能搞成以上格式的就不管。

- 修改 import 或者 include 语句的不管。

- 修改注释的不管

- 非 Python / C++ 的不管 （有些似乎是 shell 命令）

- 修改数字不是 0/1/-1 之间修改的不管

- 注解不管（python 里面 @开头的这种）
- 前后一句上下文都没有的不管（就是说全是 bug 语句，但是上下没有语句）
- 修改篇幅太大的不管
- 助教给的数据集里面的一行可能有多个可以拿来用的 bug

**分工：**

**章华鹏 0-1999 里面选**

**王子搏 2000 - 3999 里面选**

**孙立帆 4000 - 6000 里面选**

**具体来说，自己创建一个 csv 格式的文件，然后第一列名是 buggy_code, 第二列名是 fixed_code，然后每个 bug-fix pair 是一行。**

##### Code Abstraction

魔改一下现有的工具，或者用 antlr 整一个语法分析器简单做一下

##### 数据增强

设计一些规则，通过变异操作来人工引入 bug

## 通用缺陷修复模型的设计

在 CodeT5 上进行微调，这里由于数据预处理上的困难我们没有使用提供的数据集，而是使用论文 : An Empirical Study on Learning Bug-Fixing Patches in the Wild via Neural Machine Translation 中使用的数据集。  

## 框架修复模型的微调

手工标注的数据集进行数据增强之后得到大一点的数据集然后进行微调。

prompt 里面指出编程语言信息。

## 修复效果的评估

使用 exact match 作为 metric 来评估
