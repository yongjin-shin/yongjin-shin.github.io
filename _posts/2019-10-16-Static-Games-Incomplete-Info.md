---
title: "Static Games with Incomplete Information"
usemathjax: true

categories:
- summary
tags:
- MAS
---

중간고사 대비 Game Theory 정리 [PDF](https://arxiv.org/abs/1706.03762)



## Bayesian Games

### Motivation

* 우리는 이제껏, 게임 자체에 대해서는 잘 알고 있었다.
  * 즉, 누가 참여하는지, 어떤 종류의 선택을 하는지, 그리고 만약 그러한 선택을 했을 때 얼마만큼 보상을 받게될지.
  * Imperfect-Information Game이라고 해도 '상대가 무엇을 선택할지'는 몰랐지만, 게임 자체에 대해서는 알고 있는 상태였다.
  * 이런 상황에서, Dominant Strategies, Rationalizability, NE, SPE 등을 알고자 하였다.
* 그러나 현실에서는 게임 자체를 모르는 경우가 허다하다.
  * 단적인 예로, 시장에서 경쟁사가 어떤 종류의 선택을 할 수 있을지 그리고 그것으로 인해 어떤 이득을 가지고 갈 수 있을지에 대해서는 전혀 알 수 없다.
  * 물론, 어느 정도의 Guess를 할 수 있을 것이다. 이러한 점은 imperfect 게임에서 belief를 통해 best reponse를 찾으려고 했던 점과 일면 유사하다고 할 수 있겠다.



### Bayesian Games

* Games of incomplete information
  * 가정) 일어날 법한 게임들에서 에이전트의 수나 Strategy Space는 동일하고, 보상(payoff)만 달라짐. 
  * 조건이 다른 게임에서는 Reformulation에서 payoff를 조절하여 조건이 동일하게 만들어줄 수 있음.
  *  Prior에 대해서는 공유하고 있음. (Each player knows the distribution of his opponents' type)



* 베이지안 게임을 표현하는데 여러가지 방법이 있음
  * Information Sets
  * Extensive form with chance moves (Nature)
  * Epistemic



* <u>Def 1)</u> Information Sets:
  * $$N$$ is a set of agents
  * $$G$$ is a set of games with $$n$$ agents each such that if $$g, g' \in G$$ then for each agent $$i \in N$$ the strategy space in $$g$$ is identical to the strategy space in $$g'$$
  * $$P \in \prod(G)$$ is a common prior over games, where $$\prod(G)$$ is the set of all probability distiributions over $$G$$
  * $$I = (I_{1}, \cdots , I_{N})$$ is a tubple of partitions of $$G$$, one for each agent.



* Def 2) Extensive form with chance moves (Nature)
  * 보상이 필요없는 전지전능한 자연이 랜덤하게 상대방 에이전트의 속성을 결정한다. 
  * 나는 상대방의 속성을 알지 못한채 게임에 돌입한다. 하지만 어떤 선택을 했을 때, 어떤 action과 payoff가 생길지에 대한 정보는 가지고 있다. (중요한 부분. common prior가 있어야만 분석이 가능하다!-NE, SPE를 찾을 수 있다)
  * 따라서, imperfect information 게임으로 변했다.



* <u>Def 3)</u> Epistemic : A way of defining uncertainty directly over a game's utility function
  * $$N$$ is a set of agents
  * $$A=A_1 \times \cdots \times A_n$$, where $$A_i$$ is the set of actions available to player $$i$$
  * $$\Theta = \Theta_1 \times \cdots \times \Theta_n$$, where $$\Theta_n$$ is the type space of player $$i$$
  * $$p:\Theta \rigtharrow [0, 1]$$ is a common prior over types
  * $$u=(u_1, \cdots, u_n)$$ where $$u_i: A \times \Theta_i \rightarrow \mathbb{R}$$ or $$u_{i}: A \timse \Theta \rightarrow \mathbb{R}$$ is the utility function of player $$i$$, which is type dependent
    * Common prior가 주어진다면 (joint probability), 내가 어떤 타입인지를 알면 상대방이 어떤 타입일지 posterior 확률을 알 수 있다.



### Expected Utility

* ex-post: 나도 알고 상대방의 타입도 알고
  * $$\mathbb{E}[U_{i}(s, \theta)] = \sum_{a \in A} \big( \prod_{j \in N} s_{j}(a_{j}|\theta_{j}) \big) u_{i}(a, \theta)$$

* ex-interim: 나는 알지만 상대방의 타입에 대해서는 모르는
  * $$\mathbb{E}[U_{i}(s, \theta_{i})] = \sum_{\theta_{-i} \in \Theta_{-i}} p(\theta_{-i}|\theta_{i}) \sum_{a \in A} \big( \prod_{j \in N} s_{j}(a_{j}|\theta_{j}) \big) u_{i}(a, \theta_{-i}, \theta_{i})$$
  * $$\mathbb{E}[U_{i}(s, \theta_{i})] = \sum_{\theta_{-i} \in \Theta_{-i}} p(\theta_{-i}|\theta_{i}) \mathbb{E}[U_{i}(s, (\theta_{-i}, \theta_{i}))]$$
* ex-ante: 나의 타입도 모르고 상대방에 대해서도 모르고
  * 수식 쓰기 귀찮으니깐... 모든 $$\Theta$$에 대해서 ex-post' weight sum을 하면 결과가 나오고, 혹은 특정 에이전트 $$\theta_i$$에 대해서 ex-interim' weight sum을 한 결과와도 동일하다.



### Bayesian Nash Equilibrium

* Best Response는 이전과 동일한 개념. 
  * 남들이 어떤 행동을 할 때, 내 이득을 최대화할 수 있는 전략. 
  * 여기서 다른 점은 Expected Utility를 쓴다는 점.
  * 그리고 argmax를 구하지만 동일한 값이 나올 수 있는 set으로 존재할 수 있다는 점.
* BNE (Bayesian Nash Equilibrium)
  * 거의 동일하게 구한다. 다만, ex-interim으로 계산을 해주는데 (즉, 특정 타입이라고 했을 때의 Nash를 구해준다) 이유는 다른 타입에 있을 때에 어떤 선택을 하는지는 서로 영향을 미치지 않기 때문이다.
  * 즉, 모든 에이전트에 대해서, 그리고 각각의 타입들에 대해서 Best Response가 바로 BNE가 된다.

