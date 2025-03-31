---
title : "VSCode로 C++ 개발 환경 구축하기 (알고리즘)"
categories :
 - Algorithm
tags :
 - Guide
 - VSCode
 - Algorithm
 - C++
 - Tutorials
toc : true
toc_label : "목차"
toc_sticky : true
last_modified_at : 2025-03-31T02:30
---
Windows + VSCode + C++ 개발 환경 구축 Guide (2025년 3월 Updated)

안녕하세요.
오늘은 윈도우에서 VSCode로 C++ 개발 환경을 구축하는 방법에 대해 포스팅하려고 해요.
저는 알고리즘 문제를 풀때 주력 언어로 C++을 사용하고 있어요.
환경 설정하는데 시간은 넉넉하게 1시간 정도 걸려요.

사용할 환경입니다.
- Windows
- VSCode
- C++ (MinGW)

설치 순서
1. MinGW 설치
2. MinGW Path 설정
3. VSCode 설치
4. VSCode Extension 설치
5. VSCode C++ 빌드 및 디버깅 환경 설정
6. 이외 알고리즘 연습에 도움되는 vscode extension 설치
7. Baekjoon Online Judge 및 Solved.ac 둘러보기 (TBD)
8. GitHub 연동 (TBD)


# 0. 주의사항
<mark>경로에 한글이 포함되어 있으면 빌드/디버깅 시 오류가 발생할 수 있습니다!!</mark>
특히 바탕화면에 폴더를 만들어 사용하게 되면, 사용자명이 한글일 경우 경로에 한글이 포함되어 IDE가 해당 경로를 제대로 인식하지 못할 수 있습니다.
윈도우 사용자 폴더 이름 변경 방법 등을 검색하여 경로상에서 한글을 제거하거나,
`(ex: C:/Users/Amylo/Desktop/Dev)`
혹은 드라이브 최상위 영역에 새로 폴더를 만들어 그 곳에서 개발을 시작하셔도 좋습니다.
`(ex: C:/Dev)`

# 1. MinGW 설치
윈도우용 GCC, MinGW를 설치하기 위해 구글에 MinGW를 검색하세요.
요즈음 대부분의 PC가 64비트 운영체제이기 때문에 64비트 기준으로 설명하겠습니다.
(본인의 컴퓨터 운영체제가 64비트인지 확인하려면, 설정 - 정보에서 시스템 종류 정보를 확인하세요.)
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Starting_Algorithm/Untitled.png)

[MinGW-w64를 검색하시면 나오는 웹사이트](https://www.mingw-w64.org/)에 접속해 주시고, 옆의 메뉴 또는 사이드바에서 `Downloads - Pre-built Toolchains`로 이동해 줍니다.
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Starting_Algorithm/스크린샷 2025-03-31 오후 4.06.51.png)

밑으로 스크롤을 내리다 보면 `MinGW-W64-builds` 항목이 있는데, 여기에 있는 Installation Github 링크로 이동해 줍니다.
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Starting_Algorithm/스크린샷 2025-03-31 오후 4.29.01.png)

그러면 깃헙에서 최신 버전의 mingw가 빌드된 바이너리 파일을 받을 수 있는 웹사이트가 나옵니다.
여기에서 받아야 하는 파일은 다음과 같습니다.
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Starting_Algorithm/스크린샷 2025-03-31 오후 4.31.23.png)
x86-64는 빌드 환경이 64비트 컴퓨터이기 때문이고,
win32는 빌드한 프로그램을 윈도우에서만(POSIX 호환을 고려하지 않는) 실행할 예정이기 때문이고,
seh는 C++의 예외 처리 방식을 의미하는데, 윈도우에서 가장 성능이 좋은 방식이기 때문입니다.
ucrt를 고르는 이유는 msvcrt는 Win7 이하의 구버전 호환성을 고려할 필요가 없다면, 최신 윈도우 환경에서 더 나은 성능과 안정성을 제공하기 때문입니다.

