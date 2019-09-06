---
titles: "Actor-Critic and Policy Gradients"
usemathjax: true

categories:
- RL
tags:
- Policy Gradients
- Actor-Critic Algorithms

---

논문읽다가 여전히 제대로 모르는 것 같아서 Berkeley 강의를 들었다. 매번 강의 듣고 치워버렸는데, 다른 공부하기 이전에 정리를 해보고 넘어가려고 한다. (생각 정리용)


## Vanilla Policy Gradient

RL에서 하고자 하는 것은 어떠한 tast에서 가장 좋은 결과를 얻기 위한 Action을 찾아내는 것이다. (스탠포드에서는 Sequential Decision Making이라고 했다) 따라서 이를 나타내보자면,


$$ \theta^{*} = argmax_{\theta} \mathbb{E}_{\tau} \sim p_{theta}(\tau) \sum_{t} r(s_{t}, a_{t}]$$


우리가 원하는 것은 $$\theta$$의 optimal 값이 되겠다. 여기서 기대값을 구해야 하는데, 가장 쉽게는 Monte Calro를 이용하면 된다. 물론 몬테카를로가 기대값으로 수렴하기 위해서는 충분히 많은 샘플이 있어야 하고, RL에서는 특정 $$s_0, a_0$$부터 $$s_{des}, a_{des}$$까지 모두 샘플을 얻어야만 한다. (여기서 문제점은 어느 세월에 이러한 샘플링을 할 것인가에 대한 물음이 따르게 된다.) 여튼, 몬테카를로는 하기 싫기 때문에(왜??) Gradient descent/ascent를 하고자 한다. 이걸 도입하는 순간 Global Optimum은 포기한게 아닐까? 뭐, global optimum을 구하면 좋기는 한데 global이고 나발이고 일단 optimum부터 찾는게 문제이니 일단 구해보자. 증명과정 약간의 log 트릭을 사용하면 어째저째 할 수 있으니 생략하고, 결과는


$$ \nabla_{\theta}J(\theta) = \mathbb{E}_{\tau \sim \pi_{\theta}}(\tau) \big [ \big( \sum_{t=1}^{T}\nabla_{\theta} \log{\pi_{\theta}(a_{t}|s_{t})} \big) \big( \sum_{t=1}^{T} r(s_{t}, a_{t}) \big) \big]$$


흠, 그런데 어쨌거나 샘플링을 많이 한다음 derivative를 계산하고 $$\theta$$를 업데이트 해줘야 한다. $$(\theta \leftarrow \theta + \alpha \nabla_{\theta}J(\theta))$$


위의 derivative를 조금 더 음미를 해보자면, 리워드 합의 기대값이 큰 방향으로 $$\theta$$를 이동시켜주는 것이다. (마치 로지스틱 리그레션처럼) 근데 위의 수식에서 리워드 합의 기대값  

$$\sum_{t=1}^{T} r(a_{t}|s_{t})$$     

부분만 제거해주면, $$J_{\theta}$$의 MLE와 동일하게 된다. 나중에 텐서플로우에서는 MLE를 잘해준 다음에 각각의 샘플에 리워드 합의 기대값만 weight sum형식으로 진행해주면 된다는 말이다.


## Reducing Variance

근데 문제점은 단순히 derivative 구해서 업데이트를 해주면 variance가 크다는 말. 즉, 와리가리 하다가 세월 다 보내게 될 수도 있다. 내가 예측한 모델에서 미래의 값들은 점점 더 현실에서 멀어지기 때문에, 단순히 리워드 sum을 해버리면 문제가 생길 수 밖에 없다. 위에서 언급했듯이, derivative는 일종의 weighted sum인데 불확실성이 큰 부분들까지 똑같이 weight가 걸리면 오차가 점점 커질 수 밖에 없다. 이걸 해결해줄 수 있는 방법은 weight를 달리 주는 것이다. 이와 관련된 방법은,


1. 리워드 sum을 할 때 과거는 무시하고 앞으로의 일들만 고려하자.
2. 각 샘플 sequence로부터 평균을 구해서 거기서 얼마나 차이가 나는지를 weight로 사용해보자.

결과만 수식으로 나타내보자면

