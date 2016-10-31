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
- 잠재변수와의 joint probability 를 고려해보자.
    - Graphical model에서 보듯이 

$p({\bf x}, {\bf z}) = p({\bf z})p({\bf x}\|{\bf z})$

- 이제 \\( z\_k \\) 의 marginal probability 를 혼합계수(mixing coefficients) \\( \pi\_k \\) 를 사용하여 정의할 수 있다.

$p(z_k=1) = \pi_k$


- 파라미터 \\( \pi_k \\) 는 다음과 같은 성질을 만족해야 한다.

$0 \le \pi_k \le 1$

$\sum_{k=1}^K \pi_k = 1$

- \\( {\bf z} \\) 가 *1-of-K* 코딩 스킴을 사용하기 때문에 이에 대한 확률 식을 다음과 같이 나타낼 수 있다.

$p({\bf z})=\prod_{k=1}^K \pi_k^{z_k}$

- 이와 유사하게 이제 \\( x \\) 에 대한 condtional probability 을 표현해보자. \\( x \\) 는 \\( z_k \\) 가 주어졌을 때 가우시안 분포를 따른다.

$p({\bf x}|z_k=1) = N({\bf x}|{\bf \mu_k}, \Sigma_k)$

- 이를 일반화 하면

$p({\bf x}|{\bf z}) = \prod_{k=1}^{K} N({\bf x}|{\bf \mu}_k, \Sigma_k)^{z_k} \qquad{(9.11)}$

- 이제 \\( {\bf x} \\) 에 대한 marginal probability 을 계산할 수 있다.

$p({\bf x}) = \sum_z p({\bf z})p({\bf x}|{\bf z}) = \sum_{k=1}^{K} \pi_k N({\bf x}|{\bf \mu}_k, \Sigma_k)$

- 이제 \\( {\bf x} \\) 가 주어졌을 때의 conditional probability 을 정의해보자.

$\gamma(z_k) \equiv p(z_k=1|{\bf x}) = \frac{p(z_k=1)p({\bf x}|z_k=1)}{\sum_j^K p(z_j=1)p({\bf x}|z_j=1)}=\frac{\pi_k N({\bf x}|{\bf \mu}_k, \Sigma_k)}{\sum_{j=1}^K \pi_j N({\bf x}|{\bf \mu}_j, \sigma_j)} $

- 우리는 \\( \pi\_k \\) 가 \\( z\_k=1 \\) 일 때의 prior 이라는 것을 알고 있다.
- 이제 \\( \gamma(z\_k) \\) 는 관찰 데이터 \\( {\bf x} \\) 가 주어졌을 때의 posterior probability 이 된다.
- 이후에 살펴볼 것이지만 \\( \gamma(z\_k) \\) 를 성분 \\( k \\) 에 대한 *responsibility* 라고도 한다.

- 이제 로그 가능도 함수(likelihood function)를 기술해보자.

$\ln p({\bf X}|{\bf \pi}, {\bf \mu}, \Sigma) = \sum_{n=1}^N \ln \left\{\sum_{k=1}^K \pi_k N({\bf x}_n|{\bf \mu}_k, \Sigma_k)\right\}$


EM algorithm
------------

- SGD (stochastic gradient descent)같은 방법이 있는데 굳이 EM algorithm 을 왜 알아야 할까?
- 그 이유를 알기 위해서 잠재변수를 가진 모델 학습의 어려움을 살펴보자.

![]({{site.baseurl}}/images/ml_study/junk/em2.png){:class="center-block"}

- 이전에 언급했듯이 GMM은 위의 Graphical model로 표현될 수 있다.
	- 이때 $\theta_z$ 는 $\pi$ 를 의미하고, $\theta_x$ 는 $\mu$ 와 $\sum$ 을 의미한다.
- $x_i$ 와 $z_i$ 가 관찰됬다면, D-separation에 의해서 $\theta_z$ 와 $\theta_x$ 는 independent 하고, parameter의 posterior도 factorize 될 수 있다.
- 하지만 $z_i$는 잠재변수로써 관찰이 되지 않으므로 parameter들의 posterior은 factorize 되지 않는다.
- 이는 MAP, MLE의 계산을 어렵게 만든다.
- 또한 $\sum_{k=1}^K \pi_k = 1$ 같은 제약조건은 SGD (stochastic gradient descent) 적용을 어렵게 한다.
- 잠재변수 모델에서 parameter들의 posterior을 계산할때 가장 큰 문제는 다중 봉우리(multiple mode)를 가졌다는 것이다.
	- 예를 들어 GMM에서, $z_i$ 가 관찰되었다면 unimodal posterior을 갖게되어 쉽게 parameter을 추정할 수 있다.
	- 하지만 잠재변수가 관찰 되지 않았을 경우 다양한 봉우리를 가지기 때문에 어떤 잠재변수 기준으로 optimize하는지가 중요하다.
	- 쉽게 생각하면, 봉우리($z$)를 정하고 남은 parameter들을 optimize 해야 할 것이다.
- 대수적으로 likelihood는 다중 봉우리(multiple model)를 가질 수 있음을


