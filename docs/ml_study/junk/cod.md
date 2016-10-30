---
layout: page-sidenav
group: "이것 저것"
title: "Curse of Dimensionality"
---

![]({{site.baseurl}}/images/ml_study/junk/cod1.png){:class="center-block"}

- 데이터의 차원(dimensionality)이 증가할수록 해당 공간의 크기(부피)가 기하급수적으로 증가한다.
- 차원이 증가할수록 데이터의 분포 분석 또는 모델추정에 필요한 샘플 데이터 개수가 기하급수적으로 증가한다.
- 고차원의 대부분 데이터들은 데이터 공간의 표면부근에 위치한다.
- 어떤 한 점으로 부터 최근접 거리와 최원접 거리의 차이가 거의 없어진다.
- $$D$$ 차원에서 반지름의 길이가 $$r=1$$ 인 구를 생각해보자.
- 부피는

$$V_D(r)=K_Dr^D$$

- 예를들어, 3차원에서 $$K_D$$는 $$(4/3)\pi$$
- 거리가 $$r = 1 $$인 경우의 구의 부피에서 $$r = 1-e$$인 구의 부피를 빼는 것을 상상해보자.

$$\frac{V_D(1)-V_D(1-e)}{V_D(1)} = 1-(1-e)^D$$

- $$e$$ 값이 변화할 때 원래 부피와의 비율을 살펴보자.

![]({{site.baseurl}}/images/ml_study/junk/cod2.png){:class="center-block"}

- 위 그래프를 해석하자면, 차원이 증가할수록 전체 볼륨 크기의 대부분은 표면에 위치한다.
- 또한 kmeans와 같이 distance를 바탕으로 하는 alogrithm은 차원이 커지면 무의미 해진다.
- 예를들어, $$D$$ 차원에서 $$r=1$$인 구에서 feature point들이 uniformly distributed 되어있다고 가정해보자. 차원 $$D$$가 $$\infty$$에 가까워질 떄 원점을 classify 하는 문제를 생각해보자. 대부분의 데이터가 구의 표면에 위치고 원점으로 부터 거리가 동일하기 때문에 거리정보는 더이상 유의미 하지 않을 것이다.


