---
title: Reinforcement Learning(2)——MDPs
copyright: true
date: 2018-06-10 17:07:42
categories:
	- 强化学习
tags:
	- 强化学习
	- 马尔科夫决策过程
	- Q-learning
	- 值迭代
	- 策略迭代
---
 上一篇文章[强化学习——简介](https://icesuns.github.io/2018/06/03/reinforcement-learning-1/)简单介绍了一下强化学习的相关概念。这篇博客将引入 __马尔科夫决策过程(Markov Decision Processes, MDPs)__对强化学习进行建模。这篇文章，将对马尔科夫决策过程以及__Q-leaning__进行介绍。

## 马尔科夫过程

>__定义:__ 若随机过程 $ \left \lbrace X_n, n \in T \right \rbrace$对于任意非负正整数$n \in T$和任意的状态$i_0, i_1, ..., i_n \in I$, 其概率满足$$P \left\lbrace X_{n+1}=i_{n+1} \mid X_0=i_0, X_1=i_1,...,X_n=i_n \right\rbrace =P \left\lbrace X_{n+1}=i_{n+1} \mid X_n=i_n \right\rbrace$$则称$ \left \lbrace X_n, n \in T \right \rbrace$是马尔科夫过程。

 马尔科夫过程的一大特点是 __无后效性__ ,当一个随机过程在给定现在状态和过去状态情况下，其未来状态的条件概率仅依赖于当前状态。在给定现在状态是，随机过程与过去状态是条件独立的。也就是说，系统的下个状态只与当前状态有关，与更早之前的状态无关，一旦知道当前状态之后，过去的状态信息就可以抛弃。

马尔科夫过程还有一个重要的元素，那就是 __转移概率矩阵__。
$P_{ij}(n) =P \left\lbrace X_{n+1}=j \mid X_n=i \right\rbrace$，指的时刻$n$，从状态$i$转移到状态$j$的一步转移概率。当$P_{ij}(n)$y与$n$无关是，则称马尔科夫链$ \left \lbrace X_n, n \in T \right \rbrace$是齐次的。

$P=\begin {bmatrix}
{p_{11}}&{p_{12}}&{\cdots}&{p_{1n}}\\\
{p_{21}}&{p_{22}}&{\cdots}&{p_{2n}}\\\
{\vdots}&{\vdots}&{\ddots}&{\vdots}\\\
{p_{n1}}&{p_{n2}}&{\cdots}&{p_{nn}}
\end {bmatrix}$；其中，${p_{ij}\geq 0}$，$\sum_{j \in I} p_{ij} = 1$
因此，定义一个马尔科夫过程，需要给出有限状态集合$\\{ x_i \in X, i \in N \\}$以及转移概率矩阵$P$。
下图是一个马尔科夫链转移矩阵的例子。
![马尔科夫链](https://ws1.sinaimg.cn/large/e89d48b8ly1fs6c41vb5xj20lo08e0u0.jpg)


## 马尔科夫决策过程 
[上一篇文章](https://icesuns.github.io/2018/06/03/reinforcement-learning-1/)介绍了强化学习是一个与不断进行环境交互学习只能agent的学习过程。而MDPs正是立足于agent与环境的直接交互，只考虑离散时间，假设agent与环境的交互可分解为一系列阶段，每个阶段由“感知——决策——行动”构成。如下图：![agent与环境交互](https://ws1.sinaimg.cn/large/e89d48b8ly1fs6bqo0b66j20g506fjrf.jpg)
MDP模型是一个四元组$(S,A,T,R)$






> [1] Hexo编辑数学公式： http://bennychen.me/mathjax.html