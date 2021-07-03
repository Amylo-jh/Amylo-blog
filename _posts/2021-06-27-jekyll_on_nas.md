---
title: "(작성중) Synology Nas에 Docker로 Jekyll 블로그 만들기"
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
last_modified_at: 2021-07-03T23:50
comments: true
toc : ture
toc_label: "목차"
---
작성 시작일 : 2021.06.27

# Installing Jekyll using Docker at Synology NAS
참조한 사이트 :  
1. [Synology nas에서 docker를 이용한 jekyll 사용하기](https://blog.actin.kr/devlog/2019/08/14/synology-docker-jekyll/)
2. [Synology docker에 jekyll install하기](http://webcache.googleusercontent.com/search?q=cache:ev9ddroF55MJ:www.ripudblog.com/skills/2020/08/22/Synology_docker%25EC%2597%2590_jekyll_install%25ED%2595%2598%25EA%25B8%25B0.html+&cd=2&hl=ko&ct=clnk&gl=kr)
*(캐시된 사이트, 1번 사이트의 부가설명)*
3. [Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/)
4. 

## 남는 나스를 활용하자!
단순히 jekyll 블로그를 만들기 위해서라면 github에서 제공하는 서비스를 활용하여 할 수도 있지만 집에서 놀고 있는 Nas를 활용하여 개인 블로그를 만들어 보고 싶었다.

이전에 했던 삽질로는 Wordpress를 시놀로지 패키지 매니저에서 제공하는 것으로 설치해 보기, 그것도 안 돼서 docker로 설치해 보기도 했지만, ~~(더 무모했다)~~ 결국 실패해서 손을 놓고 있었다.

시간이 지나 다시 블로그를 만들어 보고 싶다는 생각이 들어 블로그 관련 내용을 찾아보던 중, markdown을 활용해 간단한 문법만으로 보기 좋은 양식을 만들어 주는, 정적 웹 페이지 생성기인 **지킬(Jekyll)** 이 있다는 것을 알게 되었다.

개인적으로는 이것저것 설치하다가 잔여 파일이나 설정이 꼬여 삽질하는 것을 굉장히 싫어해서, Docker를 활용하여 설치해 보기로 하였다.

## 설치 순서
1. Docker에 Jekyll 이미지 받기
2. Jekyll 테마 고르기 (Minimal-Mistakes 기준)
3. Dockerfile 작성 / 빌드
4. _config.yml 설정하기
5. https, 역방향 프록시 적용하기
6. 

## Docker에 Jekyll 이미지 받기
(사용환경 : DS918+)  
우선 Synology Package Manager에서 Docker를 설치해 줍니다.
아마 Docker를 위한 공유 폴더가 만들어질 겁니다.
이후 Docker를 실행하여 레지스트리에서 Jekyll을 검색하여 해당 이미지를 다운받아 줍니다.  
(글 작성 시점에서는 DSM 7.0을 설치하여 DSM 6.2버전과는 다르게 보일 수 있으나 기본 구성은 동일합니다.)

{% include gallery id="docker_registry" caption="도커 레지스트리" %}

