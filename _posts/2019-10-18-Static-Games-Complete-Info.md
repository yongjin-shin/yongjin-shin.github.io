---
title: "Static Games with Complete Information"
usemathjax: true

categories:
- summary
tags:
- MAS
---

중간고사 대비 Game Theory 정리 [PDF](https://arxiv.org/abs/1706.03762)



## Games in Normal Form

### Normal Form Game

* Finite, n-person normal form game: $$G(N, A, \mu)$$:
  * N = {$$1, \cdots, n$$} is a finite set of $$n$$, indexed by $$i$$
  * $$A = A_1 \times \cdots \times A_n$$
  * $$\mu = (\mu_1, \cdots, \mu_{n})$$
* 동시에 패를 꺼내놓는다. 상대가 무엇을 내놓을지는 모르는 상태.
  * 그러나 모든 정보는 공개되어 있다. 
  * 다만 이번 한번이 처음이자 마지막 게임이고, 
  * 상대가 무엇을 할지는 전혀 모른다.

* Expected utility of mixed strategy
  * Given a normal-form game, 
    * $$\mathbb{E}[\mu_{i}] = \sum_{a\in A}u_{i}(a)p(a|s)$$
    * $$p(a|s) = \prod_{j=1}^{n}s_{j}(a_j) = s_1(a_1) \times \cdots \times s_n(a_n)$$



## Analyzing Games

### Solution Concepts

* **Pareto Optimality**
* **Nash Equilibrium**
* Maximin and minimax strategies
* Minimax regret
* Correlated equilibrium



### Pareto Optimality

* **Pareto Dominate**: strategy $$s$$는 모든 에이전트에 대해서 다른 strategy $$s'$$보다 우월하다. $$\forall i, u_{i}(s) \geq u_{i}(s')$$
  * 이 전략에서 다른 에이전트들도 모두 happy 해야함
* **Pareto Optimality**: Pareto Dominant Strategy가 오직 유일할 때.



### Nash Equilibrium

* **Best Response**: $$i$$'s mixed strategy $$s^*_{i} \in S_{i}$$ s.t. $$u_{i}(s^*_{i}, s_{-i}) \geq u_{i}(s_{i}, s_{-i})$$ for all strategies $$s_i \in S_i$$
  * Pure Strategy가 아닌 이상, BR은 무수히 많다.

* **Nash Equilibrium**: $$s^{*}=(s^{*}_{1}, \cdots, s^{*}_{n})$$ is NE if $$\forall i, \forall s_{i}, u_{i}(s^{*}_{i}, s^{*}_{-i}) \geq u_{i}(s_{i}, s^{*}_{-i})$$
  * 모든 에이전트들이 각자가 Best Response를 가지고 있을 때를 말함.
  * Mixed의 경우에는, best response가 expected value와 동일해야하므로 $$a_i, a_j$$의 value가 동일해야함. 즉,
    * $$u_{i}(a_{i}, s^{*}_{-i}) = u_{i}(a_j, s^{*}_{-i}) = u_{i}(s^{*}_{i}, s^{*}_{-i})$$



### Pareto Optimality vs Nash Equilibrium

* Nash Equilibrium은 개인의 이해타산을 최대한 만족시키는 결과이고,
* Pareto Optimality는 전체 집단의 이해타산을 최대한 만족시키는 결과임.
* 따라서, Nash Equilibrium이 Pareto Optimality가 되지 않는 경우가 존재한다는 것은
  * 개인의 이해타산만을 추구하다가 모두에게 안 좋은 결과가 발생한다는 것
  * '보이지 않는 손'이 모든 것을 해결해 주지 않는다.



## Further Solution Concepts

### Solution Concepts

* Pareto Optimality
* Nash Equilibrium
* **Maximin and minimax strategies**
* **Minimax regret**
* **Correlated equilibrium**



### Maxmin and minmax

 * Maxmin: for player $$i$$, $$s^{*}_{i} = argmax_{s_i}min_{s_{-i}} u_{i}(s_{i}, s_{-i})$$ 
   	* 상대가 뭔짓을 하든 내가 지닐 수 있는 가장 큰 값.
 * Minimax: for player $$i$$, $$s^{*}_{i} = argmin_{s_{-i}}max_{s_{i}} u_{i}(s_{i}, s_{-i})$$ 
   	* 내가 무엇을하든 상관없이 상대의 행동으로 내가 가질 수 있는 최소한의 값.
 * Minmax Theorem
   	* 2명이서 제로섬 게임을 할 때,
   	* maxmin value = minmax value = Nash equilibrium value



### Minimax Regret

* Regret : $$\big[ max_{a'_{i} \in A_{i}}u_{i}(a'_{i}, a_{-i}) \big] - u_{i}(a_{i}, a_{-i})$$
* Max Regret : $$max_{a_{-i} \in A_{-i}}\Big( \big[ max_{a'_{i} \in A_{i}}u_{i}(a'_{i}, a_{-i}) \big] - u_{i}(a_{i}, a_{-i}) \Big)$$
  * 만약 $$i$$가 $$a_i$$를 행동했을 때, 얻게 되는 가장 큰 기회 비용.
* Minmax Regret : $$argmin_{a_{i} \in A_{i}} \Big[ max_{a_{-i} \in A_{-i}}\Big( \big[ max_{a'_{i} \in A_{i}}u_{i}(a'_{i}, a_{-i}) \big] - u_{i}(a_{i}, a_{-i}) \Big) \Big]$$
  * 최악의 케이스에서 기회비용을 가장 최소화할 수 있는 전략.



### Correlated Equilibrium

* 



## Computing Solution Concepts



