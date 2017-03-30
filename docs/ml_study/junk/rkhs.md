---
layout: page-sidenav
group: "이것 저것"
title: "RKHS (Reproducing Kernel Hilbert Sapce)"
---

> <http://www.gatsby.ucl.ac.uk/~gretton/coursefiles/rkhscourse.html>

# Hibert Space
--------------

**Hilbert Space**
- **Definition**: A *Hilbert Space* is an inner product space that is complete and separable with respect to the
norm defined by the inner product.
- Hibert Space는 Inner Product 가 정의된 Vector Space 이다.
	- 참고로 Inner prodeuct 는 다음과 같은 특성이 있는데,
		- Linear : $$\langle\alpha_1 f_1 + \alpha2 f_2, g\rangle_\mathcal{H} = \alpha_1\langle f_1 , g\rangle_\mathcal{H} + \alpha_2\langle f_2, g\rangle_\mathcal{H}$$
		- Symmetric : $$\langle f, g\rangle_\mathcal{H} = \langle g, f\rangle_\mathcal{H}$$
		- positive definite : $$\langle f, f\rangle_\mathcal{H} \ge 0$$
- 또한 inner product 에따라 정의되는 Norm 에 대하여 Complete 하다. (즉, 공간의 경계 또는 안에서 모든 점들이 빠지지 않고 존재)
- 즉, 일반적으로 우리가 사용하는 Euclidean Space 를 무한 차원 (complex) vector space 로 확장한 것이다.
- 아직 낯설지만, 우리가 일반적으로 유클리디안 공간에서 다루는 데이터들을 필요에 의해 nonlinear mapping 을 통해 매우 차원이 큰 공간으로 옮긴다 생각해보자.
- 또한 그 새로운 공간에서는 두 벡터의 유사도(관계)를 나타내는 inner product 도 정의 되야 할 것이다.

# RKHS (Reproducing Kernel Hilbert Sapce)
-----------------------------------------

- RKHS 를 살펴보기전에 Kernel 에 대해 알아보자.
- $$X$$ 의 두원소를 $$\mathbb{R}$$ 공간으로 ampping 하는 함수 $$k$$ 가 있다고 하자.

$$k : X \times X \to \mathbb{R}$$

- $$\mathbb{R}$$-Hilbert space 를 $$\mathcal{H}$$ 라 하자. 모든 $$x,x' \in X$$ 에 대해서 다음과 같은 조건을 만족하는 mapping $$\phi: X \to \mathcal{H}$$ 가 존재한다면 $$k$$ 를 kernel 이라 부른다.

$$k(x,x') := \langle \phi(x),\phi(x')\rangle_{\mathcal{H}}$$

- 즉 kernel 은 Hilbert space 에서 inner product 로 정의 될 수 있어야 한다.
- 이때 kernel 은 다음과 같은 특성을 가진다.
	- Symmetric : $$k(x,y) = k(y,x)$$
	- $$k$$ 로 이루어진 Gram matrix 가 positive semi-difinite
	- $$k(x,y) \ge 0$$ [^1]
	- $$k(x,y) \le \sqrt{k(x,x)k(y,y)}$$ [^2]
	- $$(k_1+k_2)(\cdot,\cdot) = k_1(\cdot,\cdot)+k_2(\cdot,\cdot)$$
	- $$(k_1 \times k_2)(\cdot,\cdot) = k_1(\cdot,\cdot) \times k_2(\cdot,\cdot)$$, 즉, 커널은 곱과 합에 닫혀있다.
- 쉽게 풀어쓰면, 커널은 데이터 두개를 받아서 실수로 바꾸어 주는 함수인 동시에, 두 데이터를 어떠한 nonlinear mapping 을 통해 Hilbert Space 보내고, 그 공간에서의 관계를 나타내는 inner product  값이다.
- 그렇다면 RKHS 에서의 Reproducing Kernel 이 무엇을 의미할까?

**Reproducing Kernel Hilbert Space (RKHS)**

> **Reproducing Kernel Hilbert Space**
> A Hilbert Space $$H$$ of functions $$f:\mathcal{X} \to \mathbb{R}$$ is called Reproducing Kernel Hilbert Space if the evaluation functional $$L_x: H \to \mathbb{R}$$ is a bounded ($$\iff$$ continuous) linear operator for all $$x \in \mathcal{X}$$

