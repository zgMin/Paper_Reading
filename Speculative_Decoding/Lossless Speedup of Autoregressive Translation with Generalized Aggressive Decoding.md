# Lossless Speedup of Autoregressive Translation with Generalized Aggressive Decoding
- **作者**: Heming Xia, Tao Ge, Furu Wei, Zhifang Sui
- **出版年份**: 2022
- **期刊/会议**: arXiv preprint arXiv

## 摘要
论文提出了一种名为"Generalized Aggressive Decoding (GAD)"的新解码方法，通过结合自回归和非自回归翻译，实现了自动翻译的高效加速，同时保持翻译质量。GAD的核心思想是使用非自回归翻译(NAT)进行快速起草，然后通过自回归方式验证，仅接受通过验证的部分结果。实验结果表明，GAD能够在提高翻译速度的同时保持翻译质量，具有潜力成为未来的解码范式。

## 介绍
- 背景和动机：尽管自回归模型在生成准确的修正结果方面表现出色，但其串行生成的特性导致推理速度较慢，限制了其在实际应用中的效率。
- 方法：提出了一种输入引导的非自回归机制，通过并行生成多个候选修正结果，并通过一系列策略（GAD）进行选择和修正，实现了高效的推理过程。

## 方法
为了指导起草阶段的生成过程，引入了输入引导机制（ID）。具体来说，模型会根据输入序列中的错误标记和上下文信息，生成与错误相关的候选修正结果。这种输入引导机制有助于提高修正结果的准确性和相关性。


GAD（Aggressive Decoding）和GAD++是一种用于序列到序列（seq2seq）任务的推理加速方法。

GAD是一种基于非自回归（NAR）机制的推理方法。它通过并行生成候选修正结果来加速推理过程。具体而言，GAD包括两个关键组件：NAT drafter和AT verifier。
- NAT drafter是一个非自回归模型，用于快速生成候选修正结果。
- AT verifier是一个自回归模型，用于评估候选修正结果的质量。GAD通过在NAT drafter和AT verifier之间进行迭代，生成和验证候选修正结果，最终选择最佳的修正结果作为输出。

<img width="475" alt="image" src="https://github.com/zgMin/Paper_Reading/assets/52092775/0a87f16f-f05f-4fde-b34a-3683bf32395b">


GAD++是对GAD的改进版本。与GAD不同的是，GAD++放宽了对NAR drafter生成结果的要求。具体而言，GAD++允许NAT drafter生成的结果在AT verifier的候选结果中排名靠前，但不一定与AT verifier的top-1结果完全匹配。这样可以更多地接受NAT drafter生成的结果，从而进一步提高推理速度。

<img width="459" alt="image" src="https://github.com/zgMin/Paper_Reading/assets/52092775/c0acae17-881f-4e97-8720-2190e10b19b6">


## 实验结果
在WMT14 EN-DE测试集上的实验结果显示，大多数句子相比基线的束搜索方法，使用GAD和GAD++可以实现3~6倍的加速，甚至有些罕见的情况下可以达到超过10倍的加速。

## 讨论
GAD++在WMT14 EN-DE测试集上各模块的推理时间开销。当前的简单实现中，大约有16%的时间用于（顺序地）对AR和NAR的输入进行编码，这部分时间可以明显优化。


## 结论
GAD是一种有潜力的推理加速方法，可以在不损失质量的情况下显著提高序列到序列任务的推理速度。然而，目前的实现还有改进的空间，特别是在提高AR验证器和NAR起草器的性能方面。

## 个人观点
- 和Speculative Decoding一样的结构，但这个更早。是NAT和AT的协作，还实现了并行解码来加速。GAD采用了严格的top1验证，GAD++增加一个 $\gamma $的参数作为容忍程度，放宽了验证的约束。感觉不如SpD方法的设计（有数学证明无损，且非严格约束）
- 非自回归的生成可能会导致一个较低的验证通过率
- 非自回归方法，k个draft token是独立的，第i个token不被接受，实际上不影响第i+1个token被接受，也就是说非自回归的draft在这里有较大的改进空间。


