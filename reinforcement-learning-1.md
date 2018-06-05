---
title: Reinforcement Learning(1)
copyright: true
date: 2018-06-03 20:14:45
categories:
	- 强化学习
tags: 强化学习
---
 开始写强化学习这个系列的文章，主要是为了记录学习过程中学习到的知识点，分享自己的理解。

## 强化学习简介
**强化学习**（英语：Reinforcement learning，简称RL）是机器学习中的一个领域，强调如何基于环境而行动，以取得最大化的预期利益。其灵感来源于心理学中的行为主义理论，即有机体如何在环境给予的奖励或惩罚的刺激下，逐步形成对刺激的预期，产生能获得最大利益的习惯性行为。

强化学习涉及很多学科，其中包括计算机、神经科学、工程学科等，详细可见下图。
![图1](https://ws1.sinaimg.cn/large/e89d48b8ly1fryb73c4srj20ao09tjsv.jpg)
机器学习大致可以分为强化学习、监督学习以及非监督学习这三种，这三者之间的关系如下图所示。
![image](https://ws1.sinaimg.cn/large/e89d48b8ly1fryb737cbyj209s096dgr.jpg)
强化学习属于机器学习中的一种，与我们熟悉的深度学习有一些区别。强化学习最大的特点就是能够与环境进行交互，通过交互学习到最优的方法解决问题。强化学习和标准的监督式学习之间的区别在于，它并不需要出现正确的输入/输出对，也不需要精确校正次优化的行为。强化学习更加专注于在线规划，需要在探索（在未知的领域）和遵从（现有知识）之间找到平衡。

最直观的方式去区分这三种学习方法的特征就是数据的形式：
> - 监督学习：数据带有明确的标签，这些标签可以看做是一个supervisor，指导学习数据之间的关系。常见应用有的分类、回归等
> - 非监督学习：数据没有标签，算法学习数据特征之间的内部关系。常见有聚类。
> - 强化学习： 数据没有标签，在强化学习系统中，agent感知环境变化，接受每一次行为产生的奖励(reward)，决策下一步的行为。

**强化学习的特点：**
> - 正如上面所说的，强化学习没有supervisor，只有一个每次行为之后产生的奖励reward
> - 环境对行为的反馈存在延迟，不是即时的
> - 时间的影响很大，系统是一个连续的变化过程
> - agent的行为会影响下一次接受的数据。

## 强化学习系统
![image](https://ws1.sinaimg.cn/large/e89d48b8ly1fryw9zeidmj208y077q2w.jpg)
上图为强化学习(RL)系统的原理图。用马尔科夫决策过程(Markov Decision Process, MDP)对RL问题进行建模。通常将MDP定义为一个四元组_(S,A,ρ,f)_
![image](https://ws1.sinaimg.cn/large/e89d48b8ly1frywhywjz8j20fb08zjs6.jpg)

### 强化学习四个元素
 1. 策略（Policy）
 policy是agent的一系列行为，也是状态到行为的映射。Policy可以分为两类，一个是确定性策略（deterministic policy）**a = π(s)**即状态与策略之间的关系是确定的，当系统处于某种状态，必然有一个策略与之对应。另一列是随机策略（Stochastic policy）**π(a∣s) = Ρ[At =a ∣St = s]**,这是在一个状态下，对应于不同策略的概率

 2. 值函数（value function）
 值函数是对未来奖励预测的函数，定义的是在状态s下，采取策略π的长期奖励

 3. 奖励信号（a reward signal）
 Reward就是一个标量值，是每个time step中环境根据agent的行为返回给agent的信号，reward定义了在该情景下执行该行为的好坏，agent可以根据reward来调整自己的policy。常用R来表示。
 4. 环境模型（a model of the environment），预测environment下一步会做出什么样的改变，从而预测agent接收到的状态或者reward是什么。


### agent与environment
![image](https://ws1.sinaimg.cn/large/e89d48b8ly1fryxi3r8lhj20f807wq4x.jpg)
上图是agent与environment之间的交互。
现在来介绍一个关于agent和environment之间的一些概念。
 1. 历史（History） 与 状态（State）
 > - history是observations、actions以及rewards的序列。下一步要发生的事件要依赖于history。
 > - state是决策下一步行动的信息，通常是一个history的函数。

 2. environment state、 agent state、 information state
 > - enviroment state是环境的私有状态，通常对agent是不可见的
 > - agent state是agent的内部表现状态，比如agent决策下一步行动所选用的信息等。是历史的一个函数。
 > - information state包含来自于历史的所有有用信息。

 3. fully observable environments 和 partially obervable environments
 > - full observability, agent能够直接观察到环境的变化状态，是一个马尔科夫决策过程
 > - partial observability, agent只能简洁观察环境，这是一个部分马尔科夫决策过程（partially observable Markov decision process，POMDP）。代理必须根据其他的信息，建模自己的状态模型。


### RL问题中agent的分类
![image](https://ws1.sinaimg.cn/large/e89d48b8ly1fryy5gi0r1j20e00bzt8u.jpg)




