https://www.reddit.com/r/MachineLearning/comments/7ezd1q/research_project_leela_zero_a_community_open/

https://www.reddit.com/r/cbaduk/


# leela zero
- who?
  - Gian-Carlo Pascutto

# 설치 (windows:mingw64)
- 세팅
pacman -S mingw64/mingw-w64-x86_64-gcc
 pacman -S mingw64/mingw-w64-x86_64-boost
 pacman -S mingw64/mingw-w64-x86_64-openblas
 pacman -S mingw64/mingw-w64-x86_64-opencl-headers
 pacman -S mingw64/mingw-w64-x86_64-cmake
 pacman -S git
  pacman -S make
  pacman -S vim
git clone https://github.com/KhronosGroup/OpenCL-ICD-Loader.git
install Windows SDK version 8.1 from https://developer.microsoft.com/en-us/windows/downloads/windows-8-1-sdk
cd OpenCL-ICD-Loader
 make

윈도우 설치 (mingw64 를 이용한..)
- **0xc000007b 에러**
  - https://wiki.mcneel.com/ko/rhino/rhino5/error0xc000007b
  - Dependancy walker x64 프로그램을 이용하여, leeaz.exe를 실행해보니 ... `Windows/system32` 폴더 안에 msvpr120d.dll 이 혼자 x86 이여서 에러... (64bit 프로그램인데..)
  - system 폴더안의 msvpr120d.dll 을 64bit 용 msvpr120d.dll 으로 바꾸어주어서 해결.

- 기본 패키지 설치
  - pacman -S mingw64/mingw-w64-x86_64-openblas
    - 예로, /mingw64/include/OpenBlas 에 헤더파일들
    - /mingw64/lib 에 libopenblas 파일들
- __int64 관련 에러
  - __int64 는 msvc에서 정의된...
    - config.h 에서 _WIN32 일때 조건문을 주석처리해서 .. 해결
- cannot find -lboost_program_options
  - pacman -S mingw64/mingw-w64-x86_64-boost 로 설치해보니...  `mingw64/lib` 폴더에 libboost_program_options-mt.a 있음을 확인.
    - 이미 -L/mingw/lib 경로 추가해 주었으니...
    - -lboost_program_options-mt 로 바꾸어주자.
- -lOpenCL
  - OpenCL library를 mingw64상에서 깔고 싶었으나.. (opencl icd loader github을 통해..) 쉽지 않아...
    - leela-zero 에서 제공된 visual studio 솔루션 안의 패키지 이용...opencl-nug.0.777.12
- 최종적으로, mingw64 상에서 실행할때는 에러가 나지만, 윈도우 console 상에서는 오류없이 실행 가능!

ubuntu 설치
- 이슈1
  - gcc 버전이 -std=c++14 지원 안하는 경우
    -https://gist.github.com/application2000/73fd6f4bf1be6600a2cf9f56315a2d91
    - 위 링크를 통해 gcc-7 버전을 받은후, makefile 에 gcc를 gcc-7로 바꾸고, 또한 g++ 을 g++-7로 바꾸자.
    - 예로 `CC=gcc-7 CXX=g++-7`
  - openblas를 설치했을때 설치가 안됬을경우.. 직접설치
    - https://github.com/xianyi/OpenBLAS
  - 이때 makefile을 올바르게 수정해주어야..
  - `        CXXFLAGS += -I/opt/OpenBLAS/include
        LDFLAGS  += -L/opt/OpenBLAS/lib`
  - opt폴더는 default로 설치가 이루어지는 폴더이고, 만약 다른곳에 설치했다면 그에 맞게 경로 수정 요함.


- undefined reference to 'openblas_get_corenae'

# issue
  - 우분투 Opencl
    - https://github.com/gcp/leela-zero/issues/164
    - https://askubuntu.com/questions/850281/opencl-on-ubuntu-16-04-intel-sandy-bridge-cpu
  - msvcr120d.dll 에러
