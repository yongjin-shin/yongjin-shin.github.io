---
title: "Attention Is All You Need"
usemathjax: true

categories:
- review
tags:
- Attention
- Transformer
---

이 논문을 읽게 된 이유: Communication 쪽에서 Attention이 사용되고 있고, Transformer 쪽으로 공부하기 위함. 그리고 연구실의 소스를 활용할 수도 있기 때문임! [PDF](https://arxiv.org/abs/1706.03762)



## Introduction

* [Problem] Inherently sequential nature precludes parallelization within training examples, which becomes critical at longer seqeunce lengths, as memory constraints limit batching across examples.
* [Propose] To draw global dependencies between input and output.
* [Cons] Allow for significantly more parallelization.



## Note

* **Self Attention**
  - In Self-Attention or Key=Value=Query, if the input is, for example, a sentence, then each word in the sentence needs to undergo Attention computation. The goal is to learn the dependencies between the words in the sentence and use that information to capture the internal structure of the sentence.

* **Residual Network**

  * When deeper networks are able to start converging, a *degradation problem*has been exposed: with the network depth increasing, accuracy gets saturated (which might be unsurprising) and then degrades rapidly. Unexpectedly, such degradation is not caused by overfitting, and adding more layers to a suitably deep model leads to higher training error.
    * **Degradation Problem**
      * The authors of the original paper state that they trained their net with batch normalization and examined the gradients of the net, and the problem does not appear to be a case of vanishing gradients. I’ll be simplifying some of the complexity here, but basically they guess that potentially their solvers and architecture have a hard time learning the identity function for certain layers of the net, and may have an easier time learning zero mappings for those same layers: it is harder to learn f(x) = x, than f(x) = 0. Ultimately, this motivates their setup for learning small residuals and adding them to the input, rather than just transforming the whole input directly.[[ref](https://www.quora.com/What-exactly-is-the-degradation-problem-that-Deep-Residual-Networks-try-to-alleviate)]

  

  * One solution to this problem was proposed by Kaiming He et. al in *Deep Residual Learning for Image Recognition*[[blog](https://kharshit.github.io/blog/2018/09/07/skip-connections-and-residual-blocks#myfootnote1)] to use Resnet blocks, which connect the output of one layer with the input of an earlier layer. 
  * Usually, a deep learning model learns the mapping, M, from an input x to an output y, $$M(X) = y$$. Instead of learning a direct mapping, the residual function uses the difference between a mapping applied to x and the original input, x, $$F(x) = M(x) - x$$. [[blog](https://kharshit.github.io/blog/2018/09/07/skip-connections-and-residual-blocks)]
  * It is easier to optimize this residual function F(x) compared to the original mapping M(x). 
    $$M(x) = F(x) + x$$ or $$y = F(x) + x$$

질문.

- 단순히 distance를 계산하는 것 이상으로 attention은 어떤 강점이 존재하는가??



내가 할 수 있는 문제제기.

- 확률 모델링은 할 수 없을까?
  - Attention
  - Signaling
- 왜 message를 먼저 만드는가?
  - Signaling을 통해 overhead를 줄이고 message를 잘 만들자.
  - 내가 가진 의문은...어떻게 시그널링을 잘 날릴 수 있을까?
    - 시그널을 잘 만들기만 해도 보다 쉽게 모델링을 할 수 있다.
    - q-value가 애매한 아이들은 왜 안 좋을까? 단순히 q-value를 올린다고 다 좋아지나??? 
      ==> 논문 찾아볼 것. Q-value가 높으면 다 좋은건가? : 좋은 모델이라면 당연히 그래야겠지?
  - 여기에 Attention을 끼워넣을 수 있을까?



