---
title: "Attention Is All You Need"
usemathjax: true
toc: true
toc_sticky: true

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



## Why Self-Attention

* The total computational complexity per layer
* The amount of computation that can be parallelized
* The path length between long-range dependencies in the network



![compare](/assets/images/2019-10-06-Attention-is-all-you-need/compare.png)





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



## +

문제를 확실히 정하고, 그 문제를 해결하기 위한 구조를 만들고, 결과에는 앞선 문제들을 해결한 결과가 나온다. 그리고 왜 그런 모델이어야만 하는지도 문제랑 같이 연결시켰다. 짜임이 좋은 논문이고, 아이디어도 오리지널리티가 분명하고, 더불어 결과도 뛰어난 논문. 진짜 좋은 논문이다.



문제를 확실하게 fix를 해야한다. 기존에는 어떤 것이 안 되어 있었고, 그게 왜 필요하고, 그래서 어떻게 수정할 수 있는지가 분명해야 한다. 그리고 결과도 좋아야만 한다.



요리(개념)는 많이 만들지 말고, 딱 하나만. 그러나 그걸 맛있게 만들기 위해서는 양념(기존 방법론)은 밸런스에 맞춰서 이것저것 들어가면 된다.

