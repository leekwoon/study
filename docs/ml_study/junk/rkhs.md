---
layout: page-sidenav
group: "이것 저것"
title: "RKHS (Reproducting Kernel Hilbert Sapce)"
---

# Hibert Space

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

# RKHS (Reproducting Kernel Hilbert Sapce)

- RKHS 를 살펴보기전에 Kernel 에 대해 알아보자.
- $X$ 의 두원소를 $\mathbb{R}$ 공간으로 ampping 하는 함수 $k$ 가 있다고 하자.

$k : X \times X \to \mathbb{R}$

- $\mathbb{R}$-Hilbert space 에서 다음과 같은 조건을 만족하는 mapping $\phi: X \to \mathcal{H}$ 가 존재한다면 k 를 kernel 이라 부른다.

$k(x,x') := \langle \phi(x),\phi(x')\rangle_{\mathcal{H}}$

- 즉 kernel 은 Hilbert space 에서 inner product 로 정의 될 수 있어야 한다.
- 이때 kernel 은 다음과 같은 특성을 가진다.
	- Symmetric : $k(x,y) = k(y,x)$
	- $k$ 로 이루어진 Gram matrix 가 positive semi-difinite
	- $k(x,y) \ge 0$ [^Banach]


---

[^Banach]: Gram matrix size 가 1인 경우에도 positive semidefinite