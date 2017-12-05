---
layout: page-sidenav
group: "이것저것"
title: "OpenAI gym 슈퍼마리오 (2017-12-04)"
---

# 윈도우 환경 실행환경 설정
- (참고자료) 윈도우 서브시스템 리눅스에 openAI Gym 설치하기
  > http://jinman190.blogspot.ca/2017/10/openai-gym.html
  > http://prolite.tistory.com/830
- 서브시스템 리눅스에 가상환경, tensorflow 설치 (for python 3.n)
```shell
$ sudo apt-get install python3-pip python3-dev python3-tk python-virtualenv
$ virtualenv --system-site-packages -p python3 ~/tensorflow
$ source ~/tensorflow/bin/activate
(tensorflow)~$ pip3 install --upgrade tensorflow
```
- openAI gym 설치 (for python 3.n)
```shell
(tensorflow)~$ sudo apt-get install -y cmake zlib1g-dev libjpeg-dev xvfb libav-tools xorg-dev python-opengl libboost-all-dev libsdl2-dev swig
$ git clone https://github.com/openai/gym.git
$ cd gym
$ pip install -e '.[all]' # 파이썬 3.5 설치
```

- 서브시스템 리눅스 환경에서 실행한 코드를 윈도우 상에서 visualize 하기 위하여 윈도우10에서 xming을 설치하고 실행한다.
- 리눅스 환경에서 다음명령어 실행.
```shell
$ export DISPLAY=:0
```
- 이때 굳이 visualize 할 필요가 없다면
```shell
$ export DISPLAY=:99
```
- 성공적으로 맞췄다면 아래 python 코드를 리눅스 환경에서 실행했을때, 그래프가 성공적으로 visualize 될 것이다.
```python
from matplotlib import pyplot as plt
plt.figure()
plt.plot([1,2,3])
plt.show()
```


based
=====
based
------
