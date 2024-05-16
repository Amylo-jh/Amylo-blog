---
title : "2024 Hacktheon 예선 Writeup / Review"
categories:
 - CTF
tags :
 - CTF
 - Writeup
toc : true
toc_lable : "목차"
toc_sticky : true
last_modified_at : 2024-04-29T00:00
---

2024 핵테온 국제 대학생 사이버보안 경진대회 예선 Writeup 및 후기.

# 대회 개요
- 일시 : 2024.04.27(토) 09:00 ~ 18:00
- 방법 : 온라인
- 방식 : Jeopardy (초급, 고급 문제 동일)

# 대회를 시작하기 전에
사실 핵테온 ctf가 있다는 것은 올해 처음 알게된 것이 아니다. 최소 작년쯤부터 알고 있었는데, 대학생만 참가 가능하다고 하고, 올해부터 방송대 대학생이 되어 참가 자격도 만족하여 팀을 꾸려 참가하게 되었다.
방송대 오픈채팅방애서 대회가 있다는 사실을 알게 되었다. 다음부터는 이런 일정들을 미리 알 수 있도록 레이더를 좀 켜놔야겠다 싶었다.
이전에도 ctf는 세 번 정도 참가하긴 했었다. 
2022년에 사이버전사 경연대회 예선, BingoCTF, 2023년에 CCE 예선. 올해 핵테온.
처음 사이버전사 경연대회때는 진짜 뭐가 뭔지 하나도 몰라서 당황했는데, 이제서야 겨우 윈도우 문제 하나를 풀어낼 수 있었다. 그동안 연습을 꾸준히 안 한 것도 있어서 오래 걸렸겠지만...
공부 안한게 무서워서 초급으로 신청해서 참가하였다.
우선 Writeup 먼저 작성하고, 못 풀었지만 건드려 봤던 문제들에 대해 할 얘기 하고, 대회 후기 순으로 작성해 보겠다.

# Writeup

## 00. Mic Check
처음엔 무슨 소리인가 했다.
Real Flag를 알려주는 사이트 사용법을 알려주려고 만든 문제 같다.
1을 넣으면 진짜 플래그를 알려준다.

## 01. MS
- 분류 : 포렌식, misc

파일을 받으면 웬 깨진 엑셀 파일이 나온다.
오피스 파일들은 기본적으로 zip 파일 형식으로 저장된 파일이라, zip으로 확장자를 바꾸고 열어 보면 그 내부가 어떻게 되어 있는지 알 수 있다.
내부 파일들을 뒤져보다가, (삽질했다) 최상위 경로에서 ppt라는 이름의 폴더를 보고, 뒤늦게 눈치챘다. 아, 엑셀이 아니라 ppt 파일이구나.
확장자를 xlsx에서 ppt로 바꿔 열면 플래그를 바로 볼 수 있다.
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Hacktheon-review/image.png)

## 02. Confidential
- 분류 : 포렌식, misc

pdf 파일을 010에디터로 열어 보면 파일의 초반 부분에 이렇게 시작하는 부분이 있다.
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Hacktheon-review/20240427093647.png)

ChatGPT에 돌려 보았다.
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Hacktheon-review/image-1.png)
꺾쇠 안에 Hex 코드는 자바스크립트 코드를 포함하고 있고, 이는 var encodedStr... 로 시작하는 코드를 담고 있다.

저 꺾쇠 괄호부터 시작해서 끝까지 복사한 다음, CyberChef에서 From Hex 필터를 거치면 디코딩된 자바스크립트 코드가 나온다.

나온 코드를 크롬 F12로 콘솔에서 실행하면 결과로 PK 로 시작하는 문자열이 나오는데,
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Hacktheon-review/(20240427093846.png)
나온 결과를 ChatGPT에 돌려봤더니 PK는 Zip 파일의 시그니처라고 한다.
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Hacktheon-review/image-2.png)
그래서 다시 "UEsDBA"로 시작하는 것을 파이썬 코드를 이용하여 바이너리 파일로 만들고자 다음과 같은 코드를 짰다.

