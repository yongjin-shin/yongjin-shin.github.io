---
title: "Deep Bayesian Active Learning with Image Data"
usemathjax: true
toc: true
toc_sticky: true

categories:
- review
tags:
- Federated Learning
- Transfer Learning
- mixture model
---



Deep Learning에서 1) 많은 데이터를 사용해야한다는 점, 2) 모델에 대한 불확실성을 반영할 수 없다는 점을 해결하기 위해 Deep Bayesian Network를 사용함. [^1]

![bayesian-al](/assets/images/2020-03-24-bayesian-al/bayesian-al.png)



## 요약

* Active Learning은 기본적으로 적은 수의 Labeled 샘플이 존재할 때, Unlabeled 샘플 중에 가장 학습에 효과적인 샘플들만 취하여 학습한다는 취지이므로, Deep Learning처럼 많은 수의 샘플이 필요한 모델에서 적용하기가 힘들다. 특히, 이미지와 같이 Dimension이 큰 경우는 샘플이 더 많이 필요한데 Active Learning을 적용하기가 쉽지 않다. (확실하게 언급하기 보다는 기존의 연구들은 low dimension을 많이 사용하고, 이미지 쪽에서 연구가 많이 이루어지지 않았다 라는 식으로만 언급하고 넘어감)
* 논문의 기본적인 방법론은, Bayesian Network의 기본적인 성질을 이용하여 acquisition function에서 
  $$p(y=c|x, \cal{D}_{train})$$를 $$\int p(y=c|x,w)p(w|\cal{D}_{train}) dw$$으로 치환해서 Stochasity를 부여했다는 점이다. 
* 기존의 Deterministic한 모델의 경우는 $$q*_{\theta}$$를 point mass로 만들어서 Stochastic 모델과 비교를 하였는데, MeanSTD를 제외하면 Stochastic 모델이 더 적은 수의 데이터를 사용해서 모델 학습이 가능함을 보였다.
* 굉장히 적은 수의 (그리고 +, - 불균형인) Cancer 데이터에서도 BALD를 사용한 모델이 uniform한 경우보다 잘 작동함을 보였다.



## 생각

* 잘 되겠지 싶은 make sense한 논문이고 실험도 그것을 잘 보여준다. 그런데 아쉬운 점은 실제로 어떤 샘플들이 선택이 되는지, 이를 통해서 decision boundary가 어떻게 변화하는지 설명할 수 있는 실험이 있었으면 좋았겠다. model의  stochastity가 어떤 영향을 주는지 결과론적으로만 알겠는데 그것을 좀 더 잘 해석할 여지가 있었을 것 같다.
* Mean STD는 uniform random 보다 결과가 비슷하거나 안 좋은데, 왜 다른 것들은 결과가 더 좋아졌음에도 이것만 성능이 향상이 안 되었는지도 고찰이 필요할 것으로 보인다.
* Real Dataset에서 Active Learning을 사용하지 않고 전체 데이터를 사용했을 때와 얼마나 차이가 나는지 비교 실험이 있었으면 active learning을 얼마나 효과적으로 사용했는지 확인할 수 있었을 것이다.



## Reference

[^1]: http://proceedings.mlr.press/v70/gal17a/gal17a.pdf