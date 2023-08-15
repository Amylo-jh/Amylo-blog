---
title: "Github을 이용하여 PC와 Andriod간 Obsidian 동기화 하기"
categories:
 - Obsidian
 - Tutorials
tags:
 - Obsidian
 - Github
 - Termux
toc : true
toc_label : "목차"
toc_sticky : true
last_modified_at: 2023-08-15T20:02
---

# 준비물
- App : Termux, Obsidian
	- Termux 앱은 이후 초기 세팅 내용에서 설치 방법을 다룹니다.
	- Obsidian 앱은 플레이 스토어에서 설치해 주시면 됩니다.
- Obsidian 메모 저장소로 사용할 Github Repo
- 예상 소요 시간 : 15~30분

# 요구사항
- 데스크톱에 이미 옵시디언을 설치하고, vault를 생성 후 깃헙과 연동한 상태를 전제로 시작합니다.
	- 정상 작동 테스트용으로 아무 마크다운 파일을 하나 넣고 내용을 조금 입력해 두는 것을 권장합니다.

# 초기 세팅
- 데스크톱에서 세팅을 시작합니다.
- .gitignore에 다음 내용을 추가합니다. 안드로이드와 PC간의 설정 파일이 서로 오염되지 않도록 하기 위함입니다.
	- .obsidian/workspace-mobile.json
	- .obsidian/workspace.json
	- .obsidian/app.json
- Termux 앱의 가장 최신 버전을 설치해 줍니다. 보안 경고가 뜨지만 무시하기를 누르고 설치해 줍니다.
	- https://github.com/termux/termux-app/releases

# Termux 세팅

## 패키지 세팅
- Termux 앱을 실행합니다.
- 현재 설치된 패키지를 업데이트 하기 위해 커맨드라인에 다음 명령을 입력합니다.
	- pkg upgrade && pkg update
- 중간중간 몇번 Y를 눌러 주어야 합니다. 10번 내외입니다.
- 다음으로는 github과 연동할때 필요한 git 패키지를 설치합니다.
- 커맨드라인에 다음 명령을 입력합니다.
	- pkg install git
		- Y를 한번 눌러 줘야 할 수 있습니다.

## Termux 권한 및 Git setting
- Termux에서 다음 명령어를 입력합니다.
	- termux-setup-storage
		- 파일 접근 권한을 요청하는 알림이 뜹니다. 승인해 줍니다.
	- cd ~/ 
	- ls
- ls 명령어까지 실행하면 storage 라는 폴더가 보일 것입니다.
	- 만약 안 뜬다면 exit 명령어로 termux를 종료 후 재실행해 보세요.
- 저의 경우 모바일(안드로이드)에서 Obsidian 앱이 vault로 사용할 폴더 경로는 아래와 같이 설정할 예정입니다.
	>통상적인 파일 탐색기 앱으로 들어갔을 때, Document 폴더 하위에 Obsidian-Repo 폴더.
- 이를 설정하기 위해 Termux에서 다음 경로로 이동해 줍니다.
	- cd ~/storage/shared/Documents
- 이후 깃헙에서 미리 준비한 파일들을 클론합니다.
	- git clone <깃 레포 주소.git>

# Github Access Token 설정
- Github - Setting - Developer setting - Personal access tokens - Fine-graned tokens 로 이동해 줍니다.
- Generate New Token으로 새로운 토큰을 만들어 줍니다.
	- 토큰 이름은 자유롭게 설정하시면 됩니다.
	- 기간 또한 자유롭게 설정하시면 됩니다.
	- 설명 또한 자유롭게 적어 주시면 됩니다.
	- Repository Access는 Only select repositories를 설정하여 옵시디언 관련 리포에만 접근할 수 있도록 하는 것을 추천드립니다.
	- Repository Permission의 경우 다음을 변경해 줍니다.
		- Content : Read and Write
		- Pull Requests : Read and Write
- 위 설정이 끝나면 Generate Token으로 토큰을 생성해 줍니다.
- 생성 이후 나오는 토큰 코드가 나오는 화면은 해당 화면을 벗어나면 다시 볼 수 없고, 재생성만 할 수 있으므로 해당 값을 복사해서 잠시 메모해 둡니다.- 

