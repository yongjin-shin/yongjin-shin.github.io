---
title: "MARL Review"
usemathjax: true

categories:
- review
tags:
- MARL
---

이 논문을 읽게 된 이유: Global한 View에 대한 paper를 찾다가 발견한 논문 [PDF](https://ieeexplore.ieee.org/abstract/document/8698861)

## Embedding

★☆☆☆☆ [Scalable Deep MARL via observation Embedding and Parameter Noise. IEEE 2019]((https://ieeexplore.ieee.org/abstract/document/8698861)):
Global한 View에 대한 paper를 찾다가 발견한 논문. Global view는 그냥 MLP. Parameter noise(TD3였나?)를 사용함.  여튼 Embedding에 대한 분석도 없었고, 내 기준에선 novelty는 없음. MADDPG보다 Scaling에서 유리하다에서 끝. 실험도 몇 개 안함. MAAC도 똑같은 의견을 제시하지만 거기서는 Attention도 사용했고, 실험도 더 풍부했음(Baseline도 다양하고 Attention에서 어떤 효과가 나오는지 잘 나타남). 