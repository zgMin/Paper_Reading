<img width="758" alt="image" src="https://github.com/zgMin/Paper_Reading/assets/52092775/a81d92f5-62b5-4580-b25b-ea242ed80842">
# Accelerating Large Language Model Decoding with Speculative Sampling
- **作者**: Charlie Chen, Sebastian Borgeaud, Geoffrey Irving, Jean-Baptiste Lespiau, Laurent Sifre and John Jumper
- **出版年份**: 2023
- **期刊/会议**: arXiv preprint arXiv

## 摘要
本文介绍了一种名为猜测抽样（SpS）的方法，用于加速大型语言模型中的变压器解码。作者提出生成一个较短的草稿模型，并使用较大的目标模型对其进行评分，以一次生成多个标记。他们还引入了一种修改后的拒绝抽样方案，以保留目标模型的分布。SpS方法在不修改目标模型或偏置样本分布的情况下显著加快了抽样延迟。

## 介绍
- 背景和动机：作者强调，尽管大型语言模型在各种任务上表现出了改进的性能，但解码过程仍然具有计算成本。
- 方法：引入投机采样作为一种方法，通过生成多个草稿标记来加速Transformer解码，该方法结合了一个草稿模型和一个目标模型以实现这种加速。。

## 方法
1. 生成一个较小的草稿模型，每次生成k个draft token。

2. 使用较大的目标模型将这k个token拼接到输入里面推理一个token，这样可以把k个draft token验证一次

3. 如果当前token不可接受，后边也都是不可信的，如果k个token都被接受，那么大模型推理的第k+1个token也被接受。

无损性证明见论文附录，proofs


## 实验结果
作者使用一个针对抽样延迟进行了优化的40亿参数草稿模型对猜测抽样进行了评估。他们在基准任务上比较了猜测抽样与不同抽样方法的性能和速度，包括XSum基准和HumanEval任务。结果显示，猜测抽样在不损害样本质量的情况下实现了显著的加速。


<img width="730" alt="image" src="https://github.com/zgMin/Paper_Reading/assets/52092775/db32462f-8f4e-477b-aa17-1177537e9378">



## 结论
作者总结称，投机采样是减少大型语言模型抽样延迟的有效且实际方法。该方法在不修改目标模型或偏置样本分布的情况下实现了显著的加速。他们强调了保持目标模型分布的重要性，并强调了投机采样在各个领域的潜在应用。


## 个人观点
- 投机采样使用draft和target的两个模型结构，实现了自回归解码的无损加速方法。
- 该方法有效的一个关键在于draft的解码时间远远小于target，被接受的draft token所节省的时间远远大于draft解码的时间。
- 额外的draft模型是否有必要？按照人写草稿的思路，通常草稿与最终的写作都是由一个人完成。
