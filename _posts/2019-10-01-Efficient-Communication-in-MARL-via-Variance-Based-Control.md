---
title: "Efficient Communication in MARL via Variance Based Control"
usemathjax: true

categories:
- review
tags:
- MAS
- Communication
---

이 논문을 읽게 된 이유: Communication 최근 논문 서칭 중 발견한 논문. 제목으로 봐서는 channel을 직접적으로 모델링 하는게 아니라 Message로 이슈를 넘기는 듯한 기분이 들었음. NIPS 2019 [PDF](http://www.ifaamas.org/Proceedings/aamas2019/pdfs/p2321.pdf)



## Introduction

* Three lines of research that try to mitigate the instability and indfficiency caused by decentralized execution

  * Independent Q-learning : Does not account for instability casued by environment dynamics

  * Centralized training and decentralized execution

  * Communication among agents during execution : Overhead in terms of latency and bandwidth

    * its success is heavily dependent on the usefulness of the received information.




* Problem
  * Often superfluous(unnecessary) for an agent to wait for feedback from all surrounding agents before making an action decision. 
    *i.e. the front camera trigger 'brake' signal without waiting for the feedback from the other parts of the vehicle*
  * The feedback received from the other agents may not always provide useful information.
    *i.e. the navigator should pay more attention to the messages sent by the perception system rather than by the entertainment parts.*
  * Excessive amount of communication may introduce useless and even harmful information which could even impari the convergence of learning.



*  This paper
* Fully cooperative senario
  * Centralized learning and decentralized execution + Communication
  * Inserting an extra loss term on the variance of the exchanged information, the valuable and informative part inside the messages can be effectively extracted and leveraged to benefit the training of each individual agent.
    * Initiates communication only when its confidence level on this preliminary decision is low.
    * the agent replies to the request only when its message is informative.





## Methods

* The main idea of VBC is to achieve the superior performance and communication efficiency by limiting the variance of the transferred messages. 
* During execution, each agent communicates with other agents ony when its local decision is ambiguous.
  * The degree of ambiguity is measured by the difference on top two largest action values.
* The agent replies only if its feedback is informative, namely the variance of the feedback is high.



![Screenshot from 2019-09-30 08-38-18](../assets/images/2019-10-01-Efficient-Communication-in-MARL-via-Variance-Based-Control/Screenshot%20from%202019-09-30%2008-38-18.png)





### Network

* Local Action Generator

  * GRU + FC
  * output : $$Q_{i}(o^{t}_{i}, h^{t-1}_{i}, a^{t}_{i})$$ for each action $$a^{t}_{i} \in A$$

  

* Message Encoder

  * MLP(2 $$\times$$ FC)
  * Multiple independent message encoders
  * each accepts $$c^{t}_{j}, j \neq i$$ from another agent
  * outputs $$f^{ij}_{enc}(c^{t}_{j})$$



* Combiner
  * Inputs: the outputs from both local action generator and message encoders
  * produces the global action-value function $$Q_{i}(o_{t}, h_{t-1}, a^{t}_{i})$$ of agent $$i$$