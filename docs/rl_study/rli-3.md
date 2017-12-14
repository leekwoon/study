---
layout: page-sidenav
group: "Reinforcement Learning: An Introduction"
title: 3. Finite Markov Decision Processes
---

# Finite Markov Decision Processes
>- ~~Here, we assume <span style=color:red>dicrite time finite</span> MDP to simplify the problem. In this case, the set of states, actions and rewards ($$\mathcal{S},\mathcal{A},\mathcal{R}$$) all have a finite number of elements.~~
>- <span style=color:red>Stationary</span> assumption: the dynamics and reward do not change over time. For example, transition probability $$p({s'}|{s,a})$$ and reward of transition $$r(s,a,s')$$ do not depends on the time $$t$$ (See below)

Background of math
------------------
**Conditional Probability**
- Given two events $$A$$ and $$B$$ with $$p(B) > 0$$, the **conditional probability** of $$A$$ given $$B$$ is

$$$$\mathbb{P}(A\vert B) = \frac{\mathbb{P}(A \cap B)}{\mathbb{P}(B)}$$$$

- Similarly, if $$X$$ and $$Y$$ are <u>*non-degenerate*</u>[^1] and <u>*jointly continuous random variables*</u> with density $$f_{X,Y}(x,y)$$ then if $$B$$ has positive measure then the **conditional probability** is

$$$$p(X \in A \vert Y \in B)=\frac{\int_{y \in B}\int_{X \in A}f_{X,Y}(x,y)dxdy}{\int_{y \in B}\int_{X}f_{X,Y}(x,y)dxdy}$$$$

**Law of total expectation**[^2]
- if $$X$$ is random variable whose expected value $$\mathbb{E}(X)$$ is defined, and $$Y$$ is any random variable on the same probability space, then

$$$$\mathbb{E}[Y]=\mathbb{E}[\mathbb{E}[Y\vert X=x]]$$$$

- i.e., the expected value of the conditional expected value of $$X$$ given $$Y$$ is the same as the expected value of $$X$$
- Further more, given a function $$f$$ and two random variables $$X, Y$$ we have that

$$$$\mathbb{E}_{X,Y}[f(X,Y)]=\mathbb{E}_X[\mathbb{E}_Y[f(x,Y)\vert X=x]]$$$$

**Norms**
- Given a vector space $$V \subseteq \mathbb{R}^d$$ a function $$f:V \to \mathbb{R_0}^+$$ is a **norm** if and only if
  - <span style="color:red">Definite:</span> If $$f(v)=0$$ for some $$v\in V$$, then $$v=0$$
  - <span style="color:red">Absolutely scalable:</span> For any $$\lambda\in \mathbb{R}, v\in V, \; f(\lambda v)=\vert\lambda \vert f(v)$$
  - <span style="color:red">Triangle inequality:</span> For any $$v,y \in V, f(v+u) \le f(v)+f(u)$$
- e.g., $$L_p$$-norm

$$$$\vert\vert v\vert\vert_p=\left(\sum_{i=1}^d \vert v_i\vert^p \right)^{1/p}$$$$

**Converge in norm**[^3]
- A sequence of vector $$v_n\in V$$ (with $$n \in \mathbb{N}$$) is said to converge in norm $$\vert\vert\cdot\vert\vert$$ to $$v\in V$$ if

$$$$\lim_{n\to \infty}\vert\vert v_n - v\vert\vert=0$$$$

**Cauchy sequence**
- A sequence of vector $$v_n\in V$$ (with $$n \in \mathbb{N}$$) is a **Cauchy sequence** if

$$$$\lim_{n\to \infty}\sup_{n\ge n}\vert\vert v_n-v_m\vert\vert=0$$$$

- 모든 converge sequence는 cachy sequence 이지만, cauchy sequence라고 항상 converge한 것은 아니다.[^4]

**Complete**
- A vector space $$V$$ equipped with a norm $$\vert\vert\cdot\vert\vert$$ is **complete** if every Cauchy sequence in $$V$$ is convergent in the norm of the space.
- 즉, 공간안의 모든 cauchy sequence가 수렴하는 극한이 여전히 그 공간안에 있을때 공간이 **complete** 하다고 말한다.

**L-Lipschits (non-expansion + L-contraction)**
- An operator $$\mathcal{T}:V\to V$$ is **L-Lipschitz** if for any $$v,u \in V$$

$$$$\vert\vert\mathcal{T}v - \mathcal{T}u\vert\vert \le L\vert\vert u-v \vert\vert$$$$

- If $$L \le 1 $$ then $$\mathcal{T}$$ is **non-expansion**, while if $$L<1$$ then $$\mathcal{T}$$ is a **L-contraction**
- If $$\mathcal{T}$$ is Lipschitz then it is also <span style=color:red>continuous</span>, that is
  - if $$v_n \to_{\vert\vert\cdot\vert\vert}v$$ then $$\mathcal{T}v_n \to_{\vert\vert\cdot\vert\vert}\mathcal{T}v$$

**Fixed point**
- A vector $$v\in V$$ is a **fixed point** of the operator $$\mathcal{T}:V\to V$$ if $$\mathcal{T}v=v$$

**Banach Fixed Point Theorem**
- Let $$V$$ be a <span style=color:red>complete</span> vector space equipped with the norm $$\vert\vert\cdot\vert\vert$$ and $$\mathcal{T}:V\to V$$ be a <span style=color:red>$$\gamma$$-contraction</span> mapping. Then
1. $$\mathcal{T}$$ admits a <span style=color:red>unique fixed point</span> $$v$$
2. For any $$v_0 \in V$$, if $$v_{n+1}=\mathcal{T}v_n$$ then $$v_n\to_{\vert\vert\cdot\vert\vert}v$$ with a <span style=color:red>geometric convergence rate:</span>

$$$$\vert\vert v_n-v\vert\vert \le \gamma^n\vert\vert v_0-v \vert\vert$$$$

MDP
---

**Agent Environment Model**
![]({{site.baseurl}}/s/rl_study/rli-1.1.PNG){:class="center-block"}[^5]
- Agent learns a way to achieve a goal by interacting with environment
- Agent and environment interact at each of sequence of <span style=color:red>discrete time</span> steps, $$t=0,1,2,3,...$$
  - At each time step $$t$$, the agent receives some representation of the environment's state $$S_t \in \mathcal{S}$$, and on that basis select an action $$A_t \in \mathcal{A}(s)$$
  - One time step later, as a consequence of its action, agent receive new state $$S_{t+1}$$ and reward $$R_{t+1}$$
- Several properties of environment
  - Controllability
    - fully (e.g., chess)
    - partially (e.g., portfolio optimization)
  - Uncertainty
    - determinisitic (e.g., chess)
    - stochastic (e.g., backgammon)
  - Reactive
    - adversarial (e.g., chess)
    - fixed (e.g., Tetris)
  - Observavility
    - full (e.g., chess)
    - partial (e.g., robotics)
  - Availability
    - known (e.g., chess)
    - unknown (e.g., robotics)
- When agent can directly observes environment state, formally, this is a MDP
- When agent indirectly observes environment state, formally, this is a PO-MDP (Partially observable Markov decision process)

**Markov Chain (=Markov process/memory-less random process)**
- Let the state space $$\mathcal{S}$$ be a bounded compact[^6] subset of the Euclidean space, the discrete-time dynamic system $${(s)}_{t\in \mathbb{N}} \in S$$ is a **Markov chain** if it satisfies the **Markov property**

$$$$\mathbb{P}({s_{t+1}=s'}|{s_t, s_{t-1}, ..., s_0})=\mathbb{P}({s_{t+1}=s'}|{s_t})$$$$

- Given an initial state $$s_0\in \mathcal{S}$$, a Markov chain is defined by the trainsition probability $$p$$

$$$$p({s'}|{s})=\mathbb{P}({s_{t+1}=s'}|{s_t=s})$$$$

- Given that we start in state $$s_i$$, let $$T_i$$ be the first return time to state $$s_i$$ (**hitting time**)

$$$$T_i = \inf\{n\ge 1: s_n=i \vert s_0=i\}$$$$

- and **mean recurrence time** is

$$$$M_i=E[T_i]$$$$

- the state $$s_i$$ is
  - transient: if $$\mathbb{P}(T_i < \infty) < 1$$
    - There is non-zero probability that we will never return to $$s_i$$
  - recurrent: not transient
    - positive recurrent: $$M_i$$ is **finite**
      - must return to $$s_i$$ in **finite** time
    - null recurrent: $$M_i$$ is infinite

**Markov decision process**
- Classical(+mathematical) formalization of sequentail decision making, where a actions influence not just immediate rewards, but also subsequent states and future rewards
- Straightforward framing of the problem of learning from interaction to achieve a goal
- A **Markov decision process** is defined as a tuple ($$\mathcal{S},\mathcal{A},p,r,\gamma$$)
  - $$\mathcal{S}$$ is a set of states
  - $$\mathcal{A}$$ is a set of actions
  - $$p$$ is a state transition probability with $$p({s'}|{s,a})=\mathbb{P}({s_{t+1}=s'}|{s_t=s, a_t=a})$$
  - $$r$$ is a reward of transition with $$r(s,a,s')=\mathbb{E}[{R_t}|{S_{t-1}=s, A_{t-1}=a,S_t=s'}]$$
  - $$\gamma$$ is a discount factor, $$\gamma \in [0,1)$$