# Obsidian Vault 폴더 설정
- 안드로이드에 설치한 Obsidian 앱을 켜면, 처음에 파일 접근 권한을 요구합니다. 승인해 줍니다.
- Open folder as vault를 선택후, 깃헙 레포를 클론한 폴더를 선택해 줍니다.
	- 테스트 파일을 만들었다면, 해당 파일이 보이는 곳으로 간다면 성공입니다.
	- 이후 폴더 내용을 신뢰할 것인지 물어 봅니다. 신뢰한다고 해 줍니다.

# Obsidian 앱 내 Extension 설정
- Community Plugins 에서 다음의 플러그인들을 설치 및 활성화 해 줍니다.
	- Obsidian Git
	- Advanced Tables (선택사항)
	- Table of Contents (선택사항)
- Obsidian Git 설정으로 와 줍니다.
- 다음 설정들을 세팅해 줍니다.
	- 저의 경우 다음과 같이 설정하였습니다. (추천, 필수는 아닙니다.)
		- Vault backup interval (minutes) : 5
		- Auto Backup after file changes : ON
		- Commit message on auto backup/commit : vault automatic backup: {{date}} / {{hostname}}
		- Commit message on manual backup/commit : vault backup: {{date}} / {{hostname}}
		- {{hostname}} placeholder replacement : Phone
		- Pull updates on startup : ON
		- Push on backup : ON
		- Pull changes before push : ON
	- 간혹 git이 꼬이는 것을 원치 않으시는 분은 수동 저장을 전제로 자동 백업/커밋 기능을 꺼 주시면 됩니다.
	- Authentication/Commit Author 카테고리에서 다음을 설정해 줍니다.
		- Username on your git server
			- 깃헙 아이디를 입력해 줍니다. 저의 경우 Amylo-jh.
		- Password/Personal access token
			- Github Access Token때 생성한 토큰을 붙여넣어 줍니다.
		- Auther name for commit
			- 자유롭게 입력해 줍니다.
			- 저는 Amylo-Phone이라고 설정하여 기기 구분이 용이하도록 하였습니다.
		- Auther email for commit
			- 사용 중인 이메일 주소를 입력해 줍니다.
- 위 설정이 모두 끝나면 뒤로가기를 눌러 설정을 마무리 해 줍니다.

# 단축키 설정 (선택)
- 매번 5분을 기다리거나 수동으로 깃 커밋, 풀, 푸시하기 귀찮으니 단축키를 매핑하여 즉시작업을 수행할 수 있도록 해 줍니다.
- 안드로이드 옵시디언 - 설정 - Hotkeys 에서 git 이라고 검색하면 git 확장과 관련된 단축키 옵션만 나옵니다.
	- Obsidian Git : Commit all changes : Ctrl + Shift + S (Save)
	- Obsidian Git : Pull : Ctrl + Shift + L (Load)
	- Obsidian Git : Push : Ctrl + Shift + P (Push)
	- 를 추천드립니다.

# Git Troubleshooting
- 간혹 git에서 merge가 불가능하거나, pull, push가 안 되는 상황이 발생할 수 있습니다.
- 아직 저도 완벽한 해결 방안을 찾지는 못했지만, 그때그때 상황에 따라 인터넷을 참고하여 conflict를 고치고 있습니다.
- 데스크탑의 경우 vscode의 git extension을 활용하거나, cli상으로 문제가 되는 부분을 고쳐 주시면 됩니다.
- Android의 경우, termux를 켜서 Obsidian Vault가 있는 곳까지 이동하신 다음, cli상으로 문제가 되는 부분을 고쳐 주시면 됩니다.

# 마무리
- 여기까지 오셨다면 PC, 안드로이드 모두 옵시디언 앱을 켤 때 깃헙에서 최신 정보를 자동으로 불러오고, 앱을 종료하기 전 커밋, 푸시를 통해 깃헙에 내용을 저장하여 항상 최신의 정보를 동기화 할 수 있게 됩니다.
- 만약 더욱 상세한 설명이나 도움이 필요하시다면, 언제든지 댓글이나 디스코드, 트위터 등으로 연락 남겨 주시기 바랍니다. 최선을 다해 도움을 드리겠습니다.

<mark style='background-color: #dcffe4'>
<b> 글에 오류가 있거나 궁금한 것, 의견 등이 있으면 자유롭게 댓글로 달아주시면 감사하겠습니다!</b></mark>
