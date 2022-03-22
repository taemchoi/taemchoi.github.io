---
title: "crontab을 활용한 스케줄링"
tags: [Reinforcement Learning]
categories:
  - reinforcement
date: 2022-03-22
toc : true
---
- [Markov Process(MP)](#1-markov-processmp)
- [Markov Reward Process(MRP)](#2-markov-reward-processmrp)
- [Markov Decision Process(MDP)](#3-markov-decision-processmdp)
- [MP, MRP, MDP의 관계](#4-mp-mrp-mdp의-관계)

## 1. Markov Process(MP)
- 앞으로 일어날 일련의 사건들을 확률 모델링으로 정의

### 1. Markov Property
- MP의 핵심 가정으로 S<sub>t+1</sub>이 일어날 가능성은 앞의 모든 이전 사건들(S<sub>1</sub> ... s<sub>t</sub>)를 모두의 가능성을 보는 것이 아니라,
 직전 사건의 가능성과의 관련만 있다. 즉, 그 이전 사건들과는 **독립성 가정**을 갖는다.
- 이를 조건부 확률로 표현하면   
P<sub>ss'</sub> = P(S<sub>t+1</sub> = s｜S<sub>1</sub> ... s<sub>t</sub>)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;= P(S<sub>t+1</sub> = s'｜S<sub>t</sub>)   
- 즉, 자연어처리의 bi-gram과 룰과 동일하다.

### 2. 표현
- MP는 S(set of state)와 SP(state간의 연쇄적 확률 matrix)로 표현가능하다. 
- <p align='center' style="font-size:small"><img src="/img/rl/rl1/0.jpg">SP 예시, 대칭 행렬로 행과 열은 S로 구성된다.</p>
- 게임을 한다고 했을 때, 현재 state가 적이 출현이라면 다음 장면이 무엇이 될지 여러 경우의 수가 state가 되고 그 state의 확률을 MP로 표현할 수 있다.

## 2. Markov Reward Process(MRP)
- MP의 개념에 Reward를 추가하여 모델링

### 1. Components
- S<sub>t</sub> : t시점의 states
- R<sub>t</sub> : t시점의 보상, 즉, t-1에서 t로 state가 이전되었을 때, 발생하는 reward 값
- γ : Discount factor, 바로 다음 state에서 받는 보상과 미래 시점에 예상되는 보상의 가치는 다르기 때문에 가중치 factor로 적용 (γ = [0,1])
- G<sub>t</sub> : t시점에 받을 수 있는 reward의 합(=return)  
G<sub>t</sub> = r<sub>t+1</sub> + γr<sub>t+2</sub> + γ<sup>2</sup>r<sub>t+3</sub> + ...  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;= ∑γ<sup>k</sup>r<sub>t+k+1 (k= [0,∞])
- V(s) : STATE-VALUE FUNCTION으로 현재 state에서 발생할 수 있는 return들의 기댓값  
즉, 현재 state에서 발생할 수 있는 모든 경로의 리턴의 기댓값으로 볼 수 있다.  
V(s) = Ε[G<sub>t</sub>｜S<sub>t</sub> = s]

### 2. 표현
- 즉, 기존 MP의 구성요소 S, SP 이외에 R과 γ로 표현가능하다.

## 3. Markov Decision Process(MDP)
- MRP의 개념에 action을 추가하여 모델링
- 기존 MP나 MRP는 action없이 state가 transition되었다면, 이제는 action에 따라 state가 달라진다는 개념이다.

### 2. Components
- 기존 MRP의 구성요소들 : S, R, γ, G, V
- A<sub>t</sub> : t시점의 action
- π(a｜s) : Policy로 현재 state에서 어떤 action을 수행할 것인지에 대한 조건부 확률  
π(a｜s) = P(A<sub>t</sub> = a｜S<sub>t</sub> = s)  
- Q<sub>π</sub>(s,a) : Action-value function으로 현재 state에서 어떤 action을 수행했을 때, 얻을 수 있는 return의 기댓값  
Q<sub>π</sub>(s,a) = Ε[G<sub>t</sub>｜S<sub>t</sub> = s, A<sub>t</sub> = a]

### 3. 표현
- 기존의 MRP 구성요소 이외에 A를 추가하여 표현가능하다.

## 4. MP, MRP, MDP의 관계
- MDP가 주어졌을 때, MP와 MRP를 표현하면 아래와 같다.
1. **MP**  
P<sub>π, ss'</sub> **=** P(s'｜s) **=** P(s'｜a<sub>1</sub>)π(a<sub>1</sub>｜s) +  P(s'｜a<sub>2</sub>)π(a<sub>2</sub>｜s) + ... **=** ∑π(a<sub>k</sub>｜s)P(s'｜a<sub>k</sub>)
2. **MRP**  
R<sub>π, s</sub> **=** ∑π(a<sub>k</sub>｜s)R(s｜a<sub>k</sub>)  

