---
layout: page-sidenav
group: "Deep learning"
title: "Learning convolutional neural networks for graphs"
---
# 1. 배경설명

## 1.1 문제 정의 
![]({{ site.baseurl }}/images/paper_study/deep_learning/learning_CNN_for_graphs/2.PNG){:class="center-block" height="250px"}
주어진 그래프들로 Classify를 하기위한 Representation(표현방식)을 어떻게 학습할 수 있을까. E.g. vector representation

## 1.2 어려운점
![]({{ site.baseurl }}/images/paper_study/deep_learning/learning_CNN_for_graphs/3.PNG){:class="center-block" height="400px"}
Graph theory에서 두 그래프 $$G, H$$ 의 *isomorphism* 은 전단사 함수 $$f:V(G) -> V(H)$$로 정의된다. 이때 $$f$$는 그래프 G의 두점 $$u,v$$가 인접할때 $$f(u),f(v)$$ 도 인접해야 하는 조건을 만족한다. 이때 f가 존재하면 우리는 그래프 $$G와 H$$가 *isomorphic*(동형)이라한다. 두 그래프가 *isomorphic*한지 아닌지 판별하는 문제를 *isomorphic problem*이라 하며 이문제는 *P*에 속하는지 *NP*에 속하는지 아직 풀리지 않고있다. 하지만 이를 genalize한 *subgraph isomorphic problem*의 경우 *NP-complete*한 문제로 알려져있다. 그래프가 동형인지 아닌지 판별하는 문제도 이렇게 어려운데 이러한 Graph Representation을 어떻게 Vector representation으로 나타낼 수 있을까?

## 1.3 Graph Kernel
*Graph Kernel*이란 (graph substructure에 기초하여)그래프간의 Similarity를 구하는 함수이다. 이전 연구들은 *Graph Kernel*을 이용하여 Vector Representation으로 바꾸고 SVM같은 모델을 사용하여 Classify를 하였다. 중요한 점은 이전 연구들의 경우에 Graph를 하나의 global graph structure로 가정했다는 점이다.

# 2. 새로운 접근법 (PATCHY-SAN)

## 2.1 Main Idea
- *CNN*을 통한 feature 자동 생성
- Global보다 **Local**에 집중
<div class="text-center">
  <img src="{{ site.baseurl }}/images/paper_study/deep_learning/learning_CNN_for_graphs/4.PNG" alt="" height="180px" />
  <img src="{{ site.baseurl }}/images/paper_study/deep_learning/learning_CNN_for_graphs/5.PNG" alt="" height="180px" />
</div>

이미지에선..
- 이미지는 그 안에 내재되어진 순서가 존재 (*spatial order*)
- 따라서 *CNN*에서 receptive field가 왼쪽에서 오른쪽, 위에서 아래로 적용 가능

그래프의 경우에는?
![]({{ site.baseurl }}/images/paper_study/deep_learning/learning_CNN_for_graphs/6.PNG){:class="center-block" height="200px"}

They suggest PATCHY-SAN approach
(**S**ELECT-**A**SSEMBLE-**N**ORMALIZE)

## 2.2 Node Sequence Selection
![]({{ site.baseurl }}/images/paper_study/deep_learning/learning_CNN_for_graphs/7.PNG){:class="center-block" height="400px"}

- Labeling method로 *Weisfeiler-Lehman* 알고리즘 이용

## 2.3 Neighborhood Assembly
![]({{ site.baseurl }}/images/paper_study/deep_learning/learning_CNN_for_graphs/12.PNG){:class="center-block" height="400px"}

- Simple breadth-rst expansion until at least k nodes added, or no additional nodes to add
 
## 2.4 Graph Normalization
![]({{ site.baseurl }}/images/paper_study/deep_learning/learning_CNN_for_graphs/15.PNG){:class="center-block" height="400px"}

### 2.4.1 *Optimal graph normalization* 문제정의
![]({{ site.baseurl }}/images/paper_study/deep_learning/learning_CNN_for_graphs/14.PNG){:class="center-block" height="200px"}

- 상대적으로 비슷한 위치에 있는 노드들이 *adjacency matrix*에서 비슷한 위치에 놓이게 하는 *labeling method* 을 찾기위해 위와같은 문제 정의
	- intuition: *adjacency matrix*간의 distance가 edit graph distance와 같은 그래프간의 유사도를 정의할 수 있는 *graph distance*와 유사하게 하는 *labeling method*찾기

![]({{ site.baseurl }}/images/paper_study/deep_learning/learning_CNN_for_graphs/Theorem1.PNG){:class="center-block" height="60px"}

- 하지만 위의 *Optimal graph normalization* 문제의 경우 *NP-hard*한 문제
	- 증명: By reduction from subgraph isomorphsim (which is *NP-complete* problem).

### 2.4.2 *Optimal graph normalization* 대안
![]({{ site.baseurl }}/images/paper_study/deep_learning/learning_CNN_for_graphs/Theorem2.PNG){:class="center-block" height="150px"}

- $$d_A가 d_G$$보다 같거나 크다는 가정 아래에서 (예를 들어,  $$d_A$$:Hamming distance, $$d_G$$:edit distance on graphs) $$d_G$$ 를 고려하지 않고 더 쉬운 문제를 풀면 된다.($$d_G$$경우 *NP-complete*한 문제로, $$d_G$$를 고려하지 않아도 되니 이득)
- 여러 *labeling method* 후보군 중에 위의 식을 가장 최소화 하는 $$l$$ 을 고르면 됨.

![]({{ site.baseurl }}/images/paper_study/deep_learning/learning_CNN_for_graphs/16.PNG){:class="center-block" height="400px"}

- 대부분의 *labeling method*는 단사함수가 아니라 같은 label 존재 가능.
- 이를 해결하기 위해 Nauty tool 이용 (논문에선 단순히 성능이 좋아서 이용했다고 언급.)

## 2.5 CNN Architecture
- 논문에서는 실험에서 node attributes만을 이용.
- $$a_n$$: node attributes 갯수
![]({{ site.baseurl }}/images/paper_study/deep_learning/learning_CNN_for_graphs/17.PNG){:class="center-block" height="400px"}

# 3. Result & Conclusion

## 3.1 Experiments - Graph Classification
![]({{ site.baseurl }}/images/paper_study/deep_learning/learning_CNN_for_graphs/18.PNG){:class="center-block" height="400px"}

## 3.2 Experiments - Visualization
![]({{ site.baseurl }}/images/paper_study/deep_learning/learning_CNN_for_graphs/19.PNG){:class="center-block" height="400px"}

- *RBM*같은 모델이 *unsupervised*방식으로 feature을 학습할 수 있는지 보기 위해

## 3.3 장단점
**장점**
- *Graph kernel*디자인 할 필요가 없다.
- Visualization 지원.
- 다른 *graph kernel*방식보다 성능이 좋음.

**단점**
- 적은 데이터에서 *overfitting*문제.
- 많은 *hyperparameters*.
- Graph normalization이 학습의 일부분이 아님.