```python
import base64

encoded_str = "UEsDBAoAAAAAAAAAIQD//중간생략"

decoded_str = base64.b64decode(encoded_str)

print(decoded_str)

file_name = "example.zip"
with open(file_name, "wb") as file:
    # 파일에 텍스트 쓰기
    file.write(decoded_str)

print("파일이 성공적으로 작성되었습니다.")
```
그러자 example.zip이 나왔고, 열어보니 word라는 폴더가 있어 워드 파일이라는 것을 알아챘고, 
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Hacktheon-review//assets/images/Hacktheon-review/20240427094152.png)
확장자를 zip에서 doc으로 바꿔 열면 플래그를 볼 수 있다.
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Hacktheon-review/20240427094226.png)

## 03. Rumor 1
- 분류 : 포렌식

이벤트 로그 파일(.evtx) 파일을 받을 수 있다.
솔직히 이번 문제는 그냥 무지성으로 이것저것 이벤트를 보면서 ip를 찍어 맞춘 것이 얻어걸렸다.

솔직히 윈도우 내장 이벤트 뷰어는 느리다.
Event Log Explorer 프로그램을 체험판으로 사용하여 빠르게 검색 및 필터링하여 이번 문제를 풀었다.
주의해야 할 것은, Home 라이센스를 한 번 사용하면 레지스트리에 뭐가 남는지, 지웠다 깔아도 이전 라이센스를 인식하여 체험판을 써 볼 수가 없다는 점이다.
다음에는 이유를 알고 이게 왜 플래그인지를 분간할 수 있는 능력을 키워야겠다고 생각했다.
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Hacktheon-review/20240427110610.png)

## 04. Rumor 2
- 분류 : 포렌식

Rumor 1에서 이어지는 문제이다.
앞으로 이어지는 Rumor 문제들은 모두 Rumor 1에서 받은 파일을 활용하여 풀 수 있다.
이번 문제에서는 공격자가 pc를 감염시킨 후 세션 연결을 위해 실행시킨 프로세스의 pid를 찾으라고 하였다.
Rumor 1에서 찾은 이벤트를 뒤로 쭉 탐색하면서 모르는 프로그램이 실행되면 검색해 오다가, 예전에 본 적 있는 것 같은 취약점 글을 보았다.
참조 : https://blog.naver.com/best_somansa/222839504756
MS Office 제로데이 취약점으로, sdiagnhost.exe를 사용하여 nc64.exe를 실행시킨다.

pid가 2624라는 것은 역시 이벤트 로그에는 대놓고 나오지 않지만 ChatGPT에 돌려 보았더니 저 부분에 있는 숫자가 pid를 의미한다고 하여 알 수 있었다.
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Hacktheon-review/20240427120212.png)

## 05. Rumor 3
- 분류 : 포렌식

이번에는 공격자가 추가 감염을 위해 스캔한 네트워크 대역을 찾으라고 하였다.
역시 Rumor 2에서 찾은 이벤트보다 이후 이벤트를 쭉 살펴 보다가, 내용이 매우 유사한 이벤트들이 연달아서 있는 것을 발견하여 이상해 살펴보니, ping을 통해 192.168.100.0/24 대역을 스캔하고 있는 것을 알 수 있었다.
문제 설명에 /36이라고 나와 있는 것이 헷갈리게 하였다. 문의를 넣었을 때는 예시일 뿐이라고 하였다.
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Hacktheon-review/20240427120419.png)

## 06. Rumor 4
- 분류 : 포렌식

이번에는 공격자가 네트워크 스캔 이후 리버스 쉘을 열기 위해 사용한 페이로드를 찾으라고 하였다.
이번건 조금 찾는데 오래 걸렸는데, 이것저것 막 시도하다가 이전에 이상하다고 생각했던 이벤트에서 경로를 Cyberchef에 넣고 Magic(이것저것 시도해 주는 기능)을 돌려 보니 진짜 페이로드가 나왔다.
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Hacktheon-review/20240427135258.png)
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Hacktheon-review/20240427135309.png)

