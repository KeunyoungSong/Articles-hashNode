---
title: "[Article Summary](주)딜리셔스 클라이언트 안드로이드 개발팀의 GitHub Actions로 CI 구축하기"
datePublished: Fri Jun 14 2024 16:48:28 GMT+0000 (Coordinated Universal Time)
cuid: clxexajhh000208lha9anfsl1
slug: article-summary-github-actions-ci
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1718875183042/4480dfba-c4f1-4384-97d2-8f4684866e71.png

---

[안드로이드 분야의 CI/CD 포스팅](https://hashnode.com/preview/666bb7b91328932b05ad0178) 을 작성하기 위해 학습한 내용을 따로 요약하기 위한 포스팅입니다.

# 요약

2020년 초 클라이언트 개발팀의 repository 를 Bitbucket 에서 Github 으로 이동

GitHub Actions 를 알게 됨

Github Actions 는 GitHub repo 와 직접 연동해서 쓸 수 있는 CI/CD 도구, 개발 후 배포 과정을 자동화, 커스텀, 실행하도록 도와준다.

즉, Build, Test, Deploy 를 자동화하는 서비스.

GitHub Actions 는 안드로이드 기본 빌드 스크립트를 제공함

안드로이드의 Build 시스템은 Jenkins 를 사용하고 있었고 이후 빌드 시스템을 GitHub Actions 로 옮김

이 포스팅에는 GitHub Actions의 개략적인 소개, 구축 방법, 환경 변수를 이용해 키스토어 파일과 credencial 정보를 안전하게 사용하는 방법, S3를 이용하여 APK 바이너리를 저장하고 공유하는 방법의 내용을 담고 있으며 Unit Test, UI Test 등의 내용은 없다.

### GitHub Actions 개요

빌드 스크립트 파일은 YAML 을 사용함.

많은 플러그인 보유(안드로이드 관련 포함): [확장프로그램 목록](https://github.com/marketplace?type=actions)

비용: [비용 문서](https://docs.github.com/en/billing/managing-billing-for-github-actions/about-billing-for-github-actions)

* Public repo: 무료
    
* Private repo: 2000분 빌드 시간 제공
    

빌드 가상 머신 서버: [지원 서버 목록](https://github.com/actions/runner-images)

* 가상 머신은 무료로 제공해주는 것을 사용하거나 직접 호스팅 할 수 있다.
    
* OS: Windows, Ubuntu, macOS 제공
    
* 여러 소프트웨어가 설치되어 있음.(안드로이드 관련 개발 도구 포함)
    

환경 변수 사용

* Android 빌드 시 필요한 키 정보(Keystore file, Keystore password, Key alias, Key password, Store password) 등을 repo에 저장하고 빌드시에 사용할 수 있음
    

### 안드로이드 개발자 (서버 레벨 1)의 Jenkins 경험기

* Jenkins 는 WebBase 서비스라 서버가 1대 필요하다
    
    * 클라우드 서비스에서 서버 인스턴스를 생성
        
    * 각종 소프트웨어를 설치. ex) JDK, Android SDK, FTP, Tomcat 등
        
    * 보안을 위해 여러 네트워크 규칙을 적용(포트 포워딩, 리다이렉트, 방화벽)
        
* Jenkins 를 설치하고 각종 설정을 진행한다
    
    * jenkins 홈페이지에서 안드로이드 빌드를 위해 각종 플러그인(gradle, artifect 등)을 설치
        
    * Jenkins 에 Android Build 를 위한 환경 변수 설정을 한다
        
    * git repo 의 credential 정보를 Jenkins 설정에 입력한다
        

### GitHub Actions 는 위 과정이 필요 없다.

서버 운영 필요 없음

필요한 모든 설정은 빌드 스크립트에 작성. 모든 필요한 빌드 정보는 프로젝트 내 1개 파일에 담김.

### 이후 환경 구축 방법(생략)

---

# 참고

* [https://arc.net/l/quote/avwmhabn](https://arc.net/l/quote/avwmhabn)