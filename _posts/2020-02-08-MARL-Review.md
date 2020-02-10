---
title: "MARL Review"
usemathjax: true

categories:
- review
tags:
- MARL
---



읽은 논문들 간단하게 요약 정리. 중요한 논문은 따로 정리할 것.



## Embedding

**별별별별**별 [World Models. NIPS 2018]((https://arxiv.org/abs/1803.10122)):

이제는 너무 당연해져버려서 식상해져버린 논문이랄까? RNN으로 미래를 예측하고(Recurrent 구조를 만들고), VAE로 추상화된 현실을 반영하고(Embedding을 넣고) , 이를 Actor의 input으로 넣는다. 이 논문은 pre-trained VAE와 LSTM를 사용(1e4 roll out, random action)한다. VAE -> LSTM 순서로. LSTM은 $$p(z_{t+1}|a_{t}, z_{t}, h_{t})$$을 MoG로 만든다. Hallucination으로 학습을 하는 것은 흥미로웠다. 실험도 재미있고 글도 재미있었다. 바사와니 논문처럼 짜임새 있는 논문이라는 면모보다는 상상력을 자극하는 논문. 논문에서도 언급되었듯이, pre-trained 모델을 사용하는 것은 task와 관련된 임베딩을 만드는데에는 적절하지 못한 방법인 듯 하다. 

특히 MARL 상황에서는 따로 학습하면 다른 agent가 환경의 일부가 되는데, 그 "환경"이라는 것이 가지는 policy를 전혀 유추할 수가 없다. 왜냐하면 recurrent한 부분이 반영되지 않으면 Time invariant한 embedding만 된다는 것이고, 그것은 보다 general한 케이스의 상황들에 잘 들어맞는 embedding만 학습될 터이다. 즉, task dependent(상대방의 policy를 내포하고 있는)한 embedding은 만들지 못할 수도 있다 (아마도?)



**별**별별별별 [Scalable Deep MARL via observation Embedding and Parameter Noise. IEEE 2019]((https://ieeexplore.ieee.org/abstract/document/8698861)):

Global한 View에 대한 paper를 찾다가 발견한 논문. Global view는 그냥 MLP. Parameter noise(TD3였나?)를 사용함.  여튼 Embedding에 대한 분석도 없었고, 내 기준에선 novelty는 없음. MADDPG보다 Scaling에서 유리하다에서 끝. 실험도 몇 개 안함. (MAAC도 똑같은 의견을 제시하지만 거기서는 Attention도 사용했고, 실험도 더 풍부했음. Baseline도 다양하고 Attention에서 어떤 효과가 나오는지 잘 나타남. 근데 솔직히...MAAC 다시 읽어봐야지. Attention 쓴게 그렇게 대단한건지..) 