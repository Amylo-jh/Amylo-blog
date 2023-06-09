---
title: "(WIP) Synology Nas에 Docker로 Jekyll 블로그 만들기"
categories:
 - Jekyll
tags:
 - 지킬
 - Jekyll
 - Synology
 - 시놀로지
 - Docker
 - 도커
 - 서비스 설치
docker_registry:
- url: /assets/images/jekyll_howto/docker_registry.png
  image_path: /assets/images/jekyll_howto/docker_registry.png
toc : true
toc_label: "목차"
toc_sticky : true
last_modified_at: 2021-07-03T23:50
---
나스가 있어도, 없어도 내 블로그를 만들어 보는건 어떨까요?

# Installing Jekyll using Docker at Synology NAS
참조한 사이트 :  
1. [Synology nas에서 docker를 이용한 jekyll 사용하기](https://blog.actin.kr/devlog/2019/08/14/synology-docker-jekyll/)
2. [Synology docker에 jekyll install하기](http://webcache.googleusercontent.com/search?q=cache:ev9ddroF55MJ:www.ripudblog.com/skills/2020/08/22/Synology_docker%25EC%2597%2590_jekyll_install%25ED%2595%2598%25EA%25B8%25B0.html+&cd=2&hl=ko&ct=clnk&gl=kr)
*(캐시된 사이트, 1번 사이트의 부가설명)*
3. [Minimal Mistakes 테마](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/)
4. [git hook 설정하기](https://jesse.sh/deploys-with-git-and-jekyll/)
4. [_config.yml 수정하기 / Navigation 설정하기](https://junhobaik.github.io/jekyll-apply-theme/)
5. [favicon 추가하기](https://frhyme.github.io/blog/jekyll_add_Favicon/)
6. [콘텐츠 영역 너비 조정하기](https://eona1301.github.io/github_blog/GithubBlog-Content-Width/)
7. [Markdown 사용방법](https://ansohxxn.github.io/blog/markdown/)
8. [liquid 사용방법](https://ansohxxn.github.io/blog/liquid/)
9. [Hits, 조회수 사용방법](https://choiseonjae.github.io/jekyll/hits/)

미분류 :  

[google analytics 적용방법](https://jhlov.github.io/jekyll-%EB%B8%94%EB%A1%9C%EA%B7%B8%EC%97%90-google-analytics-%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0/)  

[사이드바 추가하기](https://jhlov.github.io/jekyll-%EB%B8%94%EB%A1%9C%EA%B7%B8%EC%97%90-%EC%82%AC%EC%9D%B4%EB%93%9C%EB%B0%94-%EC%B6%94%EA%B0%80%ED%95%98%EA%B8%B0/)

## 남는 나스를 활용하자!
단순히 jekyll 블로그를 만들기 위해서라면 github에서 제공하는 서비스를 활용하여 할 수도 있지만 집에서 놀고 있는 Nas를 활용하여 개인 블로그를 만들어 보고 싶었다.

이전에 했던 삽질로는 Wordpress를 시놀로지 패키지 매니저에서 제공하는 것으로 설치해 보기, 그것도 안 돼서 docker로 설치해 보기도 했지만, ~~(더 무모했다)~~ 결국 실패해서 손을 놓고 있었다.

시간이 지나 다시 블로그를 만들어 보고 싶다는 생각이 들어 블로그 관련 내용을 찾아보던 중, markdown을 활용해 간단한 문법만으로 보기 좋은 양식을 만들어 주는, 정적 웹 페이지 생성기인 **지킬(Jekyll)** 이 있다는 것을 알게 되었다.

개인적으로는 이것저것 설치하다가 잔여 파일이나 설정이 꼬여 삽질하는 것을 굉장히 싫어해서, Docker를 활용하여 설치해 보기로 하였다.

## 설치 순서
1. Docker에 Jekyll 이미지 받기
2. Jekyll 테마 고르기 (Minimal-Mistakes 기준)
3. Dockerfile 작성 / 빌드
4. Git 서버 구축
5. Git 로컬 구축 / Hook 설정
6. _config.yml 설정하기
7. 네비게이션 메뉴 설정하기
8. https / 역방향 프록시 적용하기
9. 

## Docker에 Jekyll 이미지 받기
(사용환경 : DS918+)  
우선 Synology Package Manager에서 Docker를 설치해 줍니다.
아마 Docker를 위한 공유 폴더가 만들어질 겁니다.
이후 Docker를 실행하여 레지스트리에서 Jekyll을 검색하여 해당 이미지를 다운받아 줍니다.  
(글 작성 시점에서는 DSM 7.0을 설치하여 DSM 6.2버전과는 다르게 보일 수 있으나 기본 구성은 동일합니다.)

{% include gallery id="docker_registry" caption="도커 레지스트리" %}



작성 시작일 : 2021.06.27

---
<mark style='background-color: #dcffe4'>
<b>글에 오류가 있거나 궁금한 것, 의견 등이 있으면 자유롭게 댓글로 달아주시면 감사하겠습니다!</b>
</mark>