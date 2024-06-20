---
title: "CI/CD, DevOps의 이해"
datePublished: Fri Jun 14 2024 05:39:14 GMT+0000 (Coordinated Universal Time)
cuid: clxe9dwbg000j09l2cuhfb021
slug: cicd-devops
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1718875246602/2db3fbd2-c988-4542-a416-5a516cef790e.png
tags: devops, cicd

---

## 서론

[이전 포스팅(라인의 안드로이드 CI&Unit Test)](https://hashnode.com/post/clxe6apz7000a09l59upebyzm)에서 라인의 안드로이드 개발자분들께서 앱을 개발하고 유지보수하기 위해 어떤 방법들을 사용하시는지 들어볼 수 있었다.

영상을 리뷰하며 얕게 관련 키워드를 찾아봤고 각각의 키워드를 우선 순위를 정해 개별 포스팅으로 좀 더 깊게 파고들어 볼 생각이다.

그럼 시작하자.

# CI/CD 그게 뭐야?

위키에 따르면,

> In software engineering, CI/CD or CICD is the combined practices of continuous integration (CI) and continuous delivery (CD) or, less often, continuous deployment. They are sometimes referred to collectively as continuous development or continuous software development.

*소프트웨어 엔지니어링에서 CI/CD란 지속적 통합(CI)과 지속적 배포(CD) 또는 덜 자주 사용되는 지속적 배포(CD)의 결합된 관행이다. 이를 통칭하여 지속적 개발 또는 지속적 소프트웨어 개발이라 한다.*

흠 delivery와 deployment의 차이는 뭐지? 잘 와닿지 않는다 좀 더 보자.

> **Continuous integration**  
> Frequent merging of several small changes into a main branch.
> 
> **Continuous delivery**  
> When teams produce software in short cycles with high speed and frequency so that reliable software can be released at any time, and with a simple and repeatable deployment process when deciding to deploy.
> 
> **Continuous deployment**  
> When new software functionality is rolled out completely automatically.

***지속적 통합***: 여러 가지 작은 변경 사항을 기본 분기로 자주 병합한다.

***지속적인 전달***: 팀이 짧은 주기에서 빠른 속도와 빈도로 소프트웨어를 제작하여 언제든지 신뢰할 수 있는 소프트웨어를 출시할 수 있고, 배포를 결정할 때 간단하고 반복 가능한 배포 프로세스를 갖춘 경우.

***지속적 배포***: 새로운 소프트웨어 기능이 완전히 자동으로 배포되는 경우.

> ### Motivation
> 
> CI/CD bridges the gaps between development and operation activities and teams by enforcing automation in building, testing and deployment of applications. CI/CD services compile the incremental code changes made by developers, then link and package them into software deliverables.[<sup>[3]</sup>](https://en.wikipedia.org/wiki/CI/CD#cite_note-3) Automated tests verify the software functionality, and automated deployment services deliver them to end users.[<sup>[4]</sup>](https://en.wikipedia.org/wiki/CI/CD#cite_note-4) The aim is to increase early defect discovery, increase productivity, and provide faster release cycles. The process contrasts with traditional methods where a collection of software updates were integrated into one large batch before deploying the newer version. Modern-day [DevOps](https://en.wikipedia.org/wiki/DevOps) practices involve:
> 
> * continuous development,
>     
> * [continuous testing](https://en.wikipedia.org/wiki/Continuous_testing),
>     
> * continuous integration,
>     
> * continuous deployment, and
>     
> * [continuous monitoring](https://en.wikipedia.org/wiki/Continuous_monitoring)
>     

*CI/CD는 애플리케이션 빌드, 테스트 및 뱁포를 자동화하여 개발과 운영 활동 및 팀 간의 격차를 해소한다. CI/CD 서비스는 개발자의 점진적인 코드 변경 사항을 컴파일한 다음 소프트웨어 결과물로 연결하고 패키징한다. 자동화된 테스트는 소프트웨어 기능을 검증하고 자동화된 배포 서비스는 최종 사용자에게 지공한다. <mark>그 목표는 조기 결함 발견을 늘리고 생산성을 높이며 릴리즈 주기를 단축하는 것</mark>****<mark>.</mark>*** *이 프로세스는 최신 버전을 배포하기 전에 소프트웨어 업데이트 모음을 하나의 큰 배치로 통합하던 기존 방식과 대조적이다. 현재의 DevOps 관행에는 아래 내용들이 포함된다.*  
\- *지속적 개발*  
*\- 지속적 테스팅*  
*\- 지속적 통합*  
*\- 지속적 배포*  
*\- 지속적 모니터링*

[RedHat 사의 CI/CD 관련 문서](https://www.redhat.com/ko/topics/devops/what-is-ci-cd?page=8)에서 관련 내용을 좀 더 찾아볼 수 있었고 여기서 Delevery 와 Deployment 의 차이를 알 수 있었다.

![CI/CD Flow](https://www.redhat.com/rhdc/managed-files/styles/wysiwyg_full_width/private/ci-cd-flow-desktop.png?itok=NNRD1Zj0 align="left")

지속적 통합(CI)는 코드 변경 사항을 공유 소스 코드 레포지토리에 자동으로 자주 통합하는 것.

지속적 전달(CD)는 자동 프로덕션을 포함하지 않고 코드 변경 사항의 통합, 테스트를 수행하는 것.

지속적 배포(CD)는 이렇게 개발된 소프트웨어를 배포하는 과정이 자동화 된 것.

# DevOps는 뭐야?

역시 위키를 보면,

> **DevOps** is a methodology in the software development and IT industry. Used as a set of practices and tools, DevOps integrates and automates the work of [software development](https://en.wikipedia.org/wiki/Software_development) (*Dev*) and [IT operations](https://en.wikipedia.org/wiki/IT_operations) (*Ops*) as a means for improving and shortening the [systems development life cycle](https://en.wikipedia.org/wiki/Systems_development_life_cycle).[<sup>[1]</sup>](https://en.wikipedia.org/wiki/DevOps#cite_note-1) DevOps is complementary to [agile software development](https://en.wikipedia.org/wiki/Agile_software_development); several DevOps aspects came from the *agile* way of working.
> 
> Automation is an important part of DevOps. [Software programmers](https://en.wikipedia.org/wiki/Software_programmer) and [architects](https://en.wikipedia.org/wiki/Software_architect) should use "[fitness functions](https://en.wikipedia.org/wiki/Fitness_function)" to keep their software in check.[<sup>[2]</sup>](https://en.wikipedia.org/wiki/DevOps#cite_note-2)

*DevOps는 소프트웨어 개발 및 IT 산업의 방법론이다. DevOps는 시스템 개발 수명 주기를 개선하고 단축하기 위한 수단으로 소프트웨어 개발(Dev) 및 IT 운뎡(Ops) 작업을 통합하고 자동화한다.*

# 정리해보자

* DevOps 는 개발과 과 운영 과정을 통합하고 자동화하는 방법론이다.
    
* CI/CD 는 현대 DevOps 관행의 중요한 일부로 개발에서 배포 단계로 이동하는 프로세스를 가속화한다.
    
* CI 는 여러 가지 작은 변경 사항을 기본 분기로 자주 병합하는 것을 말한다.
    
* CD 는 두 가지 의미로 사용되는데 Continuous delivery 의 경우 자동 프로덕션을 포함하지 않는 코드의 변경 사항 통합과 테스트를 말하며 Continuous Deployment 는 이렇게 개발된 소프트웨어를 배포하는 과정을 자동화 한 것을 말한다.
    

# 마무리

이번 포스팅에서는 CI/CD의 개념과 DevOps의 개념에 대해 알아봤다.

다음 포스팅에서는 이런 소프트웨어 개발 방법론을 안드로이드 분야에서 어떤 툴과 기술들을 사용해 수행하고 있는지 찾아보고 학습해보겠다.

---

# 참고

[https://en.wikipedia.org/wiki/CI/CD](https://en.wikipedia.org/wiki/CI/CD)

[https://www.redhat.com/ko/topics/devops/what-is-ci-cd?page=8](https://www.redhat.com/ko/topics/devops/what-is-ci-cd?page=8)