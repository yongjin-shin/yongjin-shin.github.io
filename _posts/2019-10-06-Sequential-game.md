---
title: "Sequential Games with Complete Information"
usemathjax: true

categories:
- summary
tags:
- MAS
---

중간고사 대비 Game Theory 정리 [PDF](https://arxiv.org/abs/1706.03762)



## Perfect Information Extensive-Form Game

### Motivation



### Subgame Perfection

* SPE(Subgame perfect equilibria)
  : 모든 strategy profile $$S$$ 중에서 Graph $$G$$의 subgraph $$G'$$에 해당하는 $$s'$$ 역시 Nash Equilibrium일 때, 이러한 $$s$$ 를 SPE로 지칭한다. Nash equilibrium보다 더 가혹한 조건이다.

  * 말하자면, Subgraph를 normal form으로 나타내었을 때, 여기서의 NE가 전체 Graph의 NE에 포함되어야 한다는 말.
  * 주의사항은 Subgraph는 항상 완전한 Normal Form으로 나타내어진다는 것!

  → 근데 이게 왜 중요할까?

  

* Backward induction
  : Depth-First Traveral 알고리즘으로 찾아나가면 된다. 재미있는 점은 이것은 Tree 그래프에 depth에 linear하게 computation이 증가하고, normal form은 exponential하게 사이즈가 증가하므로 오히려 SPE는 extension form구하는 편이 더 나을 것이다. 
  → 그렇지만 NE는 결국 normal form이 필요하겠지?



## Imperfect Information Extensive-Form Game

### Motivation

* 다른 에이전트의 선택을 완벽하게 알 수 있다라는 가정하게 Perfect Information Game이 이루어진 반면, 만약 상대방의 선택을 모르거나/혹은 금방 잊는 게임은 어떻게 모델링을 할 수 있을까?
  * 모든 normal form은 IIEF로 상호치환이 가능한다 (죄수의 딜레마를 떠올리면 PIEF로는 불가능하다!)
  * imperfect information을 가진 agent에 대해서는 특정 node가 Information Sets에 의해 묶이게 된다.
  * 묶여 있는 두 nodes 사이에서는 구별을 할 수가 없게 된다.



### Sequential

* SE(Sequential Equilibrium)
  : NE와 SPE 사이의 그 어드메. 
  * 모든 Information Set에 대해서 주어진 Strategy $$S$$와 확률 분포 $$\mu(\cdot|I)$$, 
    (Belief, 이전 Agent가 무엇을 선택할지에 대한 확률분포)가 
    1)Sequentially Rational, 2)Consistency를 만족할 때, 이 $$S$$는 SE이다.
    * Sequentially Rational은 주어진 확률 분포 $$\mu(\cdot |I)$$를 이용해서 기대값이 더 높은 방향을 선택했을 때 $$S$$와 동일한가?
      (내 Belief를 기반으로 선택한 전략이 최적의 전략과 동일한가?)
    * Consistency는 주어진 $$S$$로 확률 분포 $$\mu(\cdot |I)$$를 다시 재현해낼 수 있을까? (Bayes Rule을 이용해서)
      (최적의 전략으로 보았을 때, 내 Belief와 동일한가?)
  



Nash Equilibrium $$ \subset $$ Sequential Equilibrium $$ \subset $$ Subgame-perfect Equilibrium



## Multistage Games

### Motivation

* Normal Form Game: Agent들이 상대방의 패를 보지 않고 동시에 전략을 선택함.
* Extensive Form Game: 상대방의 패를 보고 전략을 선택함. Payoff는 경기가 끝난 이후에 받게 됨.
* 실제 세상은, 상대방의 패를 보고 전략을 선택할 때 마다 Payoff를 얻게 되고, 이를 통해 Feedback을 얻는 효과가 있음.
  * Normal/Extensive Form Game은 하나의 큰 게임이다.
  * Q1) 만약 Agent가 합리적이고 미래를 생각한다면, 실제 세상에서는 여러 개의 작은 게임들로 구성된다고 볼 수 있나?
           ( 각 게임마다 Nash Equilibrium을 선택할까? )
  * Q2) 만약 Agent가 하나의 큰 게임으로 세상을 바라본다면, 현재 Agent의 선택은 과거의 선택에 영향을 받는가?
           ( 미래의 기대치를 위해 Nash Equilibrium이 아닌 선택을 내릴까? )



### Subgame-Pefect Equilibria for Multistage Games

* 한 게임이 끝날 때마다 과거의 선택이 공개된다는 점에서 SPE를 생각해볼 수 있다.
  * 합리적인 Agent라면 각 게임에서 Nash Equilibrium을 선택할 것이다 → SPE
    * 즉, 한 개의 NE라면 NE들만으로 구성된 SPE를 만들어낼 수 있다.
  * 그러나 과거의 NE가 아닌 선택을 기반으로 strategy를 만들어낸다고 할지라도 SPE가 될 수 있다.
    * 단, 게임 중 일부는 반드시 다수의 NE strategy를 가지고 있어야만 한다.
    * 만약 Final Round에서 Multiple NE 중에서 하나를 선택하게 된다면 SPE가 될 수 있다. → 이 경우에 Agent가 이전 stage에서도 다른 Strategy로 Deviate하지 않을 것이라는 증명이 필요하다.



