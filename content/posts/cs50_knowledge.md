---
title: "Knowledge --- Boolean logic"
date: 2024-03-10T19:39:10+08:00
draft: true
taps: ['AI', 'cs50']
categories: ['AI', 'cs50']
author: ["Yeelight"]
showtoc: true
weight:
---


> How AI can use logic to reach to new conclusions based on existing information.
>
## Propositional logic

Propositional logic is based on propositions, statements about the world that can be either true or false.

### *Logical Connectives*

- **NOT(¬)**
	Inverses the truth value of the proposition

|  P  | ¬P  |
|:---:|:---:|
|  T  |  F  |
|  F  |  T  |

- **And (∧)**
	Connects two different propositions.

|  P  |  Q  | P∧Q |
|:---:|:---:|:---:|
|  T  |  T  |  T  |
|  T  |  F  |  F  |
|  F  |  T  |  F  |
|  F  |  F  |  F  |

- **Or (∨)**
	Return true if and only if exactly one of its arguments is true

|  P  |  Q  | P∨Q |
|:---:|:---:|:---:|
|  T  |  T  |  T  |
|  T  |  F  |  T  |
|  F  |  T  |  T  |
|  F  |  F  |  F  |

- **Implication (→)**

	In the case of P implies Q (P → Q), P is called the *antecedent* and Q is called the *consequent*.
	技巧：有 P 就有 Q，没 P 不定 Q。
	它更像是必要条件和充分条件的关系

|  P  |  Q  | P→Q |
|:---:|:---:|:---:|
|  T  |  T  |  T  |
|  T  |  F  |  F  |
|  F  |  T  |  T  |
|  F  |  F  |  T  |

- **Biconditional (↔)**
	known as material biconditional or equivalence or biimplication or bientailment
	技巧：P if and only if Q
	当且仅当

|  P  |  Q  | P↔Q |
|:---:|:---:|:---:|
|  T  |  T  |  T  |
|  T  |  F  |  F  |
|  F  |  T  |  F  |
|  F  |  F  |  T  |

- **Entailment (⊨)**
	如果 α ⊨ β （α 意味着 β），那么在任何 α 为真的世界中，β 也为真。

### *Model World*

对于 **model** ，这是一个对每个 proposition 的数学模型，如：P-> "正在下雨"、Q->"今天是星期天"。则两个命题对应的 model ：
- {P: True, Q: True}
- {P: True, Q: False}
- {P: False, Q: True}
- {P: False, Q: False}

由此可以看出模型的数量是命题数量的 2 次方 **N²**

### Knowledge Base (KB)*
>
> The knowledge base is a set of sentences known by a knowledge-based agent.

知识库（Knowledge base）是用于知识管理的一种特殊的资料库，便于于有关领域知识的采集、整理以及提取。在这里我们是为以命题逻辑句子的形式提供的关于世界的知识，可用于*人工智能*对世界做出额外的推论。知识库中的知识来源于各**领域专家**、**定理**、**常识**等，它是自动化问题所需领域知识的集合，包括基本事实、规则和其他相关信息。

换言之，在 KB 中的所有知识都是 `True` 的

## Inference
>
> Inference is the process of deriving new sentences from old ones.
>
### **Model Checking algorithm**

1. 首先，我们将考虑 **what** 推断新知识
2. 其次，我们将考虑 **why** 从旧的知识里面
3. 最后，我们要分析一下 **how** 做算法

**What:**
AI 需要先确定 KB ⊨ α（new knowledge） 是否成立（换句话说，回答问题：“我们能否根据我们的知识库得出 α 为真的结论”）
- Enumerate all possible models.
	枚举所有可能的模型。
- If in every model where KB is true, α is true as well, then KB entails α (KB ⊨ α).
	如果在每个 KB 为真的模型中，α 也为真，则 KB 蕴含 α (KB ⊨ α)。
**Why:**
AI 的知识库是不断学习的，除了我们人为的更新 AI 需要自己更新，这是我们的目标，知识库将用于推断

**How:**

```python
def check_all(knowledge, query, symbols, model):
    if not symbols:
        # If knowledge base is true in model, then query must also be true
        if knowledge.evaluate(model):
            return query.evaluate(model)
        return True
    else:

        # Choose one of the remaining unused symbols
        remaining = symbols.copy()
        p = remaining.pop()

        # Create a model where the symbol is true
        model_true = model.copy()
        model_true[p] = True

        # Create a model where the symbol is false
        model_false = model.copy()
        model_false[p] = False

        # Ensure entailment holds in both models
        return(check_all(knowledge, query, remaining, model_true) and check_all(knowledge, query, remaining, model_false))
```

以上代码来自 CS50ai/1.knowledge

## **Knowledge Engineering**

Knowledge engineering is the process of figuring out how to represent propositions and logic in AI.
> 知识工程是弄清楚如何在人工智能中表示命题和逻辑的过程。

有个警察抓了 A、B、C、D 四名偷窃嫌疑犯, 但其中只有一人是真正的小偷的游戏，其中里面就是有固定的信息（每个人的论述）通过信息可以找到真正的小偷吗？
这是我小时候喜欢的谜题，很有趣，现在很少听说了
例：有 A，B 两个人

>A：我们都是小偷
>B：我不是小偷

Q：谁是小偷？
A：A 是小偷。

Why？
假设 A 是正确的，则根据我们已知的，我们要推断 KB⊨{A: True}
If A is true, 那么不满足 KB 里面的只有一个小偷
then A is false, 那么我们就可以推断出 B 是 True，而 A 是 False

其实，在 AI 进行推测的时候它会把新的推测加入知识库里面

## **Inference Rules**

### 算法过程

- initial state : starting knowledge base
- actions: inference rules
- transition model: new knowledge base after inference
- goal test: check statement we're trying to prove
- path cost function: number of steps in proof

### Rules

对于 AI 的推理我们需要保证高效和准确，因此我们需要用到在学命题逻辑的一些规则，以使 AI 拥有更高效的推理算法。

- **Modus Ponens** （肯定前件）
- **And Elimination**（合取消除）
- **Double Negation Elimination** （双重否定消除）
- **Implication Elimination** （蕴含消除）
- **Biconditional Elimination** （双条件消除）
- **De Morgan’s Law** （德摩根定律）
- **Distributive Property** （分配律）

感谢先人的努力和不断探索，使我们可以直接用这些 Rules

### First Order Logic 一阶逻辑

一阶逻辑是另一种类型的逻辑，它允许我们比命题逻辑更简洁地表达更复杂的想法。一阶逻辑使用两种类型的符号：**常量符号**和**谓词符号**。常量符号表示对象，而谓词符号就像带有参数并返回 true 或 false 值的关系或函数。

- **Universal Quantification 通用量化**
	>∃x P (x)
- **Existential Quantification 存在量化**
	>∀x P (x)
	>
## 外部链接

[cs50ai/knowledge](https://cs50.harvard.edu/ai/2024/notes/1/)

[推理規則](https://zh.wikipedia.org/wiki/Category:%E6%8E%A8%E7%90%86%E8%A7%84%E5%88%99)

[一阶逻辑](https://zh.wikipedia.org/wiki/%E4%B8%80%E9%98%B6%E9%80%BB%E8%BE%91)
