---
layout: page-sidenav
group: "github"
title: "Learning convolutional neural networks for graphs"
---
Ref)
<http://tech.whatap.io/2015/09/11/install-jekyll-on-windows/>
# 1. 배경설명
## 1.1 문제 정의 
<!-- 사진 -->
주어진 그래프들로 Classify를 하기위한 Representation(표현방식)을 어떻게 학습할 수 있을까. E.g. vector representation
## 1.2 어려운점
<!-- 사진 -->
Graph theory에서 두 그래프 G, H 의 isomorphism 은 전단사 함수 f:V(G) -> V(H)로 정의된다. 이때 f는 그래프 G의 두점 $$u$$,v가 인접할때 f(u),f(v) 도 인접해야 하는 조건을 만족한다. 이때 f가 존재하면 우리는 그래프 G와 H가 isomorphic(동형)이라한다. 두 그래프가 isomorphic한지 아닌지 판별하는 문제를 isomorphic problem이라 하며 이문제는 P에 속하는지 NP에 속하는지 아직 풀리지 않고있다. 하지만 이를 genalize한 subgraph isomorphic problem의 경우 NP-complete한 문제로 알려져있다. 그래프가 동형인지 아닌지 판별하는 문제도 이렇게 어려운데 이러한 Graph Representation을 어떻게 Vector representation으로 나타낼 수 있을까?
## 1.3 Graph Kernel
Graph Kernel이란 (graph substructure에 기초하여)그래프간의 Similarity를 구하는 함수이다. 이전 연구들은 Graph Kernel을 이용하여 Vector Representation으로 바꾸고 SVM같은 모델을 사용하여 Classify를 하였다. 중요한 점은 이전 연구들의 경우에 Graph를 하나의 global graph structure로 가정했다는 점이다.
- Jekyll은 여러 형태의 텍스트와 소스들로 구성 정적 파일들을 웹사이트로 생성시켜주는 툴이다.
- GitHub Pages 에서도 이용.

## 1.1 필요한 것
- Ruby(ruby, DevKit)
- Jekyll
- Python(Setuptool,pip,Pygments)
- rouge

Ruby for Jekyll. Pygments for syntax highlighting. python for Pygments.

## 1.2 Ruby 설치하기

<http://rubyinstaller.org/downloads/>
위 주소에서 Ruby와 Development Kit 설치

설치할때 아래 옵션을 체크하면 어느 경로에서든 ruby 실행 가능!

![Install Ruby]({{ site.baseurl }}/images/web_study/github/1.PNG){:class="center-block" height="500px"}

C:\RubyDevkit 에 Development Kit을 설치했다 가정하고
윈도우 CMD 창에서 아래 명령으로 초기화와 Ruby 와 Binding 을 해줍니다.

`cd C:\RubyDevkit`

`ruby dk.rb init`

`ruby dk.rb install`

## 1.3 Jekyll 설치하기
Ruby의 gem 패키지 인스톨러를 이용하여 설치

`gem install jekyll`

`gem install rouge`

`gem install bundle`

`gem install wdm`

`bundle install`

## 1.4 Python 설치하기
설치는 아래 링크에서
<https://www.python.org/downloads/>

Pygments 설치
`pip install Pygments`

#나머지는 나중에..