파일 다운로드가 완료되고 나면 압축 프로그램을 이용해 해당 압축파일을 풀어 줍니다.
압축 해제 후 나온 mingw64 폴더를 폴더째로 C드라이브 밑에 옮겨 주세요.
`ex) C:\mingw64`
mingw64 폴더 밑에는 bin 폴더가 있어야 하고, bin 폴더 밑에는 g++.exe 파일이 있어야 합니다.
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Starting_Algorithm/스크린샷 2025-03-31 오후 4.45.53.png)

여기까지 완료했다면 MinGW 설치는 끝났습니다.

# 2. MinGW Path 설정
이제 설치된 MinGW를 환경변수, Path에 등록해 줄 건데요, 이 작업은 VSCode에서 컴파일 할 때 사용할 g++ 파일을 VSCode가 찾아서 쓸 수 있게 해 주는 작업이에요.
그러면 Win+R을 눌러 실행창을 띄우고, `sysdm.cpl` 을 친 다음 엔터를 하시면 시스템 속성 창이 뜰 텐데,
`고급 - 환경 변수` 로 이동하고,
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Starting_Algorithm/Untitled%209.png)

시스템 변수에서 Path를 찾아 더블 클릭 하시고,
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Starting_Algorithm/Untitled%2010.png)

환경 변수 편집 창에서 1. 새로 만들기, 2. 경로 입력, 3. 확인을 해 주시면 됩니다.
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Starting_Algorithm/Untitled%2011.png)

잘 되었는지 확인하려면, Win+R, cmd를 여신 다음에,
`gcc --version`
`g++ --version`
`gdb --version`
을 입력하고, 엔터 치셨을 때 하단 사진과 같이 관련 정보가 떠야 정상입니다.
(향후 업데이트로 인해 표기되는 정보는 달라질 수 있으나, 25년 3월 기준 gcc/g++은 14.2.0, gdb는 16.2 버전임을 확인할 수 있습니다.)
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Starting_Algorithm/스크린샷 2025-03-31 오후 5.51.02.png)

만약 설치나 설정이 잘못 되었다면, `‘gcc’ 은(는) 내부 또는 외부 명령, 실행할 수 있는 프로그램, 또는 배치 파일이 아닙니다.` 라는 메시지가 표시되면서 프로그램이 동작하지 않습니다.
환경 변수 설정이나 폴더명에서 오타가 난 것은 없는지 다시 확인해 주세요.
또는 `C:/mingw64/bin/g++.exe` 파일이 경로에 있는지 확인해 주세요.

# 3. VSCode 설치
에디터로 사용할 VSCode를 설치해 봅시다.
vscode : [https://code.visualstudio.com/](https://code.visualstudio.com/) 에서 설치 파일을 받아 주세요.
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Starting_Algorithm/Untitled%2013.png)

설치 파일을 실행해서 VSCode 설치를 진행해 줍니다.
기본적으로는 아무것도 변경하지 않으셔도 됩니다.
필요에 따라 “code로 열기” 관련 2개는 체크해 주셔도 됩니다
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Starting_Algorithm/Untitled%2014.png)

VSCode 설치가 완료되었습니다.
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Starting_Algorithm/Untitled%2015.png)

# 4. VSCode Extension 설치
설치 완료된 VSCode를 열어 주세요.
사이드바에 있는 Extension을 누르고, (Ctrl+Shift+X 단축키도 있습니다.)
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Starting_Algorithm/Untitled%2016.png)

C++을 검색하신후 나오는 두 가지를 설치해 줍니다.
(C/C++, C/C++ Extension Pack)
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Starting_Algorithm/Untitled%2017.png)

그리고 한국어 언어 확장도 설치해 줍니다.
Extension에서 Korean이라고 검색하면 나오는 언어 팩을 설치해 주세요.
(Korean Language Pack for Visual Studio Code)
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Starting_Algorithm/Untitled%2018.png)

그러면 오른쪽 하단에 한국어 적용을 위해 VSCode를 재시작해야 한다는 메시지가 뜨는데, Restart 버튼을 눌러 VSCode를 재시작해 주세요.
VSCode가 껐다 켜질 거에요.
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Starting_Algorithm/Untitled%2019.png)

이제 VSCode Extension 설치가 끝났습니다.

