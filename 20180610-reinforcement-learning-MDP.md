---
title: Reinforcement Learning(2)——MDPs
copyright: true
date: 2018-06-10 17:07:42
categories:
	- 强化学习
tags:
	- 强化学习
	- 马尔科夫决策过程
	- 值函数
	- value function
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
不同资料对于MDPs定义不同。在这里，我们将MDPs模型定义为一个四元组$(\cal S,A,P,R,\gamma )$：
> 
> + $\cal S$ 是一个有限集，其中每个元素$s\in S$代表一个状态
> + $\cal A$ 是一个有限集，其中每个元素$a \in A$代表一个行动
> + $\cal P$ 代表状态之间的转移矩阵, $\cal P^a_{ss'}=\mathbb{P}[S_{t+1} = s'\mid S_t=s, A_t=a]$
> + $\cal R$ 是reward函数，$\cal R^a_s = \mathbb{E}[R_{t+1} \mid S_t=s, A_t=a]$
> + $\gamma$ 是discount系数， $\gamma \in (0,1)$。

上述定义的是完全观察的马尔科夫决策过程，定义了environment-agent之间的关系。其实这很像是在模拟人或者动物与环境之间的交互过程。agent根据environment的状态作出决策，选择相应的行动action。行动对环境产生影响，改变环境的状态state，对于这个状态的改变，环境作出评估并给出奖励reward。

马尔科夫决策过程能够刻画强化学习，我们对我们的问题建模之后，该考虑agent与environment如何交互，agent怎么选择下一步行动。在这之前，我们需要对一下几点知识做一些了解。
### 策略（Policy）
策略的 __定义__：
> $$\pi (a \mid s)=\mathbb{P}[A_t=a \mid S_t=s]$$

策略很通俗地讲，其实就是在状态$s$下，选择行动$a$的概率。强化学习的本质其实就是要去学习一个最优的策略。策略定义了agent的行为，马尔科夫决策过程策略依赖于当前的状态。

### 值函数（Value Function）
在介绍值函数之前，需要介绍一下reward。看上图，可以知道agent选择一个行动之后，会对环境产生影响，环境会对这个行动进行评估，量化该行动在该状态下的好坏程度，是该行为在该状态下的即刻奖励。
令$$G_t = R_{t+1} + \gamma R_{t+2} + ... = \sum_{k=0}^\infty \gamma_kR_{t+k+1}$$$G_t$是t时刻，到未来k个阶段的“折扣”奖励和，是一个长期的影响。其中$\
gamma \in [0,1]$,当$\gamma \rightarrow  0$时，agent关注与短期影响，当$\gamma \rightarrow 1$时，agent关注与长期的影响。

这跟人和动物在做决策的时候一样，不能仅仅看到短期的利益，还应该目光长远。因此马尔科夫决策过程具有一个奖励延迟的特点。

#### state-value function
马尔科夫决策过程state-value function $v_{\pi}(s)$是从t时刻在状态s下，按照策略$\pi$执行，得到reward的期望。
$$v_{\pi}(s) = \mathbb{E}_{\pi}[G_t \mid S_t = s]$$

#### action-value function
马尔科夫决策过程action-value function $q_{\pi}(s,a)$是从时刻t，状态s下采取a行动，并按照策略$\pi$执行决策的reward的期望。
$$q_{\pi}(s, a) = \mathbb{E}_{\pi}[G_t \mid S_t = s, A_t = a]$$


本次，介绍了一下马尔科夫链以及马尔科夫决策过程，以及值函数。在下篇文章中，将对Bellman方程进行介绍。并且将会引入动态规划的方法对马尔科夫决策过程进行优化。
## 参考资料
> [1] Hexo编辑数学公式：
+ http://bennychen.me/mathjax.html
+ https://wdxtub.com/2016/03/26/latex-notation-table/

> [2] 马尔科夫决策过程： https://www.cnblogs.com/jinxulin/p/3517377.html
> [3] 马尔科夫链： https://www.cnblogs.com/pinard/p/6632399.html
 