$$ \nabla_{\theta}J(\theta) = \mathbb{E}_{\tau \sim \pi_{\theta}}(\tau) \big [ \big( \sum_{t=1}^{T}\nabla_{\theta} \log{\pi_{\theta}(a_{t}|s_{t})} \big) \big( \sum_{t'=t}^{T} r(s_{t'}, a_{t'}) - b \big) \big]$$

$$b = \frac{1}{N}\sum_{i=1}^{N}r(\tau_{i})$$

(여러가지 수식들로 풀어쓸 수 있지만, 아직 vim이랑 markdown이 익숙하지 않으므로 여기까지만...)

## Actor-Critic Algorithms
그런데 위의 식에서 weight 부분은 어쨌거나 estimate이다. 왜냐하면 우리가 원하는 완벽한 $$\theta^{*}$$에 의해서 샘플링된 $$s_{t}, a_{t}$$가 아니기 때문이다. 뿐만 아니라, 분산을 줄이기 위해서 계산을 할 때 오로지 해당 시점에서의 샘플들만 고려를 하지, 전체 sequence의 샘플을 모두 고려하는 과정이 없다. 그렇기 때문에 biased는 되어있을지라도 여전히 분산은 높은 실정이다. 그렇다면 우리가 앞에서 계산한 것처럼 계산을 해주는 것보다 더 정확하게 계산할 수는 없을까? 어차피 우리는 $$b$$로 평균을 구한다. 차라리 Q-function에서 action들까지 평균치를 처리한 value function을 이용하면 어떨까? 즉, 
    
$$V^{\pi}(s_{t}) = \mathbb{E}_{a_{t} \sim \pi_{0}(a_{t}|s_{t})}[Q_{\pi}(s_{t}|a_{t})]$$

를 사용하기로 해보자. 여기서 Advantage function이 튀어나온다.

$$A^{\pi}(s_{t}, a_{t}) = Q^{\pi}(s_{t}, a_{t}) - V^{\pi}(s_{t})$$

이것은 weight로 역할을 할 것인데, 평균적인 action들의 값에 비해 특정 action이 얼마나 의미가 있을지에 대한 weight로써 작용을 할 것이다. 자, 이제는 Q는 잘 구하면 되는데, V까지 나타난 시점에서 우리는 모든 action들을 고려한 샘플링을 할 수는 없을 것이다. 따라서 V를 approximation하는 과정을 거칠 것이다. function approximator인 뉴럴넷을 활용하면 될 것이다. 일단 Q는 아래와 같이 근사할 수 있다. 왜냐하면 $$V^{\pi}$$가 오로지 샘플링으로 구한 리워드 sum과 완전히 동일하지는 않기 때문이다.

$$Q(s_{t}, a_{t}) = \sum_{t=1}^{T}r(s_{t}, a_{t}) \approx r(s_{t}, a_{t}) + V^{\pi}(s_{t+1})$$

이때, 위에서 언급한 Advantage function에 쏙 집어넣으면 $$Q^{pi}$$는 사라지고 $$V^{\pi}$$로만 Advantage function을 나타낼 수 있게 된다. 

$$A^{\pi}(s_{t}, a_{t}) \approx r(s_{t}, a_{t}) + V^{\pi}(s_{t+1}) - V^{\pi}(s_{t})$$

따라서, 우리는 $$V^{\pi}$$만 잘 파악하면(policy evaluation) bias도 덜하고 variance도 작은 방향으로 $$\theta$$를 업데이트(policy improvement)해 줄 수 있는 해피한 상황이 되었다. 하지만 이 V-function을 어떻게 잘 구해줄 수 있을까? 뉴럴넷은 어떻게 학습을 시켜야만 value funtion의 parameter인 $$\phi$$를 학습 시킬 수 있을까? 다시금 fitting을 해줘야 하는 상황으로 넘어가게 된다.

답은, supervised learning을 해야한다. 샘플링을 통해 얻은 $$\{(s_{i,t}, a_{i,t})\}$$에서 리워드 $$\{r_{i, t}\}$$를 구할 수 있다. 또 기존의 value-function으로 estimated reward $$\{y_{i, t}\}$$를 구할 수 있다. 결국 흔히 보듯이 L2 norm을 구해버리고 학습을 시키면 된다.

$$\mathit{L}(\phi) = \frac{1}{2}\sum_{i}||V^{\pi}_{\phi}(s_{i}) - y_{i}||^{2}$$

추가적으로 언급할 부분은 Advantage function을 구성할 때 bias-variance tradeoff 이슈가 생긴다. 만약 우리가 더 많은 variance를 허용할 수 없다면, 우리는 더 많은 실제 샘플을 추가해서 진행하면 된다. 그럴수록 우리는 더 많은 bias가 생길 수 밖에 없다. 이 부분은 $$TD(\lambda)$$의 개념을 떠올리면 쉽게 이해가 갈 것이다.

마지막으로 Actor-Critic의 개념은 Actor는 실제로 action을 담담하게 되는 Q-function 쪽을 의미하게 되고, critic은 기준점이 되는 V-function 을 의미하게 된다.