# 5. VSCode C++ 빌드 및 디버깅 환경 설정
바탕화면이나 적당한 곳에 baekjoon 폴더를 하나 만드시는걸 추천드려요.
폴더 이름이나 위치는 크게 상관 없습니다. (단, 경로명에 한글이 포함되지 않도록 유의하여 주세요.)
그리고 방금 만든 폴더를 VSCode에서 열어 주세요.
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Starting_Algorithm/Untitled%2020.png)

그러면 텅 빈 폴더가 열리는데요, 이제 파일을 하나 만들어 볼거에요.
(혹시 폴더의 내용을 신뢰하시겠냐고 물으면, 신뢰한다고 해 주시면 됩니다.)
탐색기의  위쪽에 있는 새 파일 버튼을 누르면 파일 제목을 입력해 달라고 하는데요, 
예시로 백준에서 1000번 문제를 풀어 보기 위해 제목을 `1000.cpp` 으로 지어 줍시다.
[https://www.acmicpc.net/problem/1000](https://www.acmicpc.net/problem/1000)
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Starting_Algorithm/Untitled%2021.png)

1000.cpp 로 파일 이름을 지어 주시고, 엔터를 누르면, 이제 코드를 입력할 빈 창이 떠요.
그런데 아직 코드를 작성만 하고, 빌드, 디버깅을 할 환경은 갖춰지지 않았어요.
지금부터는 디버깅을 위한 환경을 구축해 볼 거에요.
VSCode에서 F1 키를 누르면 나오는 곳에, C/C++ 을 입력하면 이렇게 나오는데, 
여기서 `C/C++ 구성 편집 (UI)` 를 눌러 줍니다.
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Starting_Algorithm/Untitled%2022.png)

다음과 같이 설정해 줍니다.
- 컴파일러 경로 : `C:/mingw64/bin/g++.exe `
  - (아까 mingw64를 넣었던 위치대로 입력해 주시면 됩니다.)
- 컴파일러 인수 : 건드리지 않습니다.
- InteliSense 모드 : `windows-gcc-x64`
- 경로 포함 : 건드리지 않습니다.
- 정의 : 건드리지 않습니다.
- C 표준 : 건드리지 않습니다.
- C++ 표준 : 건드리지 않습니다.

![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Starting_Algorithm/Untitled%2023.png)
만약 C 또는 C++ 언어 버전을 C17 또는 C++17에서 변경하고 싶으신 분은 원하는 값으로 변경해 주시면 됩니다.
설정 저장 버튼은 따로 없고, 변경하는 즉시 저장됩니다.
이렇게 설정이 끝나면. 왼쪽에 탐색기에는 `.vscode`라는 폴더가 자동으로 생겼을 겁니다.
다음으로는, 아까 만들어 둔 1000.cpp 파일을 편집하는 창을 열어둔 상태에서
조그만 톱니바퀴 모양을 누르면 디버그 구성 설정이 나오는데, 
`C/C++: g++.exe 활성 파일 빌드 및 디버그` 를 눌러 줍니다.

![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Starting_Algorithm/Untitled%2024.png)
그러면 `launch.json` 파일이 열리고, 설정을 바꿀 수 있게 됩니다.
앞으로 수많은 문제들을 풀어나가는데 각 문제마다 작성했던 코드의 exe 파일이 보이면 보기 복잡하니까, 우리는 `baekjoon` 폴더 밑에 `build` 폴더를 만들어서 그곳에 모든 exe를 넣어 둘 거에요.
(개인 취향에 따라 안 하셔도 됩니다.)
새 폴더 버튼을 눌러서 `build` 폴더를 만들어 줍니다.
.vscode 밑이 아니라 baekjoon 폴더 바로 밑에 만들어 지는지 잘 확인해 줍니다.

![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Starting_Algorithm/Untitled%2025.png)
`launch.json` 파일로 돌아가서, 내용을 잘 보다보면 `program` 으로 시작하는 것이 있는데,

이렇게 있던 것을
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Starting_Algorithm/Untitled%2026.png)

이렇게 바꿔주세요. (사이에 `build\\` 추가)
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Starting_Algorithm/Untitled%2027.png)

그리고 .vscode 밑에 있는 tasks.json에서도

