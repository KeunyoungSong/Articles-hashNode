---
title: "Server-Driven UI App"
datePublished: Wed May 29 2024 02:36:42 GMT+0000 (Coordinated Universal Time)
cuid: clwr7tj3100050ajy61xe0oib
slug: server-driven-ui-app
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1716957820058/95026e86-ab52-40f4-834e-5d36b336adcb.png

---

# Overview

이전 포스팅에서 다룬 개념을 실제 프로젝트에 적용해보고 관련 기술과 구현 경험을 쌓기 위한 토이 프로젝트입니다.

* [서버 주도 UI(Server-Driven UI)란?](https://hashnode.com/post/clwojky1s000i09k23ptfhxc7)
    
* [직렬화와 비직렬화: Serializable, Parcelable 그리고Gson의 이해](https://hashnode.com/post/clwoo94y9000109l9hticfezh)
    

## What I learned

* **Server-Driven UI (서버 주도 UI)**: 서버에서 UI 레이아웃과 데이터를 받아와서 클라이언트에서 렌더링하는 방식. 이를 통해 앱 업데이트 없이도 UI를 변경할 수 있습니다.
    
* **Custom JsonDeserializer (커스텀 JsonDeserializer)**: 커스텀 JsonDeserializer를 구현하여 다양한 ListItem 타입을 적절하게 직렬화 및 비직렬화 하는 방법을 배웠습니다. 이를 통해 서버에서 받은 JSON 데이터를 객체로 변환하여 활용할 수 있습니다.
    
* **View Holder Creation Pattern (뷰 홀더 생성 패턴)**: ViewHolderGenerator를 사용하여 다양한 ViewHolder를 생성하고 재사용하는 패턴을 익혔습니다.
    
* **Dynamic View Binding Creation (뷰 바인딩 동적 생성)**: 뷰 바인딩을 동적으로 생성하는 방법을 배웠습니다. 이를 통해 다양한 레이아웃에 대해 동적으로 바인딩을 생성하고 사용할 수 있습니다.
    
* **Reflection (리플렉션)**: DataBindingUtil 내 static 으로 선언된 inflate 함수를 호출하기 위해 ViewGroup.toBinding() 함수에서 사용되었습니다. 리플렉션을 사용하여 클래스의 메서드를 동적으로 호출하고 객체를 생성하는 방법을 익혔습니다. 이를 통해 런타임에 동적으로 타입에 접근하고 조작할 수 있습니다.
    
* **Generics (제네릭)**: 다양한 타입의 뷰 바인딩을 처리하기 위해 ViewGroup.toBinding() 함수에서 사용되었습니다. 제네릭 타입 파라미터 V를 사용하여 함수가 다양한 타입의 ViewBinding을 반환할 수 있도록 합니다.
    
* **Reified Type Parameters (실체화된 타입 파라미터)**: ViewGroup.toBinding()에서 반환타입을 ViewBinding의 자식 제네릭으로 사용하는데 이를 받는 뷰홀더는 ViewDataBinding 타입을 받는다. 이때 제네릭 타입 파라미터를 Reified 를 통해 실체화하여 런타임에 타입 정보에 접근해 DataBindingUtil 내 inflate 함수를 호출한다.(ViewDataBinding은 ViewBinding 을 구현함)
    
* **Hilt (의존성 주입)**: Hilt를 사용하여 의존성 주입을 구현하는 방법을 배웠습니다.
    

## Key Functions

## GitHub link