<!-- - Furthermore, $$r(s,a)=\sum_{s'\in \mathcal{S}}r(s,a,s')p({s'}|{s,a})$$ -->

**Return**
- The **return** $$G_t$$ is total discounted reward from time-step $$t$$

$$$$G_t=R_{t+1}+\gamma R_{t+2}+...=\sum_{k=0}^{\infty}\gamma^k R_{t+k+1}$$$$

**Policy**
- A **policy** $$\pi$$ is a distribution over actions given states

$$$$\pi(a|s)=\mathbb{P}[{A_t=a}|{S_t=s}]$$$$
- If policy is <u>Deterministic</u> $$\pi_t(s)=a$$
- If policy is <u>Stationary</u> $$\pi_t=\pi$$

**State-Value**
- The **state-value** function $$v_{\pi}(s)$$ of an MDP is the <u>expected return</u> starting from state $$s$$, and then following policy $$\pi$$

$$$$v_{\pi}(s)=\mathbb{E}_{\pi}[G_t \vert S_t=s]$$$$

- and, formally (for infinite time horizon with discount)

$$$$v_{\pi}(s)=\mathbb{E}_{\pi}\left[ \sum_{k=0}^{\infty}\gamma^{k}R_{t+k+1} \right]$$$$

**Action-value**
- The **action-value** function $$q_{\pi}(s, a)$$ is the <u>expected return</u> starting from state $$s$$, <span style="color:red">taking action $$a$$, and then following policy $$\pi$$</span>

$$$$q_{\pi}(s, a)=\mathbb{E}_{\pi}\left[ G_t \vert S_t=s, \color{red}{A_t=a} \right]$$$$

**Optimal policy and Optimal value function**
- The solution to an MDP is an **optimal policy** $$\pi^{\star}$$ satisfying

$$$$\pi^{\star} \in arg\max_{\pi \in \Pi}V^{\pi}$$$$

- in all the states $$s\in \mathcal{S}$$, where $$\Pi$$ is some policy set of interest.
  - there can be <u>more than one</u> optimal policy
  - there always exists an optimal <u>deterministic</u> policy
- The corresponding value function is the **optimal value function**

$$$$V^{\star}=V^{\pi^{\star}}$$$$

Bellman Equation
----------------
- Here, we assume <u>stationary</u> policy

**(1-step backup diagram)** for $$v_{\pi}(s)$$
![]({{site.baseurl}}/s/rl_study/rli-3.1.PNG){:class="center-block"}

$$$$v_{\pi}(s)=\sum_{a}\pi(a\vert s)q_{\pi}(s, a)$$$$

**(1-step backup diagram)** for $$q_{\pi}(s, a)$$
![]({{site.baseurl}}/s/rl_study/rli-3.2.PNG){:class="center-block"}

$$$$q_{\pi}(s, a)=\sum_{s'}p(s'|s,a)\left[r(s,a,s')+\gamma v_{\pi}(s')\right]$$$$

**(2-step backup diagram)** for $$v_{\pi}(s)$$
![]({{site.baseurl}}/s/rl_study/rli-3.3.PNG){:class="center-block"}

$$$$v_{\pi}(s)=\sum_{a}\pi(a\vert s)\sum_{s'}p(s'|s,a)\left[r(s,a,s')+\gamma v_{\pi}(s')\right]$$$$

**(2-step backup diagram)** for $$q_{\pi}(s, a)$$
![]({{site.baseurl}}/s/rl_study/rli-3.4.PNG){:class="center-block"}

$$$$q_{\pi}(s, a)=\sum_{s'}p(s'|s,a)\left[r(s,a,s')+\gamma \sum_{a'}\pi(a'\vert s')q_{\pi}(s', a')\right]$$$$

**Optimal Value Function**
- The **optimal state-value** function v_{\star}(s) is the maximum value function over all polices

$$$$v_{\star}=\max_{\pi}v_{\pi}(s)=\max_{\color{red}{a}}\sum_{a}\pi(a\vert s)\sum_{s'}p(s'|s,a)\left[r(s,a,s')+\gamma v_{\pi}(s')\right]$$$$
$$$$=\max_{\color{red}{a}}\sum_{s'}p(s'|s,a)\left[r(s,a,s')+\gamma v_{\pi}(s')\right]$$$$
The **optimal action-value** function $$q_{\star}(s,a)$$ is the maximum action-value function over all polices

$$$$q_{\star}(s,a)=\max_{\pi}q_{\pi}(s,a)=\sum_{s'}p(s'|s,a)\left[r(s,a,s')+\gamma\max_{\color{red}{a'}}\sum_{a'}\pi(a'\vert s')q_{\pi}(s', a')\right]$$$$
$$$$=\sum_{s'}p(s'|s,a)\left[r(s,a,s')+\gamma\max_{\color{red}{a'}}\sum_{a'}q_{\pi}(s', a')\right]$$$$

References
----------
[^1]: $$X$$ is degenerate if, for some $$a \in \mathbb{R}, p(X=a)=1$$. A random variable that is not degenerate is called non-degenerate.
[^2]: See [proof](https://en.wikipedia.org/wiki/Law_of_total_expectation#Proof_in_the_general_case)
[^3]: See [Wasserstein GAN 수학 이해하기 I ](https://www.slideshare.net/ssuser7e10e4/wasserstein-gan-i)
[^4]: For example, $$a_n=(1+\frac{1}{n})^n$$ is a sequence of rationals, but its limit $$e$$ is not rational
[^5]: https://www.slideshare.net/carpedm20/reinforcement-learning-an-introduction-64037079
[^6]: The meaning of compact is same as colsed and bounded