이렇게 있던 것을
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Starting_Algorithm/Untitled%2028.png)

이렇게 바꿔 주세요. args 맨 밑에 있는 것을 바꿔 주시면 됩니다.
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Starting_Algorithm/Untitled%2029.png)

만약 설정하다가 뭔가 잘못 된 것 같으면 .vscode 밑에 있는걸 모두 지우고, 6번 과정을 다시 해 주시면 됩니다.

그리고 이제 1000.cpp 파일로 돌아와서, 다음을 입력해 봅시다.

```cpp
#include <iostream>

using namespace std;

int main()
{
    cout << "hello world!";
}
```

그런 다음, F5 키를 눌렀을 때 코드에 문제가 없다면,
하단 터미널 탭에서 hello world! 출력이 잘 나오는 것을 확인할 수 있습니다.
빌드되어 생성된 exe 파일은 build 폴더 밑에 cpp 파일명으로 생성된 것을 확인할 수 있습니다.
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Starting_Algorithm/Untitled%2030.png)

이제 개발 환경 설정은 끝났습니다!

# 6. 이외 알고리즘 연습에 도움되는 VSCode Extension 설치
알고리즘 문제를 풀다 보면, 다양한 입력 예시를 넣어서 결과가 의도한 대로 테스트를 여러 번 해 봐야 하는 상황이 생깁니다.
이럴 때, 여러 개의 입력과, 그에 맞는 출력을 입력해 두고, 등록해 둔 테스트 케이스들을 실행시켜 주는 VSCode 확장이 있습니다.
`Competitive Programing Helper`라는 Extension인데, 한번 설치해 보도록 합시다.
사이드바에서 Extension을 열고, 검색창에 `cph judge`로 검색하면 나오는 Extension을 설치해 줍니다.
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Starting_Algorithm/Untitled%2031.png)

혹시 설치 후 방화벽에서 액세스 허용 창이 뜨면 허용을 눌러 줍니다.
확장을 설치하고 나면, 사이드바에 못 보던 아이콘이 하나 생깁니다.
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Starting_Algorithm/Untitled%2032.png)

이것을 눌러 보면, 아래와 같은 창이 뜨는데, 테스트 케이스들을 확인해 볼 cpp 파일을 열고, Create Problem 버튼을 눌러 줍니다.
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Starting_Algorithm/Untitled%2033.png)

만약 사이드바가 너무 작다고 문구가 뜨면 ignore를 눌러 무시하거나, 사이드바를 옆으로 드래그해서 크기를 키워 줍니다.
`Create Problem`을 누른 이후에는, 입력과 그 입력에 따른 결과값을 입력할 수 있습니다.
예시로 1000번 문제에 있는 예제 입력을 복사하여 Input에, Expected Output에 예제 출력을 복사하여 붙여넣어 주세요.

![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Starting_Algorithm/Untitled%2034.png)

더 많은 테스트 케이스를 원하시면, New Testcase를 눌러 또 다른 경우를 테스트해 볼 수 있습니다.
만약 입력으로 5와 8이 들어왔다면, 출력은 13이 나와야 문제에서 요구하는 것을 풀 수 있겠죠?
이렇게 테스트 케이스를 입력해 주었다면, 작성한 코드가 올바르게 동작하는지 테스트를 해 줍니다.
CPH Judge 확장 사이드바 하단에 Run All 버튼을 누르면, 이 코드 파일에 등록된 테스트 케이스들을 모두 테스트 해 줍니다.
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Starting_Algorithm/Untitled%2035.png)

테스트 결과 값이 예상한 대로 나왔다면 Passed가 뜨고, 예상한 답을 출력하지 못했다면 Failed와 함께 프로그램이 출력했던 값을 같이 표시해 줍니다.
실행 시간도 같이 표시해 줘서, 어느 정도는 시간 초과가 나올지 예상할 수도 있습니다.
이제 CPH Judge 확장 설치가 끝났습니다.

---
<mark style='background-color: #dcffe4'>
<b>글에 오류가 있거나 궁금한 것, 의견 등이 있으면 자유롭게 댓글로 달아주시면 감사하겠습니다!</b>
</mark>