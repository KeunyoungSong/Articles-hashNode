---
title: "[Article Summury]우아한 기술 블로그 라이더스 개발팀 모바일의 CI/CD 도입"
datePublished: Fri Jun 14 2024 09:01:06 GMT+0000 (Coordinated Universal Time)
cuid: clxeglhph000209lcao3q2znv
slug: article-summury-cicd

---

[안드로이드 분야의 CI/CD 포스팅](https://hashnode.com/preview/666bb7b91328932b05ad0178) 을 작성하기 위해 학습한 내용을 따로 요약하기 위한 포스팅입니다.

# 요약

### 팀 설명

라이더스 개발팀의 포스팅

라이더스 개발팀은 배달되지 않는 음식점의 음식을 민트색 핼멧을 쓴 라이더 분들이 오토바이를 이용하여 음식을 익업 후 고객님께 배달하는 일정 과정들을 원활하고 효율적으로 운영이 될 수 있도록 개발하는 팀이다.

라이더스는 B2B앱으로써 일반 사용자를 위한 앱이 아닌 라이더스 기사님들을 위한 앱이다.

### CI/CD 를 시작하게 된 계기

라이더스는 한번 배포가 이루어 질 때 3개의 각기 다른 주소를 가진 5개의 apk 파일이 만들어지고 Github 및 베타와 운영 시스템에 배포가 이루어 지게 된다.

수동으로 개발자의 손을 통해서 배포가 이뤄지다 보니 Human Error 의 발생의 소지가 있고 앱은 서버의 배포와 달리 한번 잘못 배포가 되면 다시 배포하는 과정의 어려움이 커 최대한 자동화를 시키려고 함.

### Jenkins for Android Using Docker 도입 배경

CI/CD를 위한 툴로는 여러가지가 있다.

Travis CI, Circle CI, BITRISE, Jenkins 등이 있다. 이 중 Jenkins 를 사용할 것.

#### Jenkins 의 장점

* 무료
    
* 사용자 정이 옵션
    
* 방대한 양의 플러그인
    
* 다양한 적용 사례 및 풍부한 레퍼런스
    
* Remote access API 제공
    

Docker 를 선택한 이유

젠킨스 서버를 띄우기 위해서는 여러가지의 서버 설정등과 설치들이 필요하다.

모든 일련의 과정을 Docker file 에 작성하고 손쉽게 띄울 수 있는 도커를 선택해 작업 시간 및 운영에 필요한 리소스를 줄이고자 함

### 기대하는 이상적 시스템

![이상적인 시스템](https://techblog.woowahan.com/wp-content/uploads/img/2018-06-26/pic01.png align="left")

내부적으로 Slack을 사용중

APK를 타겟 시스템 별로 아래 그림과 같이 배포하도록 만듬

Slack에서 bot api 를 지원해줘서 아래처럼 관리함

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718355799107/96c1b676-0b3a-4bac-aa8b-1c7c25f8ae0f.png align="center")

### Docker For Jenkins 이미지를 만들기 위한 DockerFile 작성

Docker 는 docker hub 라는 repository 가 있다. github 과 비슷.

간단하게 Jenkins official dockerfile 을 이용하고 약간의 추가 세팅으로 Jenkins 사용을 위한 docker image 를 만듬

Jenkins Official 이미지는 Android SDK 등이 포함되지 않았음

공식 이미지 레포를 클론해 몇 가지 세팅으로 커스텀 후에 사용

Gradle , OpenJDK, Android SDK를 추가함

이렇게 CI/CD를 도입해 배포 절차를 자동화하면 아래 같은 번거로움에서 해방될 수 있다

![카톡](https://techblog.woowahan.com/wp-content/uploads/img/2018-06-26/pic04.png align="left")

# 참고

* [https://techblog.woowahan.com/2579/](https://techblog.woowahan.com/2579/)