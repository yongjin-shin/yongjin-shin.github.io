---
title: "Multi Agent Actor-Critic for Mixed Cooperation-Competitve Environments"
description: "Nash Equilibrium and Multiagent RL"
usemathjax: true

categories:
- review
tags:
- MAS
- Actor-Critic
---

[PDF](https://arxiv.org/abs/1706.02275)

이 논문을 읽게 된 이유: Homogeneity와 Heterogeneity에 대한 고민. Policy Parameter를 Agent 간에 공유가 이루어질 경우에는 Homogenous한 가정 밖에 할 수 없다. 이것을 어떻게 깰 것인가?

---

## Introduction

* Each agent's policy is changing as training progresses

* The environment becomes non-stationary from the perspective of any individual agent

  → learning stability challenges, preventing the straightforward use of past experience replay.


  → Crucial for stabilizing deep Q-learning. 
  **Q) Why esp Q-learning?** since violating stationary assumption required for convergence of Q-learning. Also the experience replay buffer, helping to stabilize DQN, cannot be used when $$P(s'|s, a, \pi_{1}, \cdots, \pi_{N}) \neq P(s'|s, a, \pi'_{1}, \cdots, \pi'_{N})$$ for any $$\pi_{i} \neq \pi'_{i}$$


  → Policy-Gradient. Exhibit very high variance when coordination of multiple agents is required 
  **Q) Why?** *since an agent's reward usually depends on the actions of many agents, the reward conditioned only on the agent's own actions (when the actions of other agents are not considered in the agent's optimization process) exhibits much more variability, thereby increasing the variance of its gradients.*



* Propose the learning algorithm:

  1) To learn policies that only use local infomation.

  2) Model-free

  3) Cooprative and Competitive

* Adopt the framework of centralized training with decentralized execution

  → Allowing policies to use extra information to ease training , so long as this information is not used at test time.
  → Q function canot contain differenc information at training and test time.
  → Thus, Extension of Actor-Critic!

  + Critic: augmented with extra information about the policies of other agents (or might be programmed to infer those policies)
  + Actor: access to local information only. After training is completed, only the local actors are sued.



## Related Work

* [Gupta et al, 2017](http://ala2017.it.nuigalway.ie/papers/ALA2017_Gupta.pdf): Policy Parameter sharing.
* [Foester et al, 2017](https://arxiv.org/abs/1705.08926): A single centralized critic for all agents (same reward), No communication, Recurrent Policies with feed-forward critics, Discrete policies.



## Methods

* Assumption: 
  1) the learning policy can only use local information at execution time
  2) Model free
  3) No communication channel
  **→ For the general-purpose multi-agent learning algirhtim**

* Consider a game:
  1) $$N$$ agents
  2) Policy parameters $$\bold{\theta}=\{ \theta_{1}, \cdots, \theta_{N}\}$$
  3) Policy $$\bold{\pi}=\{\pi_{1}, \cdots, \pi_{N}\}$$
  4) Some state information $$\bold{x} = (o_{1}, \cdots, o_{N})$$
  5) Centralized action-value function $$Q^{\pi}_{i}(\bold{x}, a_{1}, \cdots, a_{N})$$ 
  → Learned separately, agents can have arbitrary reward structures, including conflicitng rewards
  6) Gradient of expected return for agent $$i$$ ($$\mathcal{J}(\theta_{i})=\mathbb{E}[R_{i}]$$) :
  $$\nabla_{\theta_{i}}\mathcal{J}(\theta_{i}) = \mathbb{E}_{s\sim p^{\mu}, a_{i} \sim \pi_{i}} \big[ \nabla_{\theta_{i}} \log{\pi_{i}(a_{i}|o_{i})} Q^{\pi}_{i}(\bold{x}, a_{1}, \cdots, a_{N}) \big]$$
  7) [Deteministic Policies](https://lilianweng.github.io/lil-log/2018/04/08/policy-gradient-algorithms.html#dpg): $$\nabla_{\theta_{i}}\mathcal{J}(\mu_{i}) = \mathbb{E}_{\bold{x},a \sim \mathcal{D}} \big[ \nabla_{\theta_{i}} \mu_{i}(a_{i}|o_{i}) \nabla_{a_{i}}Q^{\mu}_{i}(\bold{x}, a_{1}, \cdots, a_{N})|_{a_{i} = \mu_{i}(o_{i})} \big]$$, where the experience replay buffer $$\mathcal{D}$$ contains the tuples $$(\bold{x}, \bold{x'}, a_{1}, \cdots, a_{N}, r_{1}, \cdots, r_{N})$$

  

* 

  











