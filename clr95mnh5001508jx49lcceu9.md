---
title: "[Docs Review]State holders and UI State"
datePublished: Thu Jan 11 2024 11:56:59 GMT+0000 (Coordinated Universal Time)
cuid: clr95mnh5001508jx49lcceu9
slug: docs-reviewstate-holders-and-ui-state

---

The [UI layer guide](https://developer.android.com/topic/architecture/ui-layer) discusses unidirectional data flow (UDF) as a means of producing and managing the UI State for the UI layer.

> UI layer guide 에서 단방향 데이터 흐름을 UI layer 를 위한 UI State를 만들고 관리하는 수단으로설명했다

![](https://developer.android.com/static/images/topic/architecture/ui-layer/udf.png align="center")

**Figure 1**: Unidirectional data flow

It also highlights the benefits of delegating UDF management to a special class called a state holder. **You can implement a state holder either through** a ViewModel or **a plain class.** This document takes a closer look at state holders and the role they play in the UI layer.

> State holder 를 구현하기 위해선 ViewModel 이나 simple class 를 사용할 수 있다

At the end of this document, you should have an understanding of how to manage application state in the UI layer; that is the UI state production pipeline. You should be able to understand and know the following:

* UI layer 에 존재하는 UI state 유형에 대한 이해
    
* UI layer 에 존재하는 UI states 에서 동작하는 로직의 유형에 대한 이해
    
* 언제 ViewModel 과 simple class 중 어떤 것을 state holder 사용할 것인가에 대한 앎
    

# Elements of the UI state production pipeline

The UI state and the logic that produces it defines the UI layer.

> UI state 와 logic 이 UI layer 를 정의한다

## UI state

[UI state](https://developer.android.com/topic/architecture/ui-layer#define-ui-state) is the property that describes the UI.

> UI state 는 UI 를 설명하는 속성이다

There are two types of UI state:

* Screen UI state
    
* UI element state
    

> Screen UI state 는 screen을 그리기 위해 사용된다. 예로 `NewsUiState` 라는 클래스가 있는데 이 것이 news articles 정보를 가진다고 하자. 이 state holder 는 app 데이터를 포함하기 때문에 다른 계층 레이어들과 연결되야 한다
> 
> UI element state 는 UI elements 가 그려지는데 영향을 끼치는 고유 속성을 말한다

...

# References

[https://developer.android.com/topic/architecture/ui-layer/stateholders](https://developer.android.com/topic/architecture/ui-layer/stateholders)

---

*'Portions of this page are reproduced from work created and*[*shared by the Android Open Source Project*](http://code.google.com/policies.html)*and used according to terms described in the*[*Creative Commons 2.5 Attribution License*](http://creativecommons.org/licenses/by/2.5/)*.*