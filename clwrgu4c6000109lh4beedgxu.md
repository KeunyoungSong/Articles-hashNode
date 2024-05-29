---
title: "Functional programming이란?"
datePublished: Wed May 29 2024 06:49:06 GMT+0000 (Coordinated Universal Time)
cuid: clwrgu4c6000109lh4beedgxu
slug: functional-programming
tags: functional-programming

---

위키피디아를 보면,

> In computer science, **functional programming** is a programming paradigm where programs are constructed by applying and composing functions. It is a declarative programming paradigm in which function definitions are trees of expressions that map values to other values, rather than a sequence of imperative statements which update the running state of the program.

함수형 프로그래밍은 함수가 적용되고 함수로 구성하는 프로그래밍 패러다임이다. 
함수 정의가 값을 다른 값으로 매핑하는 표현식으로 구성된 트리구조인 선언적 프로그래밍 방식이다.

### 함수 정의가 트리 형태로 구성된다는 말의 의미

**표현식의 트리 구조**: 함수형 프로그래밍에서는 프로그램이 함수와 표현식들로 이루어져 있다. 이 함수와 표현식들은 서로 중첩되어 트리 형태를 이루게 된다. 예를 들어, 하나의 함수가 다른 여러 함수를 호출하고, 이 호출된 함수들이 다시 다른 함수를 호출하는 방식으로 표현식이 트리 형태로 확장된다.

**계산 흐름**: 이러한 트리 구조는 프로그램의 계산 흐름을 나타낸다. 각 노드는 함수 호출을, 각 가지는 함수 호출 간의 관계를 나타낸다. 즉, 최상위 노드에서 시작하여 각 가지를 따라가며 함수들이 호출되고 결과가 계산된다.

**값의 매핑**: 트리 구조 내의 각 표현식은 값을 다른 값으로 매핑한다. 최종적으로 트리의 루트에서 리프 노드까지 계산이 진행되며 최종 결과가 도출된다.

예를 들어,

f(x) = g(h(x)) + k(x)

여기서 `f`는 `g`와 `h` 그리고 `k` 함수를 이용하여 값을 계산합니다. 이를 트리 구조로 표현하면 다음과 같다.

```
     f
    / \
   +   k
  / \   \
 g   h   x
 |   |
 x   x
```

이 트리에서 `f`는 루트 노드이고, `g(h(x))`와 `k(x)`는 `f`의 자식 노드입니다. `g`,`h`와 `k`는 `x`를 입력으로 받아 계산되고, 그 결과가 `f`의 계산에 사용됩니다. 

수학과 프로그래밍의 긴밀한 연관관계가 보여 신기하다..

> Functional programming is sometimes treated as synonymous with purely functional programming, a subset of functional programming which treats all functions as deterministic mathematical functions, or pure functions. **When a pure function is called with some given arguments, it will always return the same result, and cannot be affected by any mutable state or other side effects.** This is in contrast with impure procedures, common in imperative programming, which can have side effects (such as modifying the program's state or taking input from a user). Proponents of purely functional programming claim that by restricting side effects, programs can have fewer bugs, be easier to debug and test, and be more suited to formal verification.

위 내용중 일부를 정리하면,

함수형 프로그래밍에서 함수는 일급 시민이다.

- 다른 데이터 유형(예: Int)과 마찬가지로 변수에 바인딩되고 매개변수에 전달될 수 있으며 다른 함수의 결과로 반할 할 수 있다.

이를 통해 작은 기능이 모듈식으로 결합되는 선언적(declarative)이고 구성가능한(composable) 스타일로 프로그램을 작성할 수 있다.

특정 인수를 사용하여 **순수 함수**를 호출하면 항상 동일한 결과를 반환하며 **변경 가능한 상태(mutable state)**나 **기타 부작용(side effects)**의 영향을 받지 않는다.

- **순수 함수**: 뒤에서 설명한다.

## 이런 프로그래밍 방식의 특징

- **선언형 접근**: 무엇을 할지를 설명하고, 어떻게 할지는 정의하지 않는다.
- **함수 중점**: 함수를 기본 단위로 하여 프로그램을 구성한다.
- **부수 효과 없음**: 함수가 입력 값을 받아 결과 값을 반환할 때, 함수 외부의 상태를 변경하지 않는다.
- **불변성**: 데이터는 변경되지 않고, 변경 시에는 새로운 데이터를 생성한다.



---

# 참고

https://en.wikipedia.org/wiki/Functional_programming