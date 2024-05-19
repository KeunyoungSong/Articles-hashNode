---
title: "[Docs Reviews]Guide to app architecture"
datePublished: Tue Jan 23 2024 16:07:51 GMT+0000 (Coordinated Universal Time)
cuid: clrqjvhqo000209kw1riagv8w
slug: docs-reviewsguide-to-app-architecture
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1716101678147/790b10cf-7c33-4137-be49-f625db24db06.png

---

This guide encompasses best practices and [recommended architecture for building robust, hi](https://developer.android.com/topic/architecture#recommended-app-arch)gh-quality apps.

## Mobile app user experiences

A typical Android app contains multiple [app components](https://developer.android.com/guide/components/fundamentals#components), including [activities](https://developer.android.com/guide/components/activities/intro-activities), [fragments](https://developer.android.com/guide/fragments), [services](https://developer.android.com/guide/components/services), [content providers](https://developer.android.com/guide/topics/providers/content-providers), and [broadcast receivers](https://developer.android.com/guide/components/broadcasts).

You declare most of these app components in your [app manifest](https://developer.android.com/guide/topics/manifest/manifest-intro). The Android OS then uses this file to decide how to integrate your app into the device's overall user experience.

> Since the mobile devices are resource-constrained, it's possible for your app components to be launched out-of-order and can be destroyed at any time. Because these events aren't under your control.
> 
> 앱은 항상 자원의 제약을 받으며, 이런 상황에서 일어나는 앱의 파괴와 같은 이벤트 들은 언제나 일어날 수 있다. <mark>그렇기 때문에 어떤 앱 데이터도 메모리나 앱 컴포넌트에 저장하지 말아라.</mark>

## Common architectural principles(설계원칙)

## 1.Separation of concerns(관심분리)

> Activity 나 Fragment 를 최대한 lean하게 유지하라. 그럼으로써 라이프사이클과 관련된 문제를 피하고 테스트를 용이하게 할 수 있다.

Keep in mind that you don't own implementations of `Activity` and `Fragment`; rather, these are just glue classes that represent the contract between the Android OS and your app.

> Activity 와 Fragment 의 구현을 own하고 있다고 생각하지 말고, 각각을 앱에서 Android OS를 사용하기 위한 계약서라고 생각해라.

## 2.Drive UI from data models(데이터 모델에서 UI를 제어하라)

> data model 이 영구적이면 더 좋다

Persistent models are ideal for the following reasons:

* 앱이 파괴되어도 데이터를 잃지 않는다
    
* 인터넷 연결 상태가 좋지 않거나 사용이 불가능할 때도 앱이 사용 가능하다
    

## 3.Single source of truth(단일 정보 출처)

When a new data type is defined in your app, you should assign a Single Source of Truth (SSOT) to it. **The SSOT is the *owner* of that data, and only the SSOT can modify or mutate it.**

This pattern brings multiple benefits:

* 어떤 범주에 있는 모든 데이터 변화를 한곳에 둘 수 있다.
    
* 다른 변조의 간섭 시도를 사전에 막는다.
    
* 데이터를 추적하기 용이하고, 버그를 찾기가 쉬워진다.
    

## 4.Unidirectional Data Flow(단방향 데이터 흐름)

The single source of truth principle is often used in our guides with the Unidirectional Data Flow (UDF) pattern. **In UDF, state flows in only one direction. The events that modify the data flow in the opposite direction.**

안드로이드에서는 일반적으로 상태나 데이터가 계층의 상위 범위 유형에서 하위 범위 유형으로 흐릅니다. 이벤트는 일반적으로 해당 데이터 유형의 SSOT(Single Source of Truth)에 도달할 때까지 하위 범위 유형에서 트리거됩니다.

> 앱 데이터는 데이터 출처로 부터 UI로 흐릅니다. 사용자 이벤트는 버튼 누름과 같은 경우에 UI에서 시작하여 응용 프로그램 데이터가 수정되고 불변 유형으로 노출되는 SSOT로 흐릅니다.
> 
> 이러한 패턴은 데이터 일관성을 더 확실히 보장하며, 에러에 영향을 덜 받고, 디버깅이 더 쉽고, SSOT 패턴의 모든 이점을 가져옵니다.

## Recommended app architecture

> 이전에 언급된 설계원칙을 고려하면 각 앱은 최소 두개의 레이어를 가져야 한다.

* The UI layer
    
* The data Layer
    

You can add an additional layer called the *domain layer* to simplify and reuse the interactions between the UI and data layers.

> UI 와 data layer 사이의 재사용을 단순화 하기 위해 domain layer 를 사용할 수 있다

![Figure 1. Diagram of a typical app architecture.](https://developer.android.com/static/topic/libraries/architecture/images/mad-arch-overview.png align="center")

**Figure 1.** Diagram of a typical app architecture.

Note: The arrows in the diagrams in this guide represent dependencies between classes. For example, the domain layer depends on data layer classes.

## Modern App Architecture

This *Modern App Architecture* encourages using the following techniques, among others:

* 반응형 및 계층형 구조
    
* 모든 레이어에 걸친 단방향 데이터 흐름
    
* State holder 를 가진 UI 레이어
    
* 코루틴과 플로우
    
* 의존성 주입의 좋은 실례
    

## UI layer

The role of the UI layer (or *presentation layer*) is to display the application data on the screen.

The UI layer is made up of two things:

* UI 요소
    
* State holders(such as ViewModel classes)
    

![](https://developer.android.com/static/topic/libraries/architecture/images/mad-arch-overview-ui.png align="center")

**Figure 2.** The UI layer's role in app architecture.

## Data layser

The data layer of an app contains the *business logic*. **The business logic is what gives value to your app**—it's made of rules that determine how your app creates, stores, and changes data.

The data layer is made of *repositories* that each can contain zero to many *data sources*. You should create a repository class for each different type of data you handle in your app. For example, you might create a `MoviesRepository` class for data related to movies, or a `PaymentsRepository` class for data related to payments.

> 데이터 레이어는 레포지토리로 구성된다. 앱에서 사용하는 모든 데이터 타입에 대해 레포지토리를 생성해야 한다.

![](https://developer.android.com/static/topic/libraries/architecture/images/mad-arch-overview-data.png align="center")

**Figure 3.** The data layer's role in app architecture.

Repository classes are responsible for the following tasks:

* 앱에 데이터 노출시키기
    
* 데이터 변화를 한군데 두기
    
* 여러 데이터 출처로부터의 충돌 해결하기
    
* 데이터의 출처를 다른 곳에서 추상화하기
    
* 비즈니스 로직 보유하기
    

**Each data source class should have the responsibility of working with only one source of data, which can be a file, a network source, or a local database.**

Data source classes are the bridge between the application and the system for data operations.

> 각 데이터 출처 클래스는 하나의 데이터 출처와만 작동해야 하며, 각 출처는 파일, 네트워크 출처, 로컬 데이터베이스가 될 수 있다

## Domain layer

The domain layer is an optional layer that sits between the UI and data layers.

**The domain layer is responsible for encapsulating complex business logic, or simple business logic that is reused by multiple ViewModels.** This layer is optional because not all apps will have these requirements. You should use it only when needed—for example, to handle complexity or favor reusability.

> 도메인 레이어는 복잡한 비즈니스 로직을 숨기고 간단하지만 여러 ViewModel에서 사용되는 비즈니스 로직에 대해 처리하는 책임을 갖는다.

![](https://developer.android.com/static/topic/libraries/architecture/images/mad-arch-overview-domain.png align="center")

**Figure 4.** The domain layer's role in app architecture.

Classes in this layer are commonly called ***usecases*** or ***interactors***. Each use case should have responsibility over a *single* functionality. For example, your app could have a `GetTimeZoneUseCase` class if multiple ViewModels rely on time zones to display the proper message on the screen.

> 도메인 레이어의 클래스들은 Usecase 나 Interactor 라고 불린다.  
> 하나의 유즈케이스는 하나의 기능(목적)을 가져야 한다.
> 
> 다양한 ViewModel 에서 사용하는 비즈니스 로직을 재활용하는 예로 `GetTimeZoneUseCase` 이 있고 이런 기능들을 도메인 레이어에 작성할 수 있다

# Manage dependencies between components

Classes in your app depend on other classes in order to function properly. You can use either of the following design patterns to gather the dependencies of a particular class:

* [Dependency injection (DI)](https://developer.android.com/training/dependency-injection): Dependency injection allows classes to define their dependencies without constructing them. At runtime, another class is responsible for providing these dependencies.
    
* [Service locator](https://en.wikipedia.org/wiki/Service_locator_pattern): The service locator pattern provides a registry where classes can obtain their dependencies instead of constructing them.
    

> 컴포넌트 간의 의존성을 관리하기 위해 의존성 주입과 Service Locator를 사용 할 수 있다.

**We recommend following dependency injection patterns and using the**[**Hilt library in Android**](https://developer.android.com/training/dependency-injection/hilt-android)**apps.** Hilt automatically constructs objects by walking the dependency tree, provides compile-time guarantees on dependencies, and creates dependency containers for Android framework classes.

## General best practices

### 1.Don't store data in app components.

Avoid designating your app's entry points—such as activities, services, and broadcast receivers—as sources of data. Instead, they should only coordinate with other components to retrieve the subset of data that is relevant to that entry point. Each app component is rather short-lived, depending on the user's interaction with their device and the overall current health of the system.

> 앱의 Activity, Service, Broadcast receiver 등을 데이터 소스로 사용하지 말아라.  
> 각각은 데이터 요청이나 처리를 다른 모듈 또는 클래스에 위임하고, 그 결과를 받아올 수 있도록 설계되어야 한다.
> 
> 각 컴포넌트들은 차라리 사용자와의 상호작용에 따라 짧은 주기를 갖는 것이 낫다.

### 2.Reduce dependencies on Android classes.

Your app components should be the only classes that rely on Android framework SDK APIs such as Context, or Toast. Abstracting other classes in your app away from them helps with testability and reduces coupling within your app.

> 클래스들을 Android framework 에 너무 의존하게 하지마라.

### 3\. **Create well-defined boundaries of responsibility between various modules in your app.**

For example, don't spread the code that loads data from the network across multiple classes or packages in your code base. Similarly, don't define multiple unrelated responsibilities—such as data caching and data binding—in the same class. Following the [recommended app architecture](https://developer.android.com/topic/architecture#recommended-app-arch) will help you with this.

> 네트워크로부터 데이터를 가져오는 것을 여러 클래스에 걸쳐서 하지 말아라.  
> 하나의 클래스에 서로 관련 없는 여러 책임을 두지말아라

### 4.**Expose as little as possible from each module.**

For example, don't be tempted to create a shortcut that exposes an internal implementation detail from a module. You might gain a bit of time in the short term, but you are then likely to incur technical debt many times over as your codebase evolves.

> 데이터를 외부로 노출할 때 mutable 하게 전달하지 말아라

### 5\. **Focus on the unique core of your app so it stands out from other apps.**

### 6.**Consider how to make each part of your app testable in isolation.**

For example, having a well-defined API for fetching data from the network makes it easier to test the module that persists that data in a local database. If instead, you mix the logic from these two modules in one place, or distribute your networking code across your entire code base, it becomes much more difficult—if not impossible—to test effectively.

> API 호출로부터 데이터를 가져오는 로직과 로컬에서 관리하는 로직을 분리하면 각 모듈의 테스트가 쉬워진다

### 7.**Types are responsible for their concurrency policy.**

If a type is performing long-running blocking work, it should be responsible for moving that computation to the right thread. That particular type knows the type of computation that it is doing and in which thread it should be executed. Types should be main-safe, meaning they're safe to call from the main thread without blocking it.

> 코루틴 써라.

### 8.**Persist as much relevant and fresh data as possible.**

That way, users can enjoy your app's functionality even when their device is in offline mode. Remember that not all of your users enjoy constant, high-speed connectivity—and even if they do, they can get bad reception in crowded places.

> 가능한 한 많은 관련 및 최신 데이터를 지속적으로 저장해라.

## Benefits of Architecture

Having a good Architecture implemented in your app brings a lot of benefits to the project and engineering teams:

* 앱 전반의 유지보수성, 품질, 견고함 향상
    
* 확장 가능성
    
* 아키텍처는 프로젝트에 일관성을 가져오기에, 새로운 팀 멤버의 합류와 적응을 쉽게 한다.
    
* 테스트를 쉽게한다.
    
* 버그가 체계적으로 조사될 수 있다
    

# References

[https://developer.android.com/topic/architecture](https://developer.android.com/topic/architecture)