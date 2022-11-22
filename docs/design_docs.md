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

## 通用缺陷修复模型的设计

在 CodeT5 上面 finetune

pipeline 设计如下:

(1)prompted function:

输入: buggy code & correct code

输出：prompted input & correct

(2)APR Model:

输入: prompted input & correct code

输出: generated patchy code

(3)loss calculation:

输入: patchy code & correct code

输出: loss & updates weights

## 框架修复模型的微调



## 修复效果的评估

