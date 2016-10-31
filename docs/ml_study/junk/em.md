---
layout: page-sidenav
group: "이것 저것"
title: "EM algorithm"
---

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

$$KL(p\vert\vert q) = -\int p(X)\ln q(x)dx - (-\int p(X)\ln q(x)dx )$$