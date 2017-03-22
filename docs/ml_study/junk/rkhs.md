---
layout: page-sidenav
group: "이것 저것"
title: "RKHS (Reproducing Kernel Hilbert Sapce)"
---

> http://www.gatsby.ucl.ac.uk/~gretton/coursefiles/lecture4_introToRKHS.pdf

# Hibert Space
--------------

**Hilbert Space**
- **Definition**: A *Hilbert Space* is an inner product space that is complete and separable with respect to the
norm defined by the inner product.
- Hibert Space는 Inner Product 가 정의된 Vector Space 이다.
	- 참고로 Inner prodeuct 는 다음과 같은 특성이 있는데,
		- Linear : $\langle\alpha_1 f_1 + \alpha2 f_2, g\rangle_\mathcal{H} = \alpha_1\langle f_1 , g\rangle_\mathcal{H} + \alpha_2\langle f_2, g\rangle_\mathcal{H}$
		- Symmetric : $\langle f, g\rangle_\mathcal{H} = \langle g, f\rangle_\mathcal{H}$
		- positive definite : $\langle f, f\rangle_\mathcal{H} \ge 0$
- 또한 inner product 에따라 정의되는 Norm 에 대하여 Complete 하다. (즉, 공간의 경계 또는 안에서 모든 점들이 빠지지 않고 존재)
- 즉, 일반적으로 우리가 사용하는 Euclidean Space 를 무한 차원 (complex) vector space 로 확장한 것이다.
- 아직 낯설지만, 우리가 일반적으로 유클리디안 공간에서 다루는 데이터들을 필요에 의해 nonlinear mapping 을 통해 매우 차원이 큰 공간으로 옮긴다 생각해보자.
- 또한 그 새로운 공간에서는 두 벡터의 유사도(관계)를 나타내는 inner product 도 정의 되야 할 것이다.

# RKHS (Reproducing Kernel Hilbert Sapce)
-----------------------------------------

- RKHS 를 살펴보기전에 Kernel 에 대해 알아보자.
- $X$ 의 두원소를 $\mathbb{R}$ 공간으로 ampping 하는 함수 $k$ 가 있다고 하자.

$k : X \times X \to \mathbb{R}$

- $\mathbb{R}$-Hilbert space 를 $\mathcal{H}$ 라 하자. 모든 $x,x' \in X$ 에 대해서 다음과 같은 조건을 만족하는 mapping $\phi: X \to \mathcal{H}$ 가 존재한다면 $k$ 를 kernel 이라 부른다.

$k(x,x') := \langle \phi(x),\phi(x')\rangle_{\mathcal{H}}$

- 즉 kernel 은 Hilbert space 에서 inner product 로 정의 될 수 있어야 한다.
- 이때 kernel 은 다음과 같은 특성을 가진다.
	- Symmetric : $k(x,y) = k(y,x)$
	- $k$ 로 이루어진 Gram matrix 가 positive semi-difinite
	- $k(x,y) \ge 0$ [^1]
	- $k(x,y) \le \sqrt{k(x,x)k(y,y)}$ [^2]
	- $(k_1+k_2)(\cdot,\cdot) = k_1(\cdot,\cdot)+k_2(\cdot,\cdot)$
	- $(k_1 \times k_2)(\cdot,\cdot) = k_1(\cdot,\cdot) \times k_2(\cdot,\cdot)$, 즉, 커널은 곱과 합에 닫혀있다.
- 쉽게 풀어쓰면, 커널은 데이터 두개를 받아서 실수로 바꾸어 주는 함수인 동시에, 두 데이터를 어떠한 nonlinear mapping 을 통해 Hilbert Space 보내고, 그 공간에서의 관계를 나타내는 inner product  값이다.
- 그렇다면 Reproducing Kernel 이란 무엇일까?

**Reproducing Property of Kernel**

참고
---

[^1]: Gram matrix size 가 1인 경우에도 positive semidefinite
[^2]: Cauchy-Schwarz inequality. $2 \times 2$ 행렬의 positive semi-difinite 한걸 보이는 식으로 증명 가능

 수학적으로 커널은 데이터 두 개를 받아서 실수로 바꿔주는 함수이다. 기계 학습 수업들에서는 데이터를 임의의 고차원 공간으로 보낸 후에 해당 공간에서 내적을 해주는 것이라고 설명을 하기도 한다. 물론 맞는 말이지만 이것 보다는 좀 더 복잡한 내용들이 존재한다. 예를 들면 과연 고차원 공간으로 보내주는 매핑이 존재하는가? 이런 것 말이다. 