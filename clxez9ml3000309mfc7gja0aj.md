---
title: "안드로이드 필드의 Ci/cd 을 알아보자"
datePublished: Fri Jun 14 2024 17:43:45 GMT+0000 (Coordinated Universal Time)
cuid: clxez9ml3000309mfc7gja0aj
slug: cicd
tags: cicd

---

[이전 포스팅(CI/CD, DvOps의 이해)](https://hashnode.com/post/clxe9dwbg000j09l2cuhfb021)에서 소프트웨어 분야의 CI/CD, DevOps 에 대해 간략히 알아봤다.

이번 포스팅에서는 회사 기술 블로그등을 통해 최근 안드로이드 분야의 CI/CD 가 어떻게 구성되어 있는지 확인해보고 이후 포스팅에서 각각의 기술을 좀 더 상세히 학습해보려고 한다.

그럼 시작하자.

# 사전 조사

우선 이전 [라인의 안드로이드 CI&Unit Test 포스팅](https://hashnode.com/post/clxe6apz7000a09l59upebyzm) 에서 알게된 키워드를 위주로 다른 회사들의 사례를 찾아봤다. 그 결과 아래의 참고 자료들을 찾을 수 있었다.

기술 블로그

* [우아한 기술블로그 - 라이더스 개발팀 모바일에서 CI/CD 도입](https://techblog.woowahan.com/2579/)
    
* [딜리셔스 기술블로그 - GitHub Actions로 안드로이드 CI환경 구축하기 (Goodbye Jenkins)](https://dealicious-inc.github.io/2021/03/30/android-ci-with-actions.html)
    

유튜브 영상

* [안드로이드 개발 환경 개선을 위한 노력들: Jenkins CI 파이프라인과 리모트 빌드 소개 / if(kakao)2022](https://www.youtube.com/watch?v=YXi0eipXBKk)
    
* [간단한 DevOps 프로젝트 | App Center에 Android APK 게시 | 초보자 파이프라인](https://www.youtube.com/watch?v=KgH0QzMHXLs)
    
* [Setup GitHub Actions CI For Android Apps](https://www.youtube.com/watch?v=lmjQFn-lQWc)
    
* [What is the CI/CD Pipeline from an Android app perspective?](https://www.youtube.com/watch?v=4nVRJ9ulKJQ)
    
* [\[토크ON세미나\] Jenkins를 활용한 CI/CD 1강 - 젠킨스(Jenkins) 이해 | T아카데미](https://www.youtube.com/watch?v=JPDKLgX5bRg)
    

우선 이정도만 찾기로 하고 환경 설정에 관한 자료는 이후 포스팅에서 참고 하겠다.

# 자료 요약

각각의 자료는 개별 포스트에 정리하고 요약한 내용을 이 포스팅에 작성하겠다.

* [\[Video Review\]카카오 모빌리티 팀의 안드로이드 CI 개선 사례](https://hashnode.com/post/clxee2iiu000q09lcfqnfc1r4)
    
* [\[Article Summary\]우아한 기술 블로그 라이더스 개발팀 모바일의 CI/CD 도입](https://hashnode.com/post/clxeglhph000209lcao3q2znv)
    
* [\[Article Summary\]딜리셔스 클라이언트 안드로이드 개발팀의 GitHub Actions로 CI 구축하기](https://hashnode.com/post/clxexajhh000208lha9anfsl1)
    

# 선례 조사 전략

먼저 안드로이드 개발 팀이 CI/CD 를 적용하기 위해 어떤 툴을 사용하는지 파악하기 위해서, 개발팀의 회고록을 보며 다음과 같은 내용들을 파악했다.

* 어떤 기술을 옵션으로 뒀는지
    
* 각 팀이 선택한 툴은 무엇이고 선택의 근거가 무엇인지
    
* 환경 설정에 관한 내용이 있다면, 난이도는 어떤지
    

이렇게 자료를 리뷰 후에 조사한 안드로이드 개발 팀들은 크게 Jenkins, GitHub Actions 를 옵션으로 고려하고 각각의 상황에 맞게 툴을 선택했다는 것을 알 수 있었다.

각각의 선택이 나뉘는데는 Jenkins 와 GitHub Actions 의 특성이 다르다는 것을 이해할 필요가 있어보였다. 그래서 각각의 특성을 아래 표로 정리해봤다.

### GitHub Actions vs Jenkins

| **항목** | **GitHub Actions** | **Jenkins** |
| --- | --- | --- |
| **개요** | GitHub 리포지토리와 통합된 CI/CD 도구 | 오픈 소스 자동화 서버 |
| **설치 및 설정** | GitHub 리포지토리에 내장, 별도 설치 불필요 | 독립형 서버로 설치 필요, 다양한 플러그인 설정 가능 |
| **플랫폼 지원** | 클라우드 기반 | 자체 호스팅, 클라우드 및 온프레미스 지원 |
| **유연성** | GitHub 리포지토리와 긴밀한 통합 | 다양한 플러그인으로 높은 유연성 제공 |
| **사용 편의성** | GitHub 사용자에게 직관적이고 쉬운 설정 | 설정 복잡도는 다소 높지만, 커스터마이징 가능 |
| **확장성** | GitHub Marketplace를 통해 확장 가능 | 수천 개의 플러그인으로 기능 확장 가능 |
| **파이프라인 정의** | YAML 파일로 파이프라인 정의 | Jenkinsfile로 파이프라인 정의 |
| **커뮤니티 및 지원** | GitHub 커뮤니티와 지원 포럼 | 대규모 커뮤니티와 다양한 지원 채널 |
| **비용** | GitHub의 프리티어 및 유료 플랜 | 오픈 소스(무료), 추가적인 플러그인 및 서버 비용 발생 |
| **보안** | GitHub의 보안 기능 및 GitHub Actions 보안 정책 | Jenkins 자체 보안 설정 필요, 다양한 플러그인 보안 설정 |
| **통합** | GitHub의 모든 서비스와 네이티브 통합 | 타사 서비스와 광범위한 통합 가능 |
| **빌드 속도** | GitHub의 클라우드 인프라로 신속한 빌드 | 인프라 설정에 따라 다름, 최적화 필요 |

가장 큰 차이점은 GitHub Actions 가 GitHub 이 제공하는 유료 서비스인 반면 Jenkins 는 오픈소스이기 때문에 추가 비용 지출에 대한 측면을 고려하지 않을 수 없다는 것이었다.

나는 학습을 위해 조사를 하고 있는 것이기 때문에 각각의 차이에 대해 가볍게 짚은 채로 이후 포스팅에서 각각의 환경을 구현하고 사용해보는 포스팅을 작성하겠다.