### Lessons

* Conditional한 선택을 하는 것이 도움이 될 수 있다.
* 미래의 사건은 꽤나 중요(Reward-and-Punishment)해야만 과거의 NE가 아닌 선택이라도 유의미해질 수 있다.



## Repeated Games

### Motivation

* Multistage Game에서 매 Stage마다 똑같은 Game이 이루어지는 경우

  * Repeated Game은 Multistage Game의 특별한 케이스임.
  * Q1) 상대 Agent에 대해서 어떤 것을 관찰할 수 있을까?
  * Q2) Agent는 얼마나 많은 과거를 기억할 수 있을까?
  * Q3) 전체 게임에서 Agent의 utility는 무엇일까?

  

### Finitely-Repeated Games

* Multi-stage Game과 거의 유사함
* Extensive-From Game w/Imperfect Information으로 표현이 가능함
* *Proposition: "만약 유한하게 반복되는 게임의 한 스테이지에서 단일한 NE가 존재한다면, 게임에는 NE가 반드시 존재한다."*



### Infinitely-Repeated Games

* *"만약 무한하게 반복되는 게임의 한 스테이지에서 단일한 NE가 존재하더라도, NE를 선택하지 않아도 전체 게임에서의 NE가 될수 있다." ?!*



* 무한하기 때문에 우리는 Tree를 만들 수도 없고 Payoff의 합산으로도 나타낼 수 없다.
* 따라서, Future Discounted Reward($$u_{i} = \sum_{t=1}^{\inf}\gamma^{t-1}u_{i}^{t}$$)를 생각해야만 한다.
  * <u>Def 1</u> Average Reward : $$\bar{u}_{i} = (1-\gamma)\sum_{t=1}^{\inf}\gamma^{t-1}u_{i}^{t}$$
    (만약 똑같은 reward $$v$$를 지속적으로 받는다면 위의 average reward는 역시나 $$v$$를 나타낸다 )
  * <u>Def 2</u> Average Reward : $$\bar{u}_{i} = lim_{k \rightarrow \inf} \frac{1}{k}\sum_{t=1}^{k}u_{i}^{t} $$



#### SPE for IRG

* 정의된 SPE은 모든 history $$h_t$$에 대해서 내가 선택한 전략이 항상 Nash Equilibrium이어야만 한다는 것.

  * 예를 들어, 무한하게 많은 stage에서 동일한 게임을 진행하는데, NE는 오직 하나라고 하자. 그렇다면 그 NE를 매번 선택하는 것이 어떤 $$t$$일 때라도 NE를 보장할 수 있다.

  * 만약 그렇지 않은 경우라면??

    * Reward-And-Punishment Strategies! i.e. Grim-Trigger
    * 증명은 어떻게 하나
      * Grim-Trigger(시작은 동맹, 상대방이 배신하는 순간 죽을때까지 나도 배신, 눈에는 눈 이에는 이)에서 Agent들이 벗어날 여지가 없음을 보이면 된다
      * On the equilibrium path
        * 잘 지키고 있다가 어느 순간 deviate 했을 때 기대보상을 구해서 비교해보자. 만약 잘 지키는 것보다 바꾸는 것이 기대보상이 적다면 SPE가 아니다. (NE가 아니므로)
      * Off the equilibrium path

    

    <u>Cournot Doupoly  예제 다시 읽어보자</u>

    

    * Folk Theorem
      * Game $$G=(N,A,\mu)$$
      * Payoff vector $$r=(r_1, r_2, \cdots, r_n)$$
      * $$v_i = min_{s_{-i}\in S_{-i}} max_{s_{i}\in S_{i}} u_{i}(s_{-i}, s_{i})$$: $$-i$$가 $$i$$를 방해하는 상황.
      * <u>Def</u> A payoff profile $$r$$ is *enforceable* if $$r_i \geq v_i$$
      * <u>Def</u> A payoff profile $$r$$ is *feasible* if $$\forall i, \exists \alpha_{a} r_{i} = \sum_{a \in A}\alpha_{a} u_{i}(a)$$ with $$\sum_{a \in A} \alpha_{a} = 1$$
        (a convex, rational combination of the outcomes in $$G$$)
      * <u>Thm</u> 각 Agent의 payoff vector $$r$$이 각 Agent 별 minmax값 보다 크고(encorceable), utility space상에서 convex combination으로 표현이 가능하다면 (feasible) 이 $$r$$은 어떤 NE에 속해져 있다.
        **<u>*READ PROOF**</u>*