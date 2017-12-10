---
layout: page-sidenav
group: "Reinforcement Learning: An Introduction"
title: 3. Finite Markov Decision Processes
---

# Finite Markov Decision Processes

Background of math
------------------
**Conditional Probability**
- Given two events $A$ and $B$ with $p(B) > 0$, the **conditional probability** of $A$ given $B$ is

$$p(A\vert B) = \frac{p(A \cap B)}{p(B)}$$

- Similarly, if $X$ and $Y$ are <u>*non-degenerate*</u>[^1] and <u>*jointly continuous random variables*</u> with density $f_{X,Y}(x,y)$ then if $B$ has positive measure then the **conditional probability** is

$$p(X \in A \vert Y \in B)=\frac{\int_{y \in B}\int_{X \in A}f_{X,Y}(x,y)dxdy}{\int_{y \in B}\int_{X}f_{X,Y}(x,y)dxdy}$$

**Law of total expectation**[^2]
- if $X$ is random variable whose expected value $\mathbb{E}(X)$ is defined, and $Y$ is any random variable on the same probability space, then

$$\mathbb{E}[Y]=\mathbb{E}[\mathbb{E}[Y\vert X=x]]$$

- i.e., the expected value of the conditional expected value of $X$ given $Y$ is the same as the expected value of $X$
- Further more, given a function $f$ and two random variables $X, Y$ we have that

$$\mathbb{E}_{X,Y}[f(X,Y)]=\mathbb{E}_X[\mathbb{E}_Y[f(x,Y)\vert X=x]]$$

**Norms**
- Given a vector space $V \subseteq \mathbb{R}^d$ a function $f:V \to \mathbb{R_0}^+$ is a **norm** if and only if
  - <span style="color:red">Definite:</span> If $f(v)=0$ for some $v\in V$, then $v=0$
  - <span style="color:red">Absolutely scalable:</span> For any $\lambda\in \mathbb{R}, v\in V, \; f(\lambda v)=\vert\lambda \vert f(v)$
  - <span style="color:red">Triangle inequality:</span> For any $v,y \in V, f(v+u) \le f(v)+f(u)$
- e.g., $L_p$-norm

$$\vert\vert v\vert\vert_p=\left(\sum_{i=1}^d \vert v_i\vert^p \right)^{1/p}$$

**Converge in norm**[^3]
- A sequence of vector $v_n\in V$ (with $n \in \mathbb{N}$) is said to converge in norm $\vert\vert\cdot\vert\vert$ to $v\in V$ if

$$\lim_{n\to \infty}\vert\vert v_n - v\vert\vert=0$$

**Cauchy sequence**
- A sequence of vector $v_n\in V$ (with $n \in \mathbb{N}$) is a **Cauchy sequence** if

$$\lim_{n\to \infty}\sup_{n\ge n}\vert\vert v_n-v_m\vert\vert=0$$

- 모든 converge sequence는 cachy sequence 이지만, cauchy sequence라고 항상 converge한 것은 아니다.[^4]

**Complete**
- A vector space $V$ equipped with a norm $\vert\vert\cdot\vert\vert$ is **complete** if every Cauchy sequence in $V$ is convergent in the norm of the space.
- 즉, 공간안의 모든 cauchy sequence가 수렴하는 극한이 여전히 그 공간안에 있을때 공간이 **complete** 하다고 말한다.

**L-Lipschits (non-expansion + L-contraction)**
- An operator $\mathcal{T}:V\to V$ is **L-Lipschitz** if for any $v,u \in V$

$$\vert\vert\mathcal{T}v - \mathcal{T}u\vert\vert \le L\vert\vert u-v \vert\vert$$

- If $L \le 1 $ then $\mathcal{T}$ is **non-expansion**, while if $L<1$ then $\mathcal{T}$ is a **L-contraction**
- If $\mathcal{T}$ is Lipschitz then it is also <span style=color:red>continuous</span>, that is
  - if $v_n \to_{\vert\vert\cdot\vert\vert}v$ then $\mathcal{T}v_n \to_{\vert\vert\cdot\vert\vert}\mathcal{T}v$

**Fixed point**
- A vector $v\in V$ is a fixed point of the operator $\mathcal{T}:V\to V$ if $\mathcal{T}v=v$

**Banach Fixed Point Theorem**
- Let $V$ be a <span style=color:red>complete</span> vector space equipped with the norm $\vert\vert\cdot\vert\vert$ and $\mathcal{T}:V\to V$ be a <span style=color:red>$\gamma$-contraction</span> mapping. Then
1. $\mathcal{T}$ admits a <span style=color:red>unique fixed point</span> $v$
2. For any $v_0 \in V$, if $v_{n+1}=\mathcal{T}v_n$ then $v_n\to_{\vert\vert\cdot\vert\vert}v$ with a <span style=color:red>geometric convergence rate:</span>

$$\vert\vert v_n-v\vert\vert \le \gamma^n\vert\vert v_0-v \vert\vert$$


----------
[^1]: $X$ is degenerate if, for some $a \in \mathbb{R}, p(X=a)=1$. A random variable that is not degenerate is called non-degenerate.
[^2]: See [proof](https://en.wikipedia.org/wiki/Law_of_total_expectation#Proof_in_the_general_case)
[^3]: See [Wasserstein GAN 수학 이해하기 I ](https://www.slideshare.net/ssuser7e10e4/wasserstein-gan-i)
[^4]: For example, $a_n=(1+\frac{1}{n})^n$ is a sequence of rationals, but its limit $e$ is not rational
