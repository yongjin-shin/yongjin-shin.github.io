---
title: "Paper Review - Short"
usemathjax: true
toc: true
toc_sticky: true

categories:
- review
tags:
- research
---

이것저것 읽다보니깐 기억해야할 것들, 재미있었던 것들 등등 잘 떠오르지가 않는다. 트렐로에서 계속 추가하는 것은 눈에 보기 힘드니깐.. 일단 간단하게라도 정리를 해둬야겠다.

## Distillation

* [Distilling the knowledge in a Neural Network](https://arxiv.org/pdf/1503.02531.pdf)
  Soft Targeting을 이용하면 distiling이 잘 된다. 이유인즉슨, attention처럼 label간의 관계를 이 logit에서 지니고 있기 때문에 적은 파라미터를 통해서도 이 관계를 쉽게 전해받을 수 있게 된다. (뿐만 아니라 soft targeting이기 때문에 각 label간의 logit이 hard target보다 차이가 크지 않기 때문에 variance가 높지 않다는 것도 한목 한다 -고 medium에서는 말한다. 논문에 있나?) MoE랑 비교해보면 Generalized Learner를 구축해서 그걸토대로 Specialized Learner을 만드는 형식이, MoE의 Gate Learner의 부담을 줄일 수 있다고 주장한다.
* [When Does Label Smoothing Help?](https://arxiv.org/pdf/1906.02629.pdf)
  Label Smoothing에서는 각 Class 별로 logit의 차이를 일정하게 유지시킨다고 주장한다. 특히 외부에서 억지로 Label에 대한 bias를 주기 때문에 이러한 작업은 Calibrate효과가 있다고 말한다. 충분히 납득가능한 주장이고 실험도 충분히 납득가능하다. 그리고 첫번째 주장 때문에 오히려 Distillation에서는 안 좋은 결과를 가지는데, Label smoothing이 없는 경우에 Class 간의 관계가 더 다양하게 나오는데 반해 (따라서 student model은 그 관계를 쉽게 알아차릴 수 있지만) Label smoothing에서는 Class 간에 관계가 항상 일정하기 때문에 Student model이 학습하기 어렵다고 말한다.



## Active Learning

* 



## Multi-Task

* 



## Federated Learning

* 

