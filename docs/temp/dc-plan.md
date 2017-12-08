---
layout: page-sidenav
group: "Digital Curling"
title: "개발 일지 (2017-12-06)"
---

To-Do
-----
- $x$
- [] GAN + MCTS to consider similar states

컬링이란
-------
- Turn based strategy game
- Legal moves exist infinitely (**continuous action**)
- Many kinds of **uncertainty** exists

디지털 컬링이란
--------------
- Curling Game SW to analyze and discuss the strategy of curling
- Sweeping is not considered
- Uncertainty is more simple $x$

관련 연구들
----------
- KR UCT (Kernel Regression Upper Confidence Bounds Applied to Trees)[^1]
  - Approach: Kernel regression + MCTS
  - Assumptions: good actions models are given
  - 장점: efficiently explore continuous actions/states
  - 단점: slow and hard to learn from executions
- Jiritsu-kun[^2]
  - Approach: game tree search + evaluation function based on Domain Knowledge
  - Assumption: goodness of the current state can be evaluated based on Domain
  - 장점: fast and intuitive
  - 단점: less accurate in complex game state
- Alphago[^3]
  - Approach: Deep Learning + Monte Carlo Tree Search (MCTS)
  - Assumption: discrete states/actions
  - Benefite: learning the game strategy from experience
  - Limitation: discretization of continuous space/action
- Ayumu:
  - 장점:
  - 단점:

Deep Learning model for Digital curling
---------------------------------------
- 기존의 KR_UCT 방식을 따라가려했지만, KR_UCT 모델의 성능은 잘 짜여진 rule-based rollout policy에 크게 영향을 받는다.
- 컬링의 Donmain Knowledge 에 기반하여 룰을 하나하나 구현하기보다, Deep learning Model 만을 이용하여 최적의 전략을 추천하고자 한다.
- 2016년도 우승 모델을 baseline 모델로 잡고, 딥러닝 모델을 학습시키기 위하여 (Game AI Tournament) GAT 2016 우승 프로그램 데이터를 수집하였다. $x$

**Training Policy Network**

- KR UCT dependent on rule based rollout policy
- To learn the policy, We collected data using AyumuGAT1 (winner of 2016 competition) to learn a policy (about 500, 000 shot logs)
- Rule 기반 방법론을 극복하기 위하여, Policy Netork를 학습
- **Input** is 32 x 32 x 31 image stack consisting  of 31 feature planes
![]({{site.baseurl}}/images/temp/dc-1.png){:class="center-block"}
- **Output** is 32 x 32 x 2 image with a probability for each shot
![]({{site.baseurl}}/images/temp/dc-2.png){:class="center-block"}
- **Network architecture**
![]({{site.baseurl}}/images/temp/dc-3.png){:class="center-block"}
- There exists several problem of Policy Network
  - Most of shots deliver stone in the house (only small number of take out shot in the game)
  - In the case take out shot their can be many candidates. Thus, It  has relatively small probability compared to shot of other strategy
  - Shot data is imbalanced
  - There must be existing the loss of variety of the strategies. (continuous -> 32x32)
  - The optimal shot from our policy net might not consider the riskiness of the strategy by the shot uncertainty. $x$

**Data Analysis**

![]({{site.baseurl}}/images/temp/dc-4.png){:class="center-block"}
![]({{site.baseurl}}/images/temp/dc-5.png){:class="center-block"}

**Training Value Network**

- Assumption
  - Learning expected VALUE given game configuration is much easier than learning where to shot the stone
- Problem
  - HIGH VARIANCE of the score makes training difficult
- To overcome problem
  - Given state $s_{t+1}$, instead of learning final score, learning from the value of next state $f_\theta(s_{t+1})$ using current value network $f_{\theta}$
  - Use virtual simulation to consider uncertainty. Thus, given $s_t$ simulation is done with $a_t + N(a_t, \Sigma)$ to move to $s_{t+1}$
  - Multi task learning (one network learns both policy and value)
- Another Problem
  - We have to consider infinite number of actions to choose action since $\pi(s_t) = \text{argmax}_a \Sigma_a p(s_{t+1}|s_t, a)[f_{\theta}(s_{t+1})]$
  - Thus, We decide to use **Policy Net + Value Net + Continuouse Search**

------------------------------------------
![]({{site.baseurl}}/images/temp/dc-6.png){:class="center-block"}

**알고리즘**

1. $\text{Initialize}$ $k$ $\text{actions using Policy Network and evaluate using virtual simulation}$
$\;\;a_{1,t} = \text{argmax}_{a\in \text{grid}}P(a|s_t)$
$\;\;a_{2,t} = \text{argmax}_{a\in (\text{grid}-a_{1,t})}P(a|s_t)$
...
$\;\;v_{i,t}=f(s_{t+1}|s_t, a_{i,t})$
2. $\text{Repeat}$ $n$ $\text{times}$
    $\;\;\;\text{Select action based on UCB (Upper Confidence Bound).}$
    $\;\;\;\text{Here, expected value is calculated by Kernel Regression}$
    $\;\;\;\; a_{selected, t} = \text{argmax}_{a \in A} E[v_t|a] + C \sqrt{\frac{\log\Sigma_{b\in A}W(b)}{W(a)}}$
    $\;\;\;\;\text{where,}\; E[v_t|a] = \frac{\Sigma_{b\in A}K(a,b)v_{b,t}}{\Sigma_{b \in A}K(a,b)}$
    $\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;W(a)=\Sigma_{b \in A}K(a,b)$
    $\;\;\;\text{Sample a new action from known gaussian distribution (which is known) and evaluate}$
    $\;\;\;\; a_{sample,t} \sim N(a_{selected,t, \Sigma})$
    $\;\;\;\; v_{sample,t}=f(s_{t+1}|s_t, a_{sample,t})$
3. $\text{Select best action}$ $a_{best, t}$ $\text{based on LCB (Lower Confidence Bound)}$
$\;\;\;\; a_{best, t} = \text{argmax}_{a \in A} E[v_t|a] - C \sqrt{\frac{\log\Sigma_{b\in A}W(b)}{W(a)}}$

------------------------------------
**Improve performance with Self-Play**

- Self-Play
  - Use previous algorithm, collect data using self-play games
  - At each move, the following information is stored
    - The game state $s_t$
    - The best action $a_{best, t} = \text{argmax}_{a \in A} E[v_t|a] - C \sqrt{\frac{\log\Sigma_{b\in A}W(b)}{W(a)}}$
    - The expected value of the best action using kernel Regression $v_{best,t}=f(s_{t+1}|s_t, a_{best,t})$
    - final score $z_t$
  - Here, label of value can be $\alpha z_t + (1-\alpha)v_{best,t}$
- Retrain Network
  - Sample a mini-batch from the last 1000,000 game states
  - Retrain the current neural network on these game states



참고문헌
-------
[^1]: T. Yee, Timothy, V. Lisy, M. Bowling, Monte Carlo Tree Search in Continuous Action Spaces with Execution Uncertainty, International Joint Conference on Artificial Intelligence (IJCAI), 2016.
[^2]: Masahito Yamamoto, Shu Kato, and Hiroyuki Iizuka. Digital curling strategy based on game tree search. In Computational Intelligence and Games (CIG), 2015 IEEE Conference on, pages 474–480.IEEE, 2015.
[^3]: D. Silver etc al., Mastering the game of Go with deep neural networks and tree search,
Nature, 2016.
