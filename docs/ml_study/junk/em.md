---
layout: page-sidenav
group: "이것 저것"
title: "EM algorithm"
---

> EM algorithm 에 대해 알아보고 Gaussian mixture model 에 적용해보자.
> 그전에 우선 Kullback-Leibler divergence 을 알아보자.

Kullback-Leibler divergence
---------------------------

- 우선 엔트로피에 대해 간단히 짚어보자.
- information theory에서는 $x$의 구체적인 값을 관찰하는 경우 얼마만큼의 정보를 얻는지를 정량화한다.
- 이때 정보의 양은 $h(x)$로 나타냄.

$h(x) = -\log p(x)$

- 그리고 정보의 평균은 다음과 같이 정의 된다.

$ H[x] = -\sum_x p(x)\log p(x)$

- 위 식을 **엔트로피(entoropy)**라고 정의한다.
- 참고로, **Noiseless coding theorem (Shannon, 1948)**에 의하면...
	- 엔트로피는 랜덤 변수의 상태를 전송하는데 필요한 비트 수의 Lower Bound가 된다.
- 정확한 형태를 모르는 확률 분포 $p(X)$ 가 있을 때 이 확률 분포를 최대한 근사한 $q(X)$가 있다고 하자.
- $p(x)$가 아닌 $q(x)$를 사용했을때 추가적으로 필요한 정보량의 기대값을 정의해보자.

$KL(p\vert\vert q) = -\int p({\bf x})\ln q({\bf x})d{\bf x} - (-\int p({\bf x})\ln p({\bf x})d{\bf x }) \\ 
=  -\int p({\bf x})\ln\{\frac{q({\bf x})}{p({\bf x})}\}d{\bf x}$

- 이러한 정의를 **relative entropy** 또는 **Kullback-Leibler divergence**라고 부른다.
- 참고로 $KL(p\vert\vert q) \ne KL(q\vert\vert p)$ 이다. (non-symmetric)
- 또한 항상 $KL(p\vert\vert q) \ge 0$ 을 만족한다.
	- $p({\bf x}) = q({\bf x})$ 일때 $KL(p\vert\vert q) = 0$ 만족.
	- **옌센부등식 (Jensen's inequality)** 를 통해 증명 가능.
	- $-\ln(\cdot)$ 이 convex 함수 이므로 $f(E[x]) \le E[f(x)]$ 만족.
	- $f(x) = \ln(\frac{q({\bf x})}{p({\bf x})})$ 로 놓고 Jensen's inequality을 이용하면

$KL(p\vert\vert q) = -\int p({\bf x})\ln\{\frac{q({\bf x})}{p({\bf x})}\}d{\bf x} \ge - \ln \int q({\bf x})d{\bf x} = 0$

GMM (Gaussian Mixture Model)
----------------------------

- EM 알고리즘의 최종 목적은 잠재 변수(latent variable)를 통해 likelihood 을 최대화 하는 것이다.
- 잠재변수 모델의 예로 GMM을 살펴보자.
- GMM 의 graphical model 은 아래와 같다.

![]({{site.baseurl}}/images/ml_study/junk/em1.png){:class="center-block"}

- 간단히 말하면 GMM은 여러 개의 가우시안 분포를 선형 결합한 형태이다.
- $K$ 개의 가우시한 분포를 결합한 GMM의 경우 잠재변수 $z$ 는
	- *1-to-K* 코딩 스킴 형태의 변수다.
	- $K$ 개의 요소 중 1개의 값만 1이고 나머지는 0인 값이다.
	- 따라서 \\( z\_k \in \{0,1\} \\) 이고 \\( \sum\_k z\_k=1 \\) 이다.
	


EM algorithm
------------
