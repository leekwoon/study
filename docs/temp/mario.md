---
layout: page-sidenav
group: "이것저것"
title: "OpenAI gym 슈퍼마리오 (2017-12-04)"
---

# 윈도우 환경 실행환경 설정
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

# 슈퍼마리오 in OpenAI gym
- 먼저 게임 에뮬레이터 fceux를 설치한다.
```shell
$ sudo apt-get install fceux
```

- 슈퍼마리오의 다양한 난이도가 들어있는 gym-super-mario 패키지 설치
```shell
(tensorflow)$ pip install git+https://github.com/chris-chris/gym-super-mario
```

# 슈퍼마리오 강화학습
- (참고자료)
  - https://brunch.co.kr/@kakao-it/144
  - https://brunch.co.kr/@kakao-it/161
- 이미지 처리를 위한 opencv-python 패키지 설치 필요  
- 우선 다음과 같은 명령어로 설치해보자.
```shell
(tensorflow)$ pip install opencv-python
```
- 만약 python 3.n 버전에서 위와같은 방법으로 설치가 되지 않으면 아래방법으로 설치.
- 주의할 점은 opencv와 contrib 버전이 동일해야 한다.
```shell
# Build and install OpenCV 3.0 with Python 3.4+ bindings
(tensorflow)$ cd
(tensorflow)$ git clone https://github.com/Itseez/opencv.git
(tensorflow)$ cd opencv
(tensorflow)$ git checkout 3.0.0 # 버전 맞추기
(tensorflow)$ cd ~
(tensorflow)$ git clone https://github.com/Itseez/opencv_contrib.git
(tensorflow)$ cd opencv_contrib
(tensorflow)$ git checkout 3.0.0 # 버전 맞추기
(tensorflow)$ cd ~/opencv
(tensorflow)$ mkdir build
(tensorflow)$ cd build
(tensorflow)$ cmake -D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D INSTALL_C_EXAMPLES=ON \
    -D INSTALL_PYTHON_EXAMPLES=ON \
    -D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib/modules \
    -D BUILD_EXAMPLES=ON ..   # Time to setup the build
(tensorflow)$ make -j4 # start OpenCV compile process
(tensorflow)$ sudo make install # 설치 마무리
(tensorflow)$ sudo ldconfig # 설치 마무리
```
- virtual environment 에서 opencv 3.0 을 사용하기 위하여 sym-link 필요
```shell
# Sym-link OpenCV 3.0
(tensorflow)$ cd ~/tensorflow/lib/python3.5/site-packages/
(tensorflow)$ ln -s /usr/local/lib/python3.5/site-packages/cv2.cpython-35m-x86_64-linux-gnu.so cv2.so
```
- OpenAI의 baseline 설치 (최신 빌드를 설치하기 위해 아래 명령어 이용)
```shell
(tensorflow)~$ pip install git+https://github.com/openai/baselines
```
- 슈퍼마리오 강화학습 예제 프로젝트를 클론(clone)하자.
```shell
(tensorflow)~$ git clone https://github.com/chris-chris/mario-rl-tutorial
```
