# Set up Python 3 build system with Sublime Text 3
- 참고: https://stackoverflow.com/questions/23730866/set-up-python-3-build-system-with-sublime-text-3
- 이때: 파이썬 경로만 올바르게 설정하면..

# 세팅
**패키지 컨트롤 설치**
- http://webdir.tistory.com/396

**패키지 설치**
- `ctrl+shift+p` 키 입력후 Package Controll: install package 입력하여 패키지 설치창 띄우기.
  - BracketHighlighter 입력후 설치 (괄호 시작끝 확인에 유용)
  - 테마 원하는것 설치
    - spacegray 추천: https://github.com/kkga/spacegray
  - SPTP: 서버 파일들 작업할때 유용
    - 참고: https://opentutorials.org/course/671/3794
  - cython syntax 패키지
  - jedi 패키지 (python autocompletion)
  - Markdown Preview 설치 
    - 참고: https://github.com/revolunet/sublimetext-markdown-preview
    - live reload 는 위의 링크 참조.
    - 수식의 경우 : enable the `enable_mathjax` setting 

**기본 세팅**
- `Preferences->Settings` 에서 원하는 설정 추가.
  - 왼쪽창은 default 로 설정되어있는 것들.
  - `"auto_complete": true`
  - `"translate_tabs_to_spaces": true` 을 우측창에 추가해주자.

# 단축키
- 해당 line 으로 이동: crtl + g
- 빌드 및 실행: crtl + b
- 단어 선택: crtl + d
  - 여러번 반복할경우 같은단어 같이 선택
- 탭 변경: alt + 숫자s
- 창 분할: alt + shift + 숫자
- 새탭 생성: crtl + n
- 탭 지우기: crtl + w
- 패키지 설치 및 관리: ctrl + - 줄 위치 조정: ctrl + shift + 방향키 (위/아래)
- 한줄 선택: ctrl + l
- 마우스 휠 누른상태로 드래그: 여러줄 선택
- 프로젝트 내에서 파일명 검색: ctrl + p
- 해당 프로젝트내의 모든 파일에서 해당 단어 검색: ctrl + shift + f
