---
title: "ReactiveX란?"
datePublished: Wed May 29 2024 07:17:46 GMT+0000 (Coordinated Universal Time)
cuid: clwrhuzmr000j0amqegc58t2n
slug: reactivex
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1716967031010/3f334e4a-edc0-4929-9235-62a978c5f374.png
tags: reactivex

---

이전 포스팅 [Reactive Programming(반응형 프로그래밍)이란?](https://hashnode.com/post/clwrc4mid000308k0hs92erhl) 에서 Reactive, Declarative, Imperative Programming 방식에 대해서 알아봤다. 이번 포스팅에서는 ReactiveX에 대해서 알아본다.

### 공식 홈페이지를 보자

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1716958303253/8d85ba94-3845-4db8-82c0-bb439ce29b56.png align="center")

**"관측 가능한 스트림을 사용하는 비동기적 프로그래밍을 위한 API"라고 한다.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1716959364950/8c996d4e-d78e-4bb6-8d3d-e53244a1233d.png align="center")

ReactiveX 는 아래의 세 가지 프로그래밍 개념을 결합한 것이다. 관련 포스팅을 링크걸어뒀다.

* [Observer 패턴](https://hashnode.com/post/clwrf98yx000308l83642gqge)
    
* [Iterator 패턴](https://hashnode.com/post/clwrfamci000009mb3mvrc4yn)
    
* [함수형 프로그래밍](https://hashnode.com/post/clwrgu4c6000109lh4beedgxu)
    

아래는 ReactiveX가 지원하는 언어와 플랫폼 목록이다.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1716965946135/fd09e3a2-d310-4218-9063-365d0128e85f.png align="center")

굉장히 많은 언어를 지원하는데 나는 Kotlin으로 Android 앱을 개발하기 때문에 ReactiveX 의 RxJava, RxKotlin, RxAndroid를 사용한다.

관련 라이브러리로 retrofit2의 RxJava 어댑터를 사용해 retrofit 의 api 호출 결과를 Call 객체가 아닌 Observable 객체로 받을 수 있다.

# 주요 개념

* **Observable**: 데이터 스트림을 나타냄. Observable은 데이터를 발행하는 역할을 하며, Observer는 이를 Subscribe 할 수 있습니다.
    
* **Observer:** Observable에서 발행된 데이터를 수신하는 역할을 한다. Observer는 데이터 항목을 받을 때 호출되는 콜백 함수를 가지고 있다.
    
* **Operators**: Observable을 변환하거나 결합하는 함수이다. 예를 들어 `map`, `filter`, `merger`, `concat` 등이 있다. 이러한 연산자를 사용해 복잡한 데이터 흐름을 간단하게 구성할 수 있다.
    
* **Scheduler:** 코드가 실행되는 컨텍스트를 제거한다. 비동기 연산의 실행 스케줄을 관리한다. 예를 들어 특정 스레드에서 실행하거나, 타이머를 사용하여 지연된 실행을 설정할 수 있다.
    

### 사용 예시

다음은 JavaScript와 RxJS를 사용한 간단한 예제이다.

```javascript
const { of } = require('rxjs');
const { map } = require('rxjs/operators');

const observable = of(1, 2, 3).pipe(
  map(x => x * 2)
);

observable.subscribe(x => console.log(x));
```

이 예제에서는 `of` 연산자를 사용하여 `1, 2, 3`을 발행하는 Observable을 생성하고, `map` 연산자를 사용하여 각 값을 2배로 변환한다. 그리고 최종 결과를 `subscribe` 함수를 통해 콘솔에 출력한다.

### ReactiveX의 장점

1. **가독성**: 데이터 흐름과 변환을 선언적으로 작성할 수 있어 코드의 가독성이 높아집니다.
    
2. **유연성**: 다양한 연산자를 사용하여 복잡한 비동기 시나리오를 쉽게 구현할 수 있습니다.
    
3. **에러 처리**: 데이터 스트림 내에서 발생하는 에러를 쉽게 처리할 수 있습니다.
    
4. **일관성**: 동기 및 비동기 코드 모두 동일한 패턴으로 처리할 수 있어 일관성이 있습니다.
    

여기까지 ReactiveX에 대해 알아보았다. 다음 포스팅에서는 ReactiveX를 활용한 프로젝트를 구현하고 사용된 RX 기능을 위주로 리뷰하는 포스팅을 하겠다.

---

# 참고

[https://reactivex.io/](https://reactivex.io/)