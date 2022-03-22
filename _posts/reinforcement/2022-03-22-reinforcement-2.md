---
title: "Bellman Expactation Equation과 Bellman Optimality Equation"
tags: [Reinforcement Learning]
categories:
  - reinforcement
date: 2022-03-22
toc : true
---
- [Bellman Equation](#0-bellman-equation)
- [Bellman Expactation Equation](#1-bellman-expactation-equation)
- [Bellman optimality Equation](#2-bellman-optimality-equation)

## 0. Bellman Equation
- 벨만 방정식은 현재의 가치함수를 다음단계의 가치함수로 정의한 방정식이다.
- 벨만 기대 방정식과 벨만 최적 방정식 등이 있다.

## 1. Bellman <u>Expactation</u> Equation
- 벨만 기대 방정식 : 현재 가치함수를 즉시 받는 rewarnd와 다음 단계의 가치함수로 표현한 관계식  

### 1. 유도  
V(s) = Ε[G<sub>t</sub>｜S<sub>t</sub> = s]  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;= E[r<sub>t+1</sub> + γr<sub>t+2</sub> + γ<sup>2</sup>r<sub>t+3</sub> + ...｜S<sub>t</sub> = s]  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;= E[r<sub>t+1</sub> + γ(r<sub>t+2</sub> + γr<sub>t+3</sub> + ...)｜S<sub>t</sub> = s]  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;= E[r<sub>t+1</sub> + γG<sub>t+1</sub>｜S<sub>t</sub> = s]  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;= E[r<sub>t+1</sub> + γV(S<sub>t+1</sub>)｜S<sub>t</sub> = s]  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;= R<sub>s</sub> + <u>γ∑p<sub>ss'</sub>V(s')</u>  
※ γ∑p<sub>ss'</sub>V(s') 이 부분을 보면, s에서 다음 state s'으로 가는 노드는 여러가지가 있다. 이 여러 노드의 **(각 가치 x 각 확률)의 합**으로 표현될 수 있다. 


### 2. Bellman Equataion 계산
#### 2.1. MRP
- small MRP : linear equation(matrix)으로 해결 가능하다.
    - V = R + γPV  
    (1-γP)V = R  
    V = (1-γP)<sup>-1</sup>R
    - <img src="/img/rl/rl2/0.jpg">

- large MRP : Dynamic programming 등 iteration method를 활용해야한다. → 연산량이 많기 때문

#### 2.2. MDP
- Q<sub>π</sub>(s, a) = E[r<sub>t+1</sub> + γQ<sub>π</sub>(S<sub>t+1</sub>, A<sub>t+1</sub>)｜S<sub>t</sub> = s, A<sub>t</sub> = a]  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;= R<sub>(a, s)</sub> + γ∑P<sub>(a, ss')</sub>V<sub>π</sub>(s')  → bellman expectation equation에 의함  
※ R<sub>(a, s)</sub> : 해당 stage의 action에 대한 return 값 / γ∑P<sub>(a, ss')</sub>V<sub>π</sub>(s') : 이후 발생하는 모든 action의 return 값의 합
- V<sub>π</sub>(s) = E[r<sub>t+1</sub> + γV<sub>π</sub>(S<sub>t+1</sub>)｜S<sub>t</sub> = s]  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;= ∑π(a｜s)Q<sub>π</sub>(s,a) → (하나의 action이 일어날 확률(π(a｜s))과 그 action을 했을 때에 대한 return 값(Q<sub>π</sub>(s,a))의 합)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;= ∑π(a｜s)**(R<sub>(a, s)</sub> + γ∑p<sub>(a, ss')</sub>V<sub>π</sub>(s'))**  
- Q<sub>π</sub>(s, a) = R<sub>(a, s)</sub> + γ∑P<sub>(a, ss')</sub>**∑π(a'｜s')Q<sub>π</sub>(s',a')**
- 이렇게 state-value function과 Action-value function은 자기 자신으로 수식을 설명할 수 있다.


## 2. Bellman <u>optimality</u> Equation
- 강화학습에서 최적화하는 목적함수로 비선형이다.
- 비선형이기 때문에 iteration method를 활용해야 한다.(ex) Q-learning, Sarsa 등)
- V<sup>*</sup>(s) = max<sub>π</sub>V<sub>π</sub>(s)  
→ state-value function을 최적값은 최적의 policy를 찾는 것으로 설명할 수 있다.  
ex) 2가지 ACTION이 있을 때, policy A가 π(s｜a1), π(s｜a2) = (0.1, 0.9), policy B가 0.4, 0.5라면 둘 중 value function의 값이 큰 것을 최적 policy로 선택한다.
- Q<sup>*</sup>(s,a) = max<sub>π</sub>Q<sub>π</sub>(s,a) : 마찬가지로 최적의 π를 찾는 것이다.
    - optimal policy는 Q<sup>*</sup>(s,a)를 최대화 하는 것이다.
    - Q<sup>*</sup>(s,a)를 알고 있다면, 최적 action에서의 π 값은 1, 나머지는 0으로 즉각적으로 optilmal policy를 추정할 수 있다.

### 1. 계산
- Q<sup>*</sup>(s, a) = R<sub>(a, s)</sub> + γ∑P<sub>(a, ss')</sub>V<sup>*</sup>(s')
- V<sup>*</sup>(s) = max<sub>a</sub>Q<sup>*</sup>(s,a)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;= max<sub>a</sub>(R<sub>(a, s)</sub> + γ∑p<sub>(a, ss')</sub>V<sup>*</sup>(s'))
- Q<sup>*</sup>(s, a) = R<sub>(a, s)</sub> + γ∑P<sub>(a, ss')</sub>(max<sub>a</sub>Q<sup>*</sup>(s',a'))
