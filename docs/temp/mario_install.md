---
layout: page-sidenav
group: "이것저것"
title: "OpenAI gym 슈퍼마리오 (2017-12-04)"
---

윈도우 환경 실행환경 설정
-----------------------
- (리눅스 환경에선 쉽지만...) openAI gym을 윈도우에서 이용하려면 우선, 윈도우용 우분투 앱 설치. (windows 10 기준)
- (참고자료)
  1. http://jinman190.blogspot.ca/2017/10/openai-gym.html
  2. http://prolite.tistory.com/830
- 참고로, 우분투 앱 상에서는 C 드라이브 위치는
```shell
$ cd /mnt/c
```
- 서브시스템 리눅스에 가상환경, tensorflow 설치 (for python 3.n)
```shell
$ sudo apt-get install python3-pip python3-dev python3-tk python-virtualenv
$ virtualenv --system-site-packages -p python3 ~/tensorflow
$ source ~/tensorflow/bin/activate
(tensorflow)$ pip3 install --upgrade tensorflow
```
- openAI gym 설치 (for python 3.n)
```shell
(tensorflow)$ sudo apt-get install -y cmake zlib1g-dev libjpeg-dev xvfb libav-tools xorg-dev python3-opengl libboost-all-dev libsdl2-dev swig
(tensorflow)$ pip install gym
```

- 서브시스템 리눅스 환경에서 실행한 코드를 윈도우 상에서 visualize 하기 위하여 윈도우10에서 xming을 설치하고 실행한다.
- 리눅스 환경에서 다음명령어 실행.
```shell
(tensorflow)$ export DISPLAY=:0
```
- 이때 굳이 visualize 할 필요가 없다면
```shell
(tensorflow)$ export DISPLAY=:99
```
- 지금까지 과정을 성공적으로 맞췄다면 아래 python 코드를 리눅스 환경에서 실행했을때, 간단한 게임 실행창을 볼 수 있을 것이다..
```python
import gym
env = gym.make('CartPole-v0')
env.reset()
for _ in range(1000):
    env.render()
    env.step(env.action_space.sample()) # take a random action
```

슈퍼마리오 in OpenAI gym
--------------------------
- 먼저 게임 에뮬레이터 fceux를 설치한다.
```shell
$ sudo apt-get install fceux
```

- 슈퍼마리오의 다양한 난이도가 들어있는 gym-super-mario 패키지 설치
```shell
(tensorflow)$ pip install git+https://github.com/chris-chris/gym-super-mario
```
