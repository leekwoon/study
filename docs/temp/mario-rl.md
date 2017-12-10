---
layout: page-sidenav
group: "이것저것"
title: "OpenAI gym 슈퍼마리오 강화학습 (2017-12-06)"
---

# OpenAI gym 슈퍼마리오 강화학습

설치
----
- (참고자료)
  - https://brunch.co.kr/@kakao-it/144
  - https://brunch.co.kr/@kakao-it/161
- 이미지 처리를 위한 opencv-python 패키지 설치 필요
- 우선 다음과 같은 명령어로 설치해보자.
```shell
(tensorflow)$ pip install opencv-python
```
- 만약 python 3.n 버전에서 위와같은 방법으로 설치가 되지 않으면 [아래방법](https://stackoverflow.com/questions/37188623/ubuntu-how-to-install-opencv-for-python3)으로 설치.
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
