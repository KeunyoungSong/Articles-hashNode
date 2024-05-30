---
title: "Reactive Programming(반응형 프로그래밍)이란?"
datePublished: Wed May 29 2024 04:37:18 GMT+0000 (Coordinated Universal Time)
cuid: clwrc4mid000308k0hs92erhl
slug: what-is-reactive-programming
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1716957507414/c048db72-aa32-4485-a382-7e77add31997.png
tags: reactive-programming, declarative-programming, imperative-programming

---

위키피디아에 따르면,

> In computing, **<mark>reactive programming</mark>** <mark> is a declarative programming paradigm concerned with data streams and the propagation of change.</mark> With this paradigm, it is possible to express static (e.g., arrays) or dynamic (e.g., event emitters) data streams with ease, and also communicate that an inferred dependency within the associated execution model exists, which facilitates the automatic propagation of the changed data flow.
> 
> For example, in an **imperative programming** setting, a := b + c would mean that a is being assigned the result of b + c in the instant the expression is evaluated, and later, the values of b and c can be changed with no effect on the value of a. On the other hand, in **reactive programming**, the value of a is automatically updated whenever the values of b or c change, without the program having to explicitly re-state the statement a := b + c to re-assign the value of a.

내용을 정리하자면,

반응형 프로그래밍은 데이터 스트림과 변화의 전파에 관한 **선언적프로그래밍** 패러다임이다.

이전의 **명령형프로그래밍** 방식에서는 a 라는 값을 얻기 위해 b와 c의 값이 필요하고 이를 연산의 결과로 평가되는 값을 할당(대입) 받았다. 하지만 **반응형 프로그래밍에서는 b와 c값이 변경될 때마다 자동으로 업데이트된 a 값을 얻을 수 있다.**

더욱 확실히 이해하기 위해 참조된 다른 용어들의 개념을 보자.

### Declarative Programming, Imperative Programming이란?

#### Declarative Programming(선언적 프로그래밍)

> In computer science, **declarativeprogramming** is a programming paradigm—a style of building the structure and elements of computer programs—that expresses the logic of a computation without describing its control flow.
> 
> <mark>Declarative programming is a non-imperative style of programming in which programs describe their desired results without explicitly listing commands or steps that must be performed.</mark>

제어 흐름을 설명하지 않고 계산 논리를 표현하는 프로그래밍 패러다임.  
컴퓨터 프로그램의 구조와 요소를 구축하는 스타일.  
수행해야 하는 명령이나 **단계를 명식적으로 나열하지 않고프로그램이 원하는 결과를 설명**하는 비명령형 프로그래밍 스타일.

이와 대비되는 것이

#### Imperative Programming(명령적 프로그래밍)

> In computer science, **<mark>imperative programming</mark>** <mark> is a programming paradigm of software that uses statements that change a program's state. </mark> In much the same way that the imperative mood in natural languages expresses commands, an imperative program consists of commands for the computer to perform. <mark>Imperative programming focuses on describing how a program operates step by step, rather than on high-level descriptions of its expected results.</mark>

프로그램의 상태를 변경하는 명령문을 사용하는 프로그래밍 방식.  
**명령적 프로그래밍**은 프로그램의 결과에 대한 높은 수준의 설명보다는 **프로그램이 작동되는 방식을 단계별로 하나 하나 기술하는데 중점을 둔다.**

정리하자면,

**Declarative Programming(선언적 프로그래밍)**은 프로그램이 결과로 원하는 결과를 설명하는 프로그래밍 하고 \*\*Imperative Programming(명령적 프로그래밍)\*\*은 프로그램이 작동되는 방식 그 과정을 구체적으로 설명한다.

그리고 **Reactive Programming은 이런 선언적 프로그래밍 방식을 사용하고 있다.**

#### 선언적 프로그래밍의 특징과 예시

선언적 프로그래밍의 특정은

* **명확한 목표**: 프로그램이 달성하려는 목표나 결과를 설명한다
    
* **추상화된 절차**: 내부적으로 구체적인 절차나 단계는 추상화된다
    
* **상태 변경 최소화**: 상태 변경을 명시적으로 다루지 않고, 데이터 흐름과 그 변화를 기술한다
    

선언적 프로그래밍의 예로는

* **SQL**: 데이터베이스에서 데이터를 조회할 때 `SELECT * FROM users WHERE age > 30`이라고 명시하면, SQL 엔진이 어떻게 데이터를 검색할지는 내부적으로 처리. 사용자는 결과적으로 "30세 이상의 사용자들을 가져온다"는 목표만 명시하면 된다.
    
* **HTML**: "이 텍스트를 제목으로 표시한다"는 목표를 명시하며, 브라우저가 이를 어떻게 렌더링할지는 내부적으로 처리한다.
    

반응형 프로그래밍, 명령형 프로그래밍 그리고 선언적 프로그래밍에 대해서 알아봤다.

### 번역에서 각각의 동작원리가 직관적으로 다가오지 않는데...

**Declarative**에는 "서술문의", "평서문의"이란 의미가 있어 **"원하는 결과를 얻기 위한 필요한 과정을 서술한다"**, **Imperative**에는 "원칙", "반드시 해야 하는", "필수적인"이란 의미가 있어 "**필요한모든 원칙을 하나하나 기술한다"** 라고 생각하면 될 것 같다.

그럼 **서술적 프로그래밍**, **원칙적 프로그래밍**이 되겠다.

# Reactive Programming(반응형 프로그래밍)의 이해

반응형 프로그래밍은 데이터 흐름과 변화를 쉽게 조작할 수 있는 선언적 프로그래밍 패러다임이다. 비동기 데이터 스트림을 사용하여 이벤트와 데이터를 처리한다.

### 반응형 프로그램의 필수 요소

* **데이터 스트림**: 시간이 지남에 따라 발생하는 값들의 시퀀스, 이벤트, 네트워크 응답, 사용자 입력 등을 스트림으로 처리할 수 있다
    
* **옵저버블 패턴**: 데이터 소스(Observable)와 이를 구독(Subscribe)하는 옵저버(Observer) 간의 관계의 정의
    
* **스트림 연산자**: 스트림을 변환하거나 결합하는 함수들. 예: `map`, `filter` 등
    

여기까지 반응형 프로그래밍과 관련된 프로그래밍 패러다임에 대해 알아봤다. 다음 포스팅에서는 반응형 프로그래밍을 기반으로 구현된 비동기 프로그래밍을 위한 ReactiveX 라이브러리에 대해 알아보겠다.

---

# 참고

[https://en.wikipedia.org/wiki/Reactive\_programming](https://en.wikipedia.org/wiki/Reactive_programming)

[https://en.wikipedia.org/wiki/Declarative\_programming](https://en.wikipedia.org/wiki/Declarative_programming)

[https://en.wikipedia.org/wiki/Imperative\_programming](https://en.wikipedia.org/wiki/Imperative_programming)