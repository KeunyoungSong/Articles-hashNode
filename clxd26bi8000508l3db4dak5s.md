---
title: "위임 패턴(Delegation Pattern)의 이해"
datePublished: Thu Jun 13 2024 09:29:37 GMT+0000 (Coordinated Universal Time)
cuid: clxd26bi8000508l3db4dak5s
slug: delegation-pattern
tags: compose, android, kotlin, delegate

---

이전 안드로이드 공부를 하던 중 by 키워드를 사용하는 예제를 본 적이 있었다.

검색을 해보니 위임을 위한 키워드라는 것을 알 수 있었다. 그래서 코틀린 공식 문서를 찾아 읽어 얕은 이해를 하려고 했었다. 하지만 실제로 위임을 사용한 구현을 해본적이 없었기 때문에 좀 더 깊은 이해를 하긴 어려웠고 제공되는 라이브러리를 사용하기 위한 정도로 사용해왔다.

컴포즈를 공부하며 위임을 많이 사용하는 것을 볼 수 있었고 이 지점이 위임에 대해 짚고 넘어가는데 좋은 기회가 될 것이라고 생각했다.

그래서 오늘은 이전 읽어봤던 문서를 다시 찾아 기억을 되짚어보고 안드로이드에서 위임을 어떻게 사용하는지 파악해 볼 생각이다. 간단한 위임을 사용한 구현도 실습해봐야겠다.

그럼 시작해보자.

# Delegation 이란?

한글로는 "위임"이란 뜻을 가졌다. 코틀린에서는 이를 뭐라하고 있는지 살펴봤다.

