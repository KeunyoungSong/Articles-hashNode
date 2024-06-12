---
title: "[Doc Review]안드로이드에서 멀티 모듈이란?"
datePublished: Wed Jun 12 2024 07:49:06 GMT+0000 (Coordinated Universal Time)
cuid: clxbj579n000y09jy0yxhhs9j
slug: doc-review
tags: android

---

안드로이드 관련 기술을 학습하던 중 멀티 모듈 앱 이라는 키워드를 알게 되었다. 이번 포스팅에서는 안드로이드 필드에서 멀티 모듈이 무엇을 말하는지와 왜 필요한 것인지 알아보겠다.

### 안드로이드 공식 문서를 보자

[**Guide to Android app modularization**](https://developer.android.com/topic/modularization) **을 보자.**

> **A project with multiple Gradle modules is known as a multi-module project.** This guide encompasses best practices and recommended patterns for developing multi-module Android apps.

**여러개의 Gradle 모듈을 가지고 있는 프로젝트를 멀티 모듈 프로젝트라고 한다**.

끊임 없이 성장하는 코드베이스에서는 확장성, 가독성, 및 전반적인 코드 품질이 시간이 지남에 따라 저하되는데 이런 문제를 방지하고 유지관리성을 향상시키는 방식으로 코드베이스를 구조화하는 수단이 모듈레이션이라고 한다.

그럼 modularization은 뭘까? 문서를 보면,

> Modularization is a practice of organizing a codebase into loosely coupled and self contained parts. Each part is a module. Each module is independent and serves a clear purpose. By dividing a problem into smaller and easier to solve subproblems, you reduce the complexity of designing and maintaining a large system.

모듈레이션은 코드베이스를 느슨하게 결합하고 자족적인 부분으로 구성하는 방법이다. 각 부분을 모듈이라고 하며 각 모듈은 독립적으로 명확한 목적을 제공한다고 한다. 문제를 작고 해결하기 쉬운 하위 문제로 나누면 대규모 시스템을 설계하고 유지 관리하는 복잡성이 줄어든다.

![](https://developer.android.com/static/topic/modularization/images/1_sample_dep_graph.png align="left")

위 사진은 구글 문서에 나온 샘플 다중 모듈 코드 베이스의 종속성 그래프이다.

### 모듈레이션의 장점

문서를 보면 이런 모듈레이션 사용의 장점으로

* Reusability(재사용성)
    
* Strict visibility control(엄격한 가시성 제어)
    
* Customizable delivery
    

> Customizable delivery를 사용하면 앱이 데모인지 아닌지에 따라 제한적 기능 제공이 가능하다고 한다.

이런 모듈레이션의 사용에 더해 다른 기술들을 활용하면 아래의 내용을들 더욱 효과적으로 달성할 수 있다고 한다.

* Scalability: 잘 모듈화된 프로젝트는 SOC(Separation of Concerns)를 포괄한다.
    
* Ownership: 모듈별 관리자를 두어 소유권 관리를 할 수 있습니다.
    
* Encapsulation: 캡슐화는 코드의 각 부분이 다른 부분에 대해 최소한의 지식을 가져아 함을 의미합니다. 이렇게 고립된 코드는 더 읽고 이해하기 쉽습니다.
    
* Testability: 단독으로 테스트 가능하기 쉬운 코드를 만듭니다.
    
* Build time: Incremental build, build cache, parallel build 를 활용해 빌드 성능을 향상 시킬 수 있다.
    

### 정리해보자

1. 안드로이드 필드에서는 여러개의 Gradle 모듈을 가지고 있는 프로젝트가 멀티 모듈 프로젝트이다.
    
2. 모듈레이션이란 코드를 자족 가능하게 느슨한 결합을 가진 형태로 구성하는 방법이다.
    
3. 일반적으로 코드 규모가 커지면서 확장성, 가독성 등 전반적 품질 저하가 오기 마련이지만 이를 적절한 모듈레이션을 활용해 방지하고 유지보수성을 개선할 수 있다.
    

이렇게 멀티 모듈의 개념에 대해서 알아봤다.

실제 구현 방법은 문서에서 제공하는 샘플 코드를 아래서 확인하자.

[https://github.com/android/architecture-samples/tree/multimodule](https://github.com/android/architecture-samples/tree/multimodule)

# 참고

[https://developer.android.com/topic/modularization](https://developer.android.com/topic/modularization)

[https://github.com/android/architecture-samples/tree/multimodule](https://github.com/android/architecture-samples/tree/multimodule)