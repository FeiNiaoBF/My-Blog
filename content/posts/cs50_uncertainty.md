---
title: "Uncertainty --- Probability"
date: 2024-03-10T19:39:26+08:00
draft: true
taps: ['AI', 'cs50', 'probability']
categories: ['AI', 'cs50']
author: ["Yeelight"]
showtoc: true
weight:
---

# Probability - 概率论

我们的世界是一个 Uncertainty world ，每一件事都有自己的概率，而对于 AI 的世界来说概率是很重要的事情，比如，AI 需要根据已知且有一定概率的事件来推测未知或想了解的另一件事，因此概率对于人工智能和机器学习是很重要的。

## 基础概率学

首先要了解一下基础的概率知识，其实用中学学习到知识足以学习之后的知识了，我这里做一个大概的复习。

### Axioms in Probability

在概率中我们以 `P` 来表示概率的值（probability value）, 而我们公认的概率值必须介于 0 和 1 之间，即：
$$ 0 < P(ω) < 1 $$
且满足：
$$ \sum_{\omega\in\infty} P(ω) = 1 $$


其中 0 是不可能事件， 1 是必然事件

### Unconditional Probability 独立概率

我们常常听见*古典概率* ，即“事件中，每一个样本点的概率相等”，比如说我掷骰子是 6 的概率，很明显 `p (骰子是 6) = 1/6` ，拿第二次的是 6 的概率呢？也是 `p (第二次骰子是 6) = 1/6` 而这其中的"隐性条件"是两次掷都互不影响，**无条件概率**是在没有任何其他（前提）证据的情况下对某个命题的相信程度。因为掷骰子的结果不依赖于之前的事件。

### Conditional Probability 条件概率

条件概率和无条件概率相反，即：在给定一些已经知晓（前提）证据情况下对某个命题的相信程度，换句话说可以根据已知事件概率推测未知事件概率，AI 需要这个功能。

条件概率使用以下符号表示：`P(a | b)`，意思是“在我们知道事件 b 发生的情况下，事件 a 发生的概率”，或者更简洁地说，“a 在给定 b 的概率”。
而为了计算 `P (a | b)` 我们有以下的数学公式：

$$P(A|B)=\frac {P(A∧B)}{P(B)}$$
换句话说，给定 b 为真的概率等于 `a ∧ b` 为真的概率除以 b 的概率。

### Random Variables 随机变量

随机变量是概率论中的变量，具有可能取值的域。

### Independent Events 独立事件

独立性是指一个事件的发生不会影响另一事件发生的概率。例如，当掷两个骰子时，每个骰子的结果彼此独立。用第一个骰子掷出 4 不会影响我们掷出的第二个骰子的值。
独立性可以用数学方式定义：事件 a 和 b 是独立的，当且仅当 a 和 b 的概率等于 a 的概率乘以 b 的概率：

$$
 P(a ∧ b) = P(a)P(b)
$$

### Bayes’ Rule  贝叶斯法则*

他很重要，在概率中最重要的公式之一：贝叶斯定理---这个公式是科学发现的核心，它是机器学习和人工智能的核心工具。
推荐一个视频
[贝叶斯定理，改变信念的几何学](https://youtu.be/HZGCoVF3YvM)
他具体的数学公式为：
$$
P(B|A)=\frac{P(B)P(A|B)}{P(A)}
$$
通常，事件 A 在事件 B 已发生的条件下发生的概率，与事件 B 在事件 A 已发生的条件下发生的概率是不一样的。然而，这两者是有确定的关系的，贝叶斯定理就是这种关系的陈述。贝叶斯公式的一个用途，即透过已知的三个概率而推出第四个概率。
按这些术语，贝叶斯定理可表述为：

$$后验概率 = (似然性 * 先验概率)/标准化常量$$
### Joint Probability 联合概率

联合概率是指两个（`X & Y`）或更多事件同时发生的可能性。它是用来计算两个事件同时发生的概率，通常用于解决诸如以下问题：

- 在两次抛硬币中都出现正面的概率是多少？
- 连续从一副牌中抽两张A的概率是多少？
- ...

统计学家使用符号 P(A ∩ B) 来表示事件“A”和“B”同时发生的联合概率。例如，P(F ∩ Mac) 表示女性购买 Mac 的可能性。这个交集指定了两个或更多事件如何同时发生。因此，联合概率也被称为事件的交集。

对于概率的复习就到此暂停，以下是在学习 AI 的时候使用的数据结构或算法。
### Bayesian Networks 贝叶斯网络

贝叶斯网络是一种表示随机变量之间依赖关系的数据结构。贝叶斯网络具有以下属性：
- They are **directed graphs**.
- Each node on the graph represent a **random variable**.
- An arrow from X to Y represents that X is a parent of Y. That is, the probability distribution of Y **depends** on the value of X.
- Each node X has probability distribution P(_X | Parents(X)_).

### Sampling  采样

抽样是一种近似推理技术。在采样中，每个变量根据其概率分布被采样为一个值。

一个例子：如果现在我们可以多次掷骰子并记录每次得到的值。假设我们掷骰子 600 次（可以更多）。我们计算得到 `1` 的次数（应该大约是 100），然后对其余值（2-6）重复。然后，我们将每个计数除以总数（600），之后每个值出现的概率大概为 `1/6` 的结果，我们会得到接近它的近似概率值。

在代码中，采样函数可以类似于 `generate_sample` ：

```python
import pomegranate
from collections import Counter
from model import model

def generate_sample():

    # Mapping of random variable name to sample generated
    sample = {}

    # Mapping of distribution to sample generated
    parents = {}

    # Loop over all states, assuming topological order
    for state in model.states:

        # If we have a non-root node, sample conditional on parents
	if isinstance(state.distribution,
		          pomegranate.ConditionalProbabilityTable):
            sample[state.name] =
            state.distribution.sample(parent_values=parents)
        # Otherwise, just sample from the distribution alone
        else:
            sample[state.name] = state.distribution.sample()
        # Keep track of the sampled value in the parents mapping
        parents[state.distribution] = sample[state.name]
    # Return generated sample
    return sample
```

### Markov Models  马尔可夫模型

马尔可夫假设是当前状态仅取决于有限固定数量的先前状态的假设。这对我们很重要。比方说预测天气。理论上，我们可以使用过去一年的所有数据来预测明天的天气。然而，这是不可行的，这既是需要高负荷的计算能力，也是因为可能没有基于 365 天前的天气而对于明天天气的信息。使用马尔可夫假设，我们限制了之前的状态（例如，在预测明天的天气时我们将考虑之前的几天），从而使任务易于管理。这意味着我们可能会得到感兴趣概率的更粗略的近似值，但这通常足以满足我们的需求。此外，我们可以使用基于最后一个事件的信息的马尔可夫模型（例如，根据今天的天气预测明天的天气）。

对于马尔可夫模型还有许多的内容，我还没学到，之后学到了我会记录下来的

## 外部连接

[cs51ai/uncertainty](https://cs50.harvard.edu/ai/2024/notes/2/)

[谭升的博客](https://face2ai.com/Math-Probability-2-3-Bayes-Teorem/)

[Bayesian Networks](https://www.bayesserver.com/docs/introduction/bayesian-networks/)

[联合分布](https://zh.wikipedia.org/wiki/%E8%81%94%E5%90%88%E5%88%86%E5%B8%83)
