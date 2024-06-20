---
title: "[Video Summary]GitHub Actions로 CI/CD 환경을 만들어보자"
datePublished: Mon Jun 17 2024 09:25:15 GMT+0000 (Coordinated Universal Time)
cuid: clxirs4cy000m09la5tmp0ote
slug: video-summary-detekt
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1718875131876/0b262cc6-3647-4a64-a545-b11add62ca5f.png

---

이번 포스팅에서는 이전에 다룬 **CI/CD와 DevOps** 및 **안드로이드 앱 개발의 CI/CD 개요**를 실제 환경에서 구현해보는 실습을 해보겠습니다. 여기서는 GitHub Actions 를 사용해보겠습니다.

#### 이전 포스팅

* [CI/CD, DevOps의 이해](https://hashnode.com/post/clxe9dwbg000j09l2cuhfb021)
    
* [안드로이드 앱 개발의 CI/CD Overview](https://hashnode.com/post/clxez9ml3000309mfc7gja0aj)
    

본 내용은 아래 유튜브 강좌를 통해 학습한 내용을 정리한 것입니다.

* [**detekt: a static code analysis linter tool for Kotlin Android projects!**](https://www.youtube.com/watch?v=o384NbCDB0U&t=31s)
    
* [**GitHub Actions | Detekt | Android | 2023**](https://www.youtube.com/watch?v=xDL4iyr_Oq8&t=30s)
    

# 요약

## detekt: a static code analysis linter tool for Kotlin Android projects!

[https://github.com/detekt/detekt](https://github.com/detekt/detekt)

Detekt 는 Kotlin 프로젝트를 위해 Kotlin 에서 개발한 Linter 도구로 **코드 품질을 담당**합니다.

* 추가 플러그인을 통해 ktlint 기반의 포메팅을 지원하는 의존성을 추가할 수 있습니다.[(문서)](https://detekt.dev/docs/intro#adding-more-rule-sets)
    

(ktlint는 코드 형식 지정을 담당합니다.)

Kotlin 컴파일러에서 제공하는 추상 구문 트리에서 작동하는 정적 코드 분석 도구이다.

이 라이브러리는 아래와 같은 것을 찾아내래 수 있습니다.

* 잠재적인 코드 냄새
    
* 잠재적인 버그
    
* 잠재적인 성능 문제
    
* 잠재적인 보안 문제
    
* 코드 복잡성
    
* 휴먼 에러
    

Detekt 에는 전체 프로젝트에 걸쳐 정의할 수 있는 고도로 구성 가능한 규칙이 있다.

이런 규칙은 팀에 대한 기본 규칙을 정의하여 새로운 팀 구성원의 온보딩을 더 쉽게 만든다.

Detekt 는 CLI, Gradle plugin, 코딩 중 즉각적인 피드백을 표시하는 IntelliJ 플러그인이 있다.

또한 내부적으로 ktlint를 래핑하고 사람이 읽을 수 있는 HTML 보고서나 구문 분석을 위한 XML 보고서를 생성할 수 있다.

보고서는 경고 및 오류로 제공되며 더 높은 우선순위부터 해결해야 한다.

모든 규칙을 활성화하고 프로젝트의 기준선을 정의하여 CI 검사에서 이를 수행할 수 있다.

---

## GitHub Actions | Detekt | Android | 2023

만약 회사에서 하나의 파일 내에 100 줄 이상의 코드를 작성하지 않기로하고 또 5개 이상의 메서드를 하나의 클래스 내에 작성하지 않기로 했다고 합시다. 그럼 이런 규칙을 검토하는 것은 번거로운 일이 될 것이고 이를 정적 분석 툴로 대신할 수 있습니다.

인덴트 사이즈 규칙, 한 줄당 최대 글자 수 등 많은 것을 설정할 수 있습니다. 설정 가능한 내용은 문서를 참고 [(문서)](https://detekt.dev/docs/rules/formatting)

### 사용법

1. 프로젝트와 모듈에 detekt 의존성 추가
    
2. gradle app:detekt 명령 실행 -&gt; 많은 에러 발생
    
3. app - build - reports - detekt 경로에가면 에러 로그를 볼 수 있음
    
4. 에러가 발생하는 이유는 base 라인을 정하지 않았기 때문이다.
    
    1. 현 시점에서 베이스라인을 생성하면, 현재 발생하는 모든 오류 목록을 가지는 파일을 만들고 다음 테스팅 시 베이스라인에 선언된 에러는 에러를 발생시키지 않는다.
        
5. gradle app:detektBaseline 명령 실행 -&gt; 발생했던 에러 목록 파일 생성
    
6. 다시 gradle app:detekt 명령 실행 -&gt; 에러가 발생하지 않음
    
7. 한 줄에 기본 설정 MaxLineLength 보다 긴 코드를 작성하고 다시 테스팅하면 베이스 라인에 없던 에러가 발생한다
    
    1. ...MainActivity.kt:22:1: Line detected, which is longer than the defined maximum line length in the code style. \[MaxLineLength\]
        
8. 기본 값을 커스텀하여 팀에서 요구하는 설정을 만들자
    
    1. gradle :app:detektGenerateConfig 수행
        
        1. /config/detekt/detekt.yml 파일 생성 (기본 규칙이 정의된 파일)
            
    2. maxLineLength를 220 으로 설정하고 테스트하면 이전에 발생한 에러가 발생하지 않는다.
        
9. 이 정적 도구 분석을 메인 브랜치에 PR 생성 시 자동으로 수행되게 할 것이다.
    
    1. 먼저 GitHub Actions 를 알아야 한다.
        
10. 프로젝트에 Github Actions 를 설정하자
    
    1. root 밑에 .github 폴더 생성
        
    2. .github/ 에 workflow 폴더 생성
        
    3. workflow/ 에 detekt-actions.yml 파일 생성
        
    4. 워크 플로우를 해당 파일에 명세 후 push
        
    5. 이제 다른 브랜치로 이동해 테스트한다
        
11. test-branch 생성
    
    1. 변경 사항 작성 후 push
        
    2. PR 생성
        
    3. 자동으로 Github Actions 가 동작되어 PR 에서 진행 상황을 볼 수 있다.
        

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718616081715/50d9bebd-95ec-413e-895a-ac432dbe3b18.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718616128166/0b83763a-9265-4cc9-8972-0e044d4ee6e6.png align="center")