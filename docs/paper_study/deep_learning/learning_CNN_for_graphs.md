---
layout: page-sidenav
group: "deep_learning"
title: "Learning convolutional neural networks for graphs"
---
# 1. 배경설명
## 1.1 문제 정의 
![]({{ site.baseurl }}/images/paper_study/deep_learning/learning_CNN_for_graphs/2.PNG){:class="center-block" height="300px"}
주어진 그래프들로 Classify를 하기위한 Representation(표현방식)을 어떻게 학습할 수 있을까. E.g. vector representation
## 1.2 어려운점
![]({{ site.baseurl }}/images/paper_study/deep_learning/learning_CNN_for_graphs/2.PNG){:class="center-block" height="400px"}
Graph theory에서 두 그래프 $$G, H$$ 의 *isomorphism* 은 전단사 함수 $$f:V(G) -> V(H)$$로 정의된다. 이때 $$f$$는 그래프 G의 두점 $$u,v$$가 인접할때 $$f(u),f(v)$$ 도 인접해야 하는 조건을 만족한다. 이때 f가 존재하면 우리는 그래프 $$G와 H$$가 *isomorphic*(동형)이라한다. 두 그래프가 *isomorphic*한지 아닌지 판별하는 문제를 *isomorphic problem*이라 하며 이문제는 *P*에 속하는지 *NP*에 속하는지 아직 풀리지 않고있다. 하지만 이를 genalize한 *subgraph isomorphic problem*의 경우 *NP-complete*한 문제로 알려져있다. 그래프가 동형인지 아닌지 판별하는 문제도 이렇게 어려운데 이러한 Graph Representation을 어떻게 Vector representation으로 나타낼 수 있을까?
## 1.3 Graph Kernel
*Graph Kernel*이란 (graph substructure에 기초하여)그래프간의 Similarity를 구하는 함수이다. 이전 연구들은 *Graph Kernel*을 이용하여 Vector Representation으로 바꾸고 SVM같은 모델을 사용하여 Classify를 하였다. 중요한 점은 이전 연구들의 경우에 Graph를 하나의 global graph structure로 가정했다는 점이다.

# 2. 새로운 접근법 (PATCHY-SAN)
- G