## 07. Rumor 5
- 분류 : 포렌식

이번에는 공격자가 최종적으로 유출한 파일 이름을 찾으라고 하였다.
이건 Rumor 4를 할 때 이미 확인해 둔 뒤라 바로 플래그를 찾을 수 있었다.
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Hacktheon-review/20240427135459.png)
파일 이름은 secret.tar.gz이다.

## 08. PNG
- 분류 : 포렌식, misc

예전 대회들을 통해 png의 파일 구조에 대해 들어 보기는 했는데, 자세히 공부는 안 한 바람에 접근 방법을 정하는데 조금 애먹었다.
결과적으로 외부 툴을 사용해서 풀었다. 파일의 시그니처가 깨져 있었던 듯하다. 어느 부분이 잘못되었는지는 전부는 아직 잘 모르겠다.
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Hacktheon-review/20240427170415.png)
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Hacktheon-review/20240427170429.png)
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Hacktheon-review/20240427170447.png)
사용한 툴 : https://github.com/sherlly/PCRT
이것 말고도 이것저것 찾아서 써보긴 했다.
python 2.7과 pip 버전을 python3와 같이 깔아서 쓸 때 주의점을 잘 몰라서 좀 애먹었다.

<!-- # 문제 이야기
![Untitled]({{ site.url }}{{ site.baseurl }}/assets/images/Hacktheon-review/20240427180734.png)

## Decrypt Message 1
c++ 코드를 하나 준다. 

못풀었지만 건드려 본것에 대한 이야기 -->

# 대회 후기
기사나 후기 등을 찾아보니 작년과 비슷한 문제점이 아직 있는 것 같다.
대회 시작 전 운영되던 오픈채팅 관리가 미흡했다. 분탕도 있었고.. 신청 관련해서도 참가 신청 메일이 누락되어서 참가자랑 개별적으로 조율하는 등등 미심쩍은 구석도 있었고, 대회 중에 운영되었던 디스코드 서버도 다른 CTF에서는 끝나고 나서 각 분야별로 질의응답이나 참가자 간 소통을 하도록 해 주는 곳이 많았는데, 그런 것도 없이 대회 끝나고 나서 얼마 지나지 않아 서버 자체가 폭파되었던 것이 가장 아쉬웠다. 그.. 채널 열어주는게 그렇게 힘든 일인가?

대회 운영과는 별개로 내 리버싱 실력은 아직 크게 발전하지 못한 것 같다. 어떤 식으로 뚫어야 하는지 감 잡는 연습을 더 해야 하는 것 같다. 노력을 안 하고서는 실력이 늘을 수가 없지. 드림핵에 문제라도 꾸준히 풀어야 하는데, 방송대 방학때 시동을 걸어 봐야겠다.

그리고 앞에 써둔 풀이를 보면 알겠지만 AI 도움을 정말 많이 받았는데, 공부하는 방식에도 이걸 접목하면 빠르게 내가 모르는 것이 무엇인지, 어떻게 검색해 봐야 할지 알 수 있을 것 같다. 물론, 문제를 푸는 중에 AI로 아무리 검색해도 안 나오던 것이 내 예상과는 다르게 웰노운이라던가, 빙빙 돌아가고 있었다던가 하는 것이 간혹 나오지만, 애초에 모르는 영역이었으니 끝나고 나서 배우는 것에 중점을 둬야겠다.

다음 대회는 무엇이 될 지 모르겠지만, 2024년 처음 CTF였고, 오랜만에 하는 대회였지만 그 긴장감과 몰입하는 느낌, 불탈 때의 재밌음을 다시 한번 알 수 있었다. 내가 이걸 재밌어라 하는구나, 하고. 워게임 연습 좀 더 하고 와야겠다. 대회도 시간 되면 쳐보면서.