코틀린 공식 문서의 [Delegation](https://kotlinlang.org/docs/delegation.html) 페이지에서는 "위임"에 대한 정의는 위키피디아 링크를 걸어두고 간단한 실습을 제공하고 있다. 우선 위키피디아에서 개념을 파악한 뒤 간단한 실습을 학습하러 돌아오겠다.

> "In software engineering, the **delegation pattern** is an object-oriented design pattern that allows object composition to achieve the same code reuse as inheritance."

*소프트웨어 엔지니어링에서****위임 패턴****은 객체 지향 디자인 패턴으로, 객체 구성이 상속과 동일한 코드 재사용을 달성할 수 있게 합니다.*

위임이 상속과 같이 코드 재사용을 가능하게 한다면 이미 있는 상속을 두고 왜 이런 개념이 필요한 것인지 또 어떤 특정 맥락에서 유용할지를 알아봐야겠다.

다시 코틀린 문서로 돌아와서, 간단한 코드 스니펫을 통해 위임에 대해 알아보자.

> The Delegation pattern has proven to be a good alternative to implementation inheritance, and Kotlin supports it natively requiring zero boilerplate code.

코틀린에서는 위임에 대해 기본적으로 상용구 코드 없이 이를 지원한다고 한다.

A class `Derived` can implement an interface `Base` by delegating all of its public members to a specified object:

```kotlin
interface Base {
    fun print()
}

class BaseImpl(val x: Int) : Base {
    override fun print() { print(x) }
}

class Derived(b: Base) : Base by b

fun main() {
    val base = BaseImpl(10)
    Derived(base).print()  // 10
}
```

위 코드에서는 Derived class 가 Base interface 를 <s>상속받고</s> 위임받고 있으며 매개변수로 Base 인스턴스를 받아 그 객체에 Base interface의 구현을 의존하고 있다.

코드에서는 Derived class 가 BaseImpl 의 인스턴스를 받아 구현을 의존하고 있으니 생성된 Derived 인스턴스를 참조하여 Base interface의 기능을 호출하면 BaseImpl 에 구현된 함수가 호출된다.

다음 코드를 보자.

```kotlin
interface Base {
    fun printMessage()
    fun printMessageLine()
}

class BaseImpl(val x: Int) : Base {
    override fun printMessage() { println(x) }
    override fun printMessageLine() { println(x) }
}

class Derived(b: Base) : Base by b {
    override fun printMessage() { println("abc") }
}

fun main() {
    val base = BaseImpl(10)
    Derived(base).printMessage()  // abc
    Derived(base).printMessageLine() // 10
}
```

위 코드에서도 Derived 가 BaseImpl 을 통해 Base 의 구현을 의존하고 있으나 printMessage() 메서드는 override 를 통해 Derived 내에서 재 구현했다.

결과를 보면 Derived는 기본적으로 모든 구현을 BaseImpl 로부터 위임받고 있으나 override 하고 있는 내용에 대해서는 Derived 내부에서 작성된 로직을 따랐다.

하지만 이런 구현에 주의해야 할 점이 하나 있다. 다음 코드를 보자

```kotlin
interface Base {
    val message: String
    fun print()
}

class BaseImpl(x: Int) : Base {
    override val message = "BaseImpl: x = $x"
    override fun print() { println(message) }
}

class Derived(b: Base) : Base by b {
    // This property is not accessed from b's implementation of `print`
    override val message = "Message of Derived"
}

fun main() {
    val b = BaseImpl(10)
    val derived = Derived(b)
    derived.print()  // BaseImpl: x = 10
    println(derived.message)  // Message of Derived
}
```

위 코드 또한 Derived class 가 BaseImpl 로부터 Base interface의 구현을 위임받아 그 구현을 처리하고있다. 하지만 message 를 Derived 내에서 override 했고, BaseImpl 에서 구현된 print() 에서는 Base interface의 message 프로퍼티를 참조하고 있다.

생각해보면 message 를 재작성했으니 print() 는 이를 참조하여 "Message of Derived" 를 출력해야 할 것 같지만 예상과 다르게 BaseImpl 에 작성된 message 를 출력하는 것을 볼 수 있다.

이는 코틀린이 기본적으로 제공하는 언어 수준에서의 위임이 내부적으로 어떻게 이뤄지는 알아야 한다.

# 위임은 어떻게 구현되는가?

기본적으로 위임은 특정 기능의 구현을 다른 구현체에게 의존하는데 있다.

위키피디아의 위임 페이지의 예제 코드를 보자.

```kotlin
class Rectangle(val width: Int, val height: Int) {
    fun area() = width * height
}

class Window(val bounds: Rectangle) {
    // Delegation
    fun area() = bounds.area()
}
```

Window 에서는 area() 메서드의 기능을 Rectangle 의 인스턴스에게 모두 의존하고 있다.

전달 받은 인스턴스를 참조하여 기능을 구현하고 있기 때문에 이전 예제에서 print() 를 호출했을 때 원래 message가 출력된 것이다.

아래는 언어 수준에서 지원된 코틀린의 by 를 활용한 문법이다.

```kotlin
interface ClosedShape {
    fun area(): Int
}

class Rectangle(val width: Int, val height: Int) : ClosedShape {
    override fun area() = width * height
}

// The ClosedShape implementation of Window delegates to that of the Rectangle that is bounds
class Window(private val bounds: Rectangle) : ClosedShape by bounds
```

Window 에서도 역시 area()를 호출할 수 있지만 bounds를 통해 전달된 Rectangle 의 인스턴스를 참조하여 구현된다.

# 중간 정리

### 위임(Delegation)이란?

객체지향 프로그래밍에서 객체가 특정 작업을 수행할 때, 자신이 직접 처리하지 않고 다른 객체(위임 객체)에게 그 작업을 맡기는 디자인 패턴.

### 위임 vs 상속

| 항목 | 위임(Delegation) | 상속(Inheritance) |
| --- | --- | --- |
| 기본 개념 | 한 객체가 특정 작업을 다른 객체에 위임함 | 한 클래스가 다른 클래스의 특성과 행동을 물려받음 |
| 구현 방법 | 위임 객체를 클래스 내부에서 사용 | 클래스 정의 시 `extends` 또는 `implements` 키워드 사용 |
| 유연성 | 더 유연하며 런타임에 동적으로 결정 가능 | 컴파일 시점에 결정됨 |
| 캡슐화 | 포함된 객체의 내부 구현이 숨겨짐 | 부모 클래스의 구현이 자식 클래스에 노출됨 |
| 클래스 관계 | 객체 간의 협력 관계(has-a) | 부모-자식 관계(is-a) |
| 코드 재사용성 | 전달받은 위임 객체의 기능 재사용\* | 전체 클래스 구조를 재사용, 구조 변경 어려움 |

### Delegation 사용 시 주의점

Delegate 객체가 내부적으로 상태를 갖는 프로퍼티를 갖는 경우 그 프로퍼티를 참조하는 메서드가 예상과 다른 동작을 할 수 있다. 때문에 Deletation 사용을 위해선 위임할 객체를 상태를 갖지 않도록 인터페이스로 구현하는 것이 좋을 것 같다.

여기까지 위임의 개념과 위임이 상속과 어떻게 다른지 그리고 그 차이에서 오는 주의점을 알아봤다.

---

그럼 안드로이드에서는 이 위임을 어떻게 사용하고 있는지 사례를 찾아보자.

# 위임 방식의 이해

먼저 위임을 사용하는 방식을 몇가지 살펴보자.

* 프로퍼티 위임(Property Delegation)
    
* 인터페이스 위임(Interface Delegation)
    
* `Lazy` 를 사용한 지연 초기화
    
* 기타 표준 라이브러리가 제공하는 위임
    

### 프로퍼티 위임: 변수에 사용되는 by 키워드

코틀린 문서의 [Delegating to another property](https://kotlinlang.org/docs/delegated-properties.html#delegating-to-another-property) 파트를 보면

> A property can delegate its getter and setter to another property. Such delegation is available for both top-level and class properties (member and extension). The delegate property can be:

* A top-level property
    
* A member or an extension property of the same class
    
* A member or an extension property of another class
    

*프로퍼티는 그 프로퍼티를 다른 프로퍼티의 getter 와 setter에 위임 할 수 있다.*

* *최상위 레벨 프로퍼티*
    
* *같은 클래스의 멤버, 확장 프로퍼티*
    
* *다른 클래스의 멤버, 확장 프로퍼티*
    

```kotlin
var topLevelInt: Int = 0
class ClassWithDelegate(val anotherClassInt: Int)

class MyClass(var memberInt: Int, val anotherClassInstance: ClassWithDelegate) {
    var delegatedToMember: Int by this::memberInt // 같은 클래스의 멤버 프로퍼티
    var delegatedToTopLevel: Int by ::topLevelInt // 최상위 프로퍼티

    val delegatedToAnotherClass: Int by anotherClassInstance::anotherClassInt // 다른 클래스의 프로퍼티
}
var MyClass.extDelegated: Int by ::topLevelInt // 최상위 프로퍼티
```

### 인터페이스 위임: 시그니처에 사용되는 by 키워드

클래스의 특정 인터페이스 구현을 다른 객체에 위임할 수 있다. (앞서 배웠다)

### by lazy 를 구현한 지연 초기화 위임

안드로이드 앱 개발에서 by lazy{} 를 사용해 Lazy를 구현해 지연 초기화를 사용한다.

### 기타 표준 라이브러리가 제공하는 위임

코틀린 표준 라이브러리의 다양한 위임 클래스 (`Delegates.observable` 등)를 사용하여 프로퍼티의 동작을 제어할 수 있다. (여기선 다루지 않는다)

# Android 에서의 by 키워드

안드로이드에서는 by 키워드를 어떻게 사용하고 있는지 몇 가지 사례에서 알아보자.

* by lazy 초기화
    
* by ViewModels()
    

### by lazy

> `lazy()` is a function that takes a lambda and returns an instance of `Lazy`

by lazy 는 Lazy 를 람다를 활용해 인스턴스화 하고 위임하는 손쉬운 방법이다

내부 구현은 아래와 같다.

```kotlin
public actual fun <T> lazy(initializer: () -> T): Lazy<T> = SynchronizedLazyImpl(initializer)
```

`by lazy {}` 는 `SynchronizedLazyImpl`를 통해 람다의 마지막 줄의 값을 반환 값으로 하여 Lazy를 초기화 해 프로퍼티 위임을 한다.

### by ViewModels()

`by viewModels()` 는 안드로이드의 ViewModel 을 편하게 초기화 하고 사용할 수 있게 해주는 코틀린의 위임 패턴이다. 이 기능은 Android KTX(Kotlin Extensions) 라이브러리에 포함되어 있으며, 내부적으로 `ViewModelLazy` 를 사용하여 ViewModel을 초기화하고 반환하는 람다표현식이다.

MainActivity의 viewModels() 내부 구현을 보면 viewModels() 는 Lazy 를 반환한다.

```kotlin
@MainThread
public inline fun <reified VM : ViewModel> ComponentActivity.viewModels(
    noinline extrasProducer: (() -> CreationExtras)? = null,
    noinline factoryProducer: (() -> Factory)? = null
): Lazy<VM> {
    val factoryPromise = factoryProducer ?: {
        defaultViewModelProviderFactory
    }

    return ViewModelLazy(
        VM::class,
        { viewModelStore },
        factoryPromise,
        { extrasProducer?.invoke() ?: this.defaultViewModelCreationExtras }
    )
}
```

Lazy의 구현을 보자.

```kotlin
public interface Lazy<out T> {
    public val value: T
    public fun isInitialized(): Boolean
}
```

상태를 갖는 value와 이 상태가 초기화 되면 그 후부터 남은 생애주기 동안 항상 true 를 반환하는 메서드가 있다.

근데 여기서 의문이 한가지 든다. by 를 통해서 getValue, setValue operator 를 변수에 위임해 변수를 통해 delegate 의 값을 사용한다는 것을 앞에서 배웠다.

하지만 여기 Lazy 객체는 getValue, setValue operator 를 구현하지 않고 있다. 이럴 땐 Lazy 객체의 어떤 기능이 위임 받는 변수에 위임되는 것일까?

# 실습을 통한 by 구현의 이해

우선 IntelliJ IDEA 에서 아무것도 없는 클래스를 변수에 위임해봤다.

```kotlin
class NumberDelegate {}

fun main() {
    val numberDelegate = NumberDelegate()
    val number by numberDelegate
}
```

IDE 가 경고를 준다.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718264427229/810ac727-0111-4721-8194-2276f9bfc497.png align="center")

클래스에 getValue 멤버 메서드를 만들라고 한다. 자동완성으로 생성해봤다.

```kotlin
class NumberDelegate {
    operator fun getValue(nothing: Nothing?, property: KProperty<*>): Any {
        return 1
    }
}

fun main() {
    val numberDelegate = NumberDelegate()
    val number by numberDelegate
    println(number)  // 1
}
```

일반 클래스를 by 로 위임하면 getValue를 기본으로 위임한다는 것을 알았다. (Android Stduio에서도 같았다.)

지금은 위임받는 변수를 val 로 두어 getValue 만 구현하면 되지만 var 변수에 위임하는 경우 setValue 함수도 구현해야 한다. 이때 getValue 와 setValue 는 내부 메서드이든 확장함수이든 상관 없다는 것을 앞서 배웠다.

by 를 사용해 프로퍼티 위임을 위해선 getter/setter 의 구현이 필요하다는 것을 알았는데 Lazy 는 인터페이스니까 확장함수로 가지고 있겠다고 생각해서 코드를 다시 보니, 확장함수로 가지고 있었다.

```kotlin
@kotlin.internal.InlineOnly
public inline operator fun <T> Lazy<T>.getValue(thisRef: Any?, property: KProperty<*>): T = value
```

나도 같은 구현을 할 수 있었다.

```kotlin
class NumberDelegate {}

private operator fun NumberDelegate.getValue(nothing: Nothing?, property: KProperty<*>): Any {
    return 1
}

fun main() {
    val numberDelegate = NumberDelegate()
    val number by numberDelegate
    println(number) // 1
}
```

이제 by viewModels() 이 내부적으로 어떻게 구현되었는지 알겠다.

그럼 이제 컴포즈 앱에서 자주 보이는 위임 용례에 대해서 몇 가지 살펴보고 마무리하겠다.

# Jetpack Compose에서의 위임

예제를 보자.

```kotlin
@Composable
fun CounterScreen() {
    var count by remember { mutableStateOf(0) }

    Button(onClick = { count++ }) {
        Text(text = "Clicked $count times")
    }
}
```

count 변수에 by를 사용해 remember 가 반환하는 결과를 위임한다. 뭘 반환하는지 보자.

```kotlin
@Composable
inline fun <T> remember(crossinline calculation: @DisallowComposableCalls () -> T): T =
    currentComposer.cache(false, calculation)
```

remember 함수는 currentComposer.cache(false, calculation)의 결과를 반환한다.

```kotlin
@ComposeCompilerApi
inline fun <T> Composer.cache(invalid: Boolean, block: @DisallowComposableCalls () -> T): T {
    @Suppress("UNCHECKED_CAST")
    return rememberedValue().let {
        if (invalid || it === Composer.Empty) {
            val value = block()
            updateRememberedValue(value)
            value
        } else it
    } as T
}
```

결국 remember 는 관리할 상태를 캐싱하는 역할이고 remember{}의 반환 값은 블록의 반환 값과 같다.

이렇게 우리는 변수 count에 단순히 `mutableStateOf(0)`이 반환하는 결과를 위임한다는 것을 알았다.

```kotlin
@Composable
fun CounterScreen() {
    var count by remember { mutableStateOf(0) }

    Button(onClick = { count++ }) {
        Text(text = "Clicked $count times")
    }
}
```

`mutableStateOf()`는 `MutableState` 를 반환하고 이 타입에선 getValue 만 찾을 수 있었다.

```kotlin
@Suppress("NOTHING_TO_INLINE")
inline operator fun <T> MutableState<T>.setValue(thisObj: Any?, property: KProperty<*>, value: T) {
    this.value = value
}
```

count 값은 var 타입으로 선언되어있으니 getter/setter 모두 있어야했다. 그래서 MutableState의 부모 타입에서 getter를 찾아봤다.

MutableState 는 State 를 확장할테니 거기서 찾아보니 찾을 수 있었다.

```kotlin
@Suppress("NOTHING_TO_INLINE")
inline operator fun <T> State<T>.getValue(thisObj: Any?, property: KProperty<*>): T = value
```

자식 타입에 없는 getter 와 setter operator를 상속 중인 부모타입에서 가져올 수 있다는 것을 확신하기 위해 간단한 예제를 만들어 확인했다.

```kotlin
import kotlin.reflect.KProperty

open class State {
    open var value: String = "aaa"
}

class MutableState : State() {
    override var value: String = super.value
}

private operator fun MutableState.setValue(nothing: Nothing?, property: KProperty<*>, newValue: Any) {
    // 자식의 setter
    value = newValue as String
}

private operator fun State.getValue(nothing: Nothing?, property: KProperty<*>): Any {
    // 부모의 getter
    return value
}

fun main() {
    val mutableState = MutableState()
    var state by mutableState
    println(state)  // aaa
    state = "ccc"
    println(state)  // ccc
}
```

코드에서 부모 타입(State)은 getter, 자식(MutableState)은 setter operator만 갖는다. 자식을 state 에 위임하면 각각의 getter, setter 를 위임받아 사용한다.

이때 위임을 사용하지 않으려면 직접 value를 참조하면 된다.

```kotlin
@Composable
fun CounterScreen() {
    val countState = remember { mutableStateOf(0) }

    Button(onClick = { countState.value++ }) {
        Text(text = "Clicked ${countState.value} times")
    }
}
```

# 최종 정리

* 위임은 특정 객체의 기능을 다른 객체에게 맞기는 것이다.
    
    * 이를 명시적으로 구현할 수도 있고 코틀린 처럼 언어 수준에서 지원할 수도 있다.
        
* 코틀린에서는 위임을 by 키워드를 통해 지원한다.
    
* 프로퍼티 위임을 위해서는 val/var 에 따라 getter/setter operator 의 구현이 필요하다.
    
    * 이때 operator 는 확장함수여도 된다.
        
    * Lazy는 지연 초기화를 위한 클래스로 하나의 값을 가지고 확장함수로 그 값에 대한 getter를 가지고 있다.
        
* by viewModels() 은 내부적으로 ViewModel이 반환한 Lazy 의 구현 클래스인 ViewModelLazy 인스턴스를 변수에 위임하는데 Lazy 는 value 를 val 로 가지고 있어 getter 만을 변수에 위임한다.
    
* Compose 에서는 위임을 자주 사용하는데 remember와 관련 없다. remember는 단순 캐싱을 위한 함수이다.
    
* Compose에서는 State를 관리해야 하는데 State interface는 하나의 값만 프로퍼티로 가지므로 위임을 통해 타입을 거치지 않고 이 값(상태)에 대한 getter 와 setter 를 위임받아 상태를 직접 조작하는 것처럼 코드를 구현할 수 있다.
    

# 참조

[https://kotlinlang.org/docs/delegation.html](https://kotlinlang.org/docs/delegation.html)

[https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-lazy/](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-lazy/)