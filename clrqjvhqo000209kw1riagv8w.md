---
title: "[Docs Reviews]Guide to app architecture"
datePublished: Tue Jan 23 2024 16:07:51 GMT+0000 (Coordinated Universal Time)
cuid: clrqjvhqo000209kw1riagv8w
slug: docs-reviewsguide-to-app-architecture

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