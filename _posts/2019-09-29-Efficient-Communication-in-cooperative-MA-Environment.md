---
title: "Multi Agent Actor-Critic for Mixed Cooperation-Competitve Environments"
usemathjax: true

categories:
- review
tags:
- MAS
- Communication
---

이 논문을 읽게 된 이유: Communication 최근 논문 서칭 중 발견한 논문. [PDF](http://www.ifaamas.org/Proceedings/aamas2019/pdfs/p2321.pdf)



## Introduction

* Communication is crucial to get a best result in cooperative settings.
* If agents can use all kinds of information from other agents, it would be very difficult to study something really useful during training.
* Need to extract relative information from it.



## Methods

* Propose CFAC(communication filtering actor-critic framework)
  * Decentralized policies which could exchange filtered information
  * Using centrally computed critics.

![Screenshot from 2019-09-30 06-45-20](/assets/images/2019-09-29-Efficient-Communication-in-cooperative-MA-Environment/Screenshot%20from%202019-09-30%2006-45-20.png)

* Critic Network
  * Centralized
  * $$Q_{\theta}$$ w/parmaeter $$\theta$$



* Information filtering network
  * semi-centralized
  * $$F_{\mathcal{K}}$$ w/parameter $$\mathcal{K}$$
  * $$d_{t} = F_{\mathcal{K}}(c^{0}_{t})$$
    → 특별한거 없고 그냥 뉴럴넷 넣어서 만듦;; CommNet에서 Mean 대신 뉴럴넷으로 대체한 케이스. 
    → 근데 같은 정보를 broadcasting 해준다는 말인지, 다른걸 던져준다는 말인지. 자세한 설명이 없으니 모르겠음. → 그리고 $$C_{t}$$를 어떻게 만든다는건지 모르겠네..




* Policy Network
  - Decentralized
  - $$\pi_{\psi}$$ w/parameter $$\phi$$
  - Input: 
    - partial observations (Continous)
    - the filtered and efficient information 
  - output: the current time decision (Discrete)
  - $$a_{t} = \pi_{\theta}(o_{t}, d_{t})$$



![Screenshot from 2019-09-30 07-19-57](/assets/images/2019-09-29-Efficient-Communication-in-cooperative-MA-Environment/Screenshot%20from%202019-09-30%2007-19-57.png)



## Results

![Screenshot from 2019-09-30 07-31-57](/assets/images/2019-09-29-Efficient-Communication-in-cooperative-MA-Environment/Screenshot%20from%202019-09-30%2007-31-57.png)

* 당연히 CommNet보다는 잘 나와야할 것. 왜냐하면 Individual Reward를 하니깐.

* ATOC보다도 잘 나올 수 밖에 없는건 어쩌면 Global Broadcasting을 하기 때문에. 마치 IC3Net같은 효과?

  근데 N을 얼마로 잡고 실험을 했는지 모르니 함부로 얘기할 순 없을 듯



## ? 

* 이 논문 설명을 제대로 안 한게 한 두개가 아니다.
  * Parameter sharing between agents? MAYBE..
  * $$C_{$}$$는 도대체 어디서 튀어나오는거냐!
  * Filtering하는 부분은 뭔 놈의 Semi-Centralized인가????
  * 실험 조건들이 하나도 안 나와있다. 
    N이 크지 않다면 당연히 브로드캐스팅이 나을테고 그래서 결과는 잘 나올꺼다. 특히나 CommNet처럼 linear하게 embedding시키지도 않았고. 근데 만약 N이 크다면 Efficiency에서 보았을때 ATOC보다 나을 면이 전혀 없는데;; ATOC은 ~~마음에 안들긴 하지만 그래도 나름~~ local에서만 주고받았다.