- Evaluation functional $$L_x$$ 는 함수를 인자로 받는 범함수이면서 $$L_x(f) = f(x)$$ 인 functional
	- 즉 함수를 인풋으로 받아 $$x$$ 에서의 값을 evaluation
- 이때 $$L_x$$ 가 Bounded Linear Operator 이면, Continuous operator 이고[^3], **Reisz Representation Theorem** 을 적용 할 수 있다.

> **Reisz Representation Theorem**
> Let $$H$$ be a Hilbert space and $$H^*$$ be its dual space consisting of all **continuous linear functionals** $$H \to \mathbb{R}$$. Define $$\varphi_f \in H^* : H \to \mathbb{R}$$ for $$f \in H$$ as,
> 
> $$\begin{align} \varphi_f(\cdot) \equiv \langle f, \cdot \rangle_H \end{align}$$
>
> Then, the mapping $$\Phi: H \to H^*$$ defined by $$\Phi(f) \equiv \varphi_f$$ is an **isometric isomorphism**.

- 위의 Theorem 이 말하길, 
- 예를들어, dual space $$\mathcal{H}^*$$ 에는 continuous linear functionals $$L_x$$ 가 살고 있다고 해보자.
- 그렇다면 Hilbert Space $$\mathcal{H}$$ 에 정의된 function $$f$$(무한차원 vector) 를 dual space $$\mathcal{H}^*$$ 에서 $$\langle f,\cdot\rangle_\mathcal{H}$$ 로 정의할 수 있고, $$\mathcal{H}$$ 와 $$\mathcal{H}^*$$ 사이에 isomorphism 이 존재한다.
	- 달리 말하면, dual space $$\mathcal{H^*}$$ 에서의 functional $$f$$ 는 **"hilbert space 에서 $$f$$ 를 ~와 inner product 하는 것"** 
	- 위의 말을 수식으로 풀면

$$L_x(\cdot)=\langle K_x, \cdot \rangle_{\mathcal{H}}, \text{where } K_x \in H$$

- functional 에 $$f$$ 를 대입하면,

$$L_x(f)=f(x)=\langle K_x, f \rangle_{\mathcal{H}} = \langle f, K_x, \rangle_{\mathcal{H}}$$

- 아래 와 같은 Reproducing Property 를 만족한다.

**Reproducing Property of Kernel**

> **Reproducing Property of Kernel**
> Consider Hilbert space $$H$$ of functions $$f:\mathcal{X} \to \mathbb{R}$$ and kernel function $$k:\mathcal{X} \times \mathcal{X} \to \mathbb{R}$$ such that $$k(\cdot, x) \in H, \forall x \in \mathcal{X} $$. The reproducing property is defined by,
> 
> $$\begin{align} \langle f, k(\cdot, x) \rangle_H = f(x) \end{align}, \forall x \in \mathcal{X}$$

- $$f$$ 에 $$K_y$$ 를 대입해보면

$$k(x,y) = \langle K_x, K_y \rangle_\mathcal{H}$$

- Kernel 을 위와 같이 정의하면 Reproducing Kernel 이 된다.
- 이런 Reproducing Kernel 이 정의된 Hilbert Space 를 Reproducing Kernel Hilbert Space (RKHS) 라 부른다.
- Reproducing Kernel 이 Hilbert Space 에서 inner product 로 나타내지는걸 이용하면 Reproducing Kernel 이 Positive-definite 함을 보일 수 있다.
- 역으로, Kernel 이 Positive-definite 하면 RKHS 를 유도 하는 것 도 가능하다. (즉, Positive-definite Kernel 에 대해서 RKHS 의 성질을 이용하여 이론 전개 가능)

참고
---

[^1]: Gram matrix size 가 1인 경우에도 positive semidefinite
[^2]: Cauchy-Schwarz inequality. $$2 \times 2$$ 행렬의 positive semi-difinite 한걸 보이는 식으로 증명 가능
[^3]: <https://en.wikipedia.org/wiki/Bounded_operator#Equivalence_of_boundedness_and_continuity>

