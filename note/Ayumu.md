
# 설치 (windows:mingw64)
1. git clone --recursive https://github.com/YuriCat/AyumuCurling.git
2. box 2d 설치
- pacman -S mingw64/mingw-w64-x86_64-box2d
  - /mingw64/lib
  - /mingw64/include
  - 에 가각 설치됨
3. makefile 경로 수정.
4. make 할때 에러들..
  - min (undefinred...)
    - 이유: 안의 두 인자의 type 이 달라서..
    - 해결책: min 을 min<int> 로 바꿔주어 type 맞춰주기
  - ASSERT 어쩌고 에러
    - 걍 주석처리
  - __imp_send undefined ...
    - 컴파일 할때 -lws2_32 링크해주어야.
