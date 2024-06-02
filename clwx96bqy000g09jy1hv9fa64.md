---
title: "Android에서 JavaScript와 상호작용하는 방법"
datePublished: Sun Jun 02 2024 08:01:16 GMT+0000 (Coordinated Universal Time)
cuid: clwx96bqy000g09jy1hv9fa64
slug: android-javascript
tags: android, webview

---

[관련 구글 문서](https://developer.android.com/reference/android/webkit/JavascriptInterface)에 따르면,

`public abstract @interface JavascriptInterface`  
`implements`[`Annotation`](https://developer.android.com/reference/java/lang/annotation/Annotation)

android.webkit.JavascriptInterface

> <mark>Annotation that allows exposing methods to JavaScript.</mark> Starting from API level `Build.VERSION_CODES.JELLY_BEAN_MR1` and above, only methods explicitly marked with this annotation are available to the Javascript code. See `WebView.addJavascriptInterface(Object, String)` for more information about it.

Annotation 인터페이스를 상속받는 **JavaScript에 (Java의) 메서드를 노출시키는 애너테이션**이다.

# 안드로이드 스튜디오에서 해당 인터페이스 구현을 살펴봤다.

```javascript
package android.webkit;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@SuppressWarnings("javadoc")
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.METHOD})
public @interface JavascriptInterface {}
```

* @SuppressWarnings("javadoc"): 문서화 주석이 없는 경고를 무시하게 하는 설정
    
* @Retention(RetentionPolicy.RUNTIME): `@JavascriptInterface` 애너테이션이 어느 시점까지 유지되는지를 지정한다. `RetentionPolicy.RUNTIME`은 애노테이션이 런타임 동안 유지되어 리플렉션을 통해 접근할 수 있음을 의미.
    
* @Target({ElementType.METHOD}): `@JavascriptInterface` 애노테이션이 어디에 적용될 수 있는지를 지정합니다. `ElementType.METHOD`는 메서드에만 이 애노테이션을 적용할 수 있음을 의미.
    
* public @interface JavascriptInterface: `JavascriptInterface` 라는 새로운 애노테이션 타입을 정의.
    

# 자바 객체를 웹뷰에 삽입하는 WebView의 공개 메서드 addJavascriptInterface를 문서에서 찾아보면

> Injects the supplied Java object into this WebView. The object is injected into all frames of the web page, including all the iframes, using the supplied name. This allows the Java object's methods to be accessed from JavaScript. For applications targeted to API level [`Build.VERSION_CODES.JELLY_BEAN_MR1`](https://developer.android.com/reference/android/os/Build.VERSION_CODES#JELLY_BEAN_MR1) and above, only public methods that are annotated with [`JavascriptInterface`](https://developer.android.com/reference/android/webkit/JavascriptInterface) can be accessed from JavaScript. For applications targeted to API level [`Build.VERSION_CODES.JELLY_BEAN`](https://developer.android.com/reference/android/os/Build.VERSION_CODES#JELLY_BEAN) or below, all public methods (including the inherited ones) can be accessed, see the important security note below for implications.
> 
> Note that injected objects will not appear in JavaScript until the page is next (re)loaded. JavaScript should be enabled before injecting the object. For example:

요약하자면,

#### 요약

`addJavascriptInterface` 는 Java 객체를 WebView에 주입하는 메서드이다.

#### 메서드 주입 대상

* **API 레벨 17 (Jelly Bean MR1) 이상**: `@JavascriptInterface` 애노테이션이 달린 메서드만 JavaScript에서 접근 가능.
    
* **API 레벨 16 (Jelly Bean) 이하**: 모든 public 메서드에 접근 가능. 이 경우 보안 위험이 크다.
    

#### 보안 위험

* **Jelly Bean 이하**: JavaScript가 리플렉션(reflection)을 통해 모든 public 메서드와 필드에 접근할 수 있다. 따라서 공격자가 악의적인 코드를 실행할 수 있는 가능성이 있다.
    
* **최신 API 레벨 사용 권장**: 보안을 위해 가능한 API 레벨 17 이상을 타겟팅하고, Android 4.2 (Jelly Bean MR1) 이상에서만 이 메서드를 사용해야 한다.
    

#### 인터페이스 적용

* 인터페이스 객체가 Javascript에서 사용가능하려면 페이지 로드 전 객체를 주입해야 한다.
    
* 객체를 추가하기 전, WebView의 JavaScript 사용을 활성화 해야 한다.
    

#### 스레드 안정성

* JavaScript는 WebView의 백그라운드 스레드에서 Java 객체와 상호작용합니다. 따라서 스레드 안전성을 유지해야 합니다.
    

#### 모든 프레임에서 접근 가능

* <mark>객체는 웹 페이지의 모든 프레임(iframe)에서도 접근할 수 있다.</mark> 따라서 신뢰할 수 없는 콘텐츠가 WebView에 로드되지 않도록 해야 한다.
    

> Use of this method in a WebView containing untrusted content could allow an attacker to manipulate the host application in unintended ways, executing Java code with the permissions of the host application. **Use extreme care when using this method in a WebView which could contain untrusted content.**

믿을 수 없는 컨텐츠를 웹뷰에 올리면 공격자가 앱의 권한을 조작할 수 있으니 사용에 정말 주의하라고 강조한다. 페이지 내 iframe에도 인터페이스 객체가 주입되어 앱의 Java 코드에 접근할 수 있다고 하니 정말 조심해야 겠다.

# 어떻게 JavaScript와 Java가 상호작용하는 하는 걸까?

구글의 [WebView Java Bridge (WebView#addJavascriptInterface())](https://chromium.googlesource.com/chromium/src/+/refs/heads/main/android_webview/docs/java-bridge.md) 문서를 보자.

### 개요

이 문서에너는 Java &lt;=&gt; JavaScript 브릿지 구현에 관한 개념을 설명한다.

### API

* **addJavascriptInterface(Object object, String name)**: 주어진 Java 객체를 WebView에 주입한다. 이 객체는 JavaScript에서 접근할 수 있게 된다.
    
* **removeJavascriptInterface(String name)**: 이전에 주입된 객체를 제거한다.
    

### 중요한 포인트

* 객체의 추가나 제거는 자바스크립트 페이지가 새로 로드되기 전까지 반영되지 않는다.
    
* `@JavascriptInterface`로 애노테이션된 메서드만 JavaScript에서 접근 가능합니다.(하지만 API Level 16 이하는 모든 자바 메서드에 접근 가능하다고 이전 문서에서 확인했다.)
    
* Java 객체의 메서드는 WebView의 백그라운드 스레드에서 호출된다.
    
    * 즉 JavaScript에서 요청한 Java 코드가 완료될 때까지 WebView의 메인 애플리케이션 스레드(브라우저 UI 스레드)는 정지한다.
        

### 매개변수와 반환값 변환

* 가상 머신간에 전달될 수 있는 것은 제한적이며 허용되는 것은 다음과 같다.
    
    * 기본 값(primitive values)
        
    * 단일 차원 배열(singe-dimensional arrays)
        
    * “배열 같은” JavaScript 객체(“length” 속성을 가지는 객체와 ES6의 타입 배열)
        
    * 이전에 주입된 Java 객체(자바스크립트에서 자바로 전달되는 경우)
        
    * 새로운 Java 객체(자바에서 자바스크립트로 전달되는 경우). 이러한 객체는 이름 없이 `addJavascriptInterface`를 호출한 것처럼 JavaScript에 주입됩니다. 또한, 이러한 일시적 객체의 수명 주기는 다릅니다 (아래 참조).
        

### 객체의 라이프사이클

Java bridge의 목적은 Java와 JavaScript라는 두 VM(가상머신)간의 상호작용을설정하는 것이다. 두 VM 모두 개체 수명 관리에 유사한 접근 방식을 사용한다. VM은 GC(Garbage Collection) cycle 동안 참조 되지 않은 개체를 수집하고 삭제한다. Java bridge가 추가하는 점은 이제 한 VM의 개체가 다른 VM의 개체를 가상으로 참조(및 삭제 방지)를 할 수 있다는 점이다. 다음 Java 코드를 고려해보면

```java
// Java
webView.addJavascriptInterface ( new MyObject (), "myObject" );  
```

인스턴스화된 MyObject는 이제 JavaScript 대응 항목에 의해 가상으로 보유되고 있으며 Java 측에서 이에 대한 명시적인 참조가 없음에도 불구하고 Java VM에 의해 가비지 수집이 되지 않는다. MyObject 인스턴스는 **addJavascriptInterface** 호출 작업이 **RemoveJavascriptInterface** 호출에 의해 취소될 때까지 계속 참조됩니다.

```java
// Java
webView.removeJavascriptInterface("myObject");
```

더 흥미로운 상황은 주입된 Java 객체의 메서드에서 반환된 임시 객체의 경우이다. 다음을 보자.

```java
// in Java
class MyObject {
    class Handler {
    }
    @JavascriptInterface
    public Object getHandler() { return new Handler(); }
}
```

다시 말하지만, `getHandler()`메서드를 통해 반환된 객체는 Java 측에서 명시적으로 참조되지 않지만 JavaScript 측에서 "사용 중"인 동안에는 삭제되어서는 안된다. 여기서 말하는 "사용 중"이 말하는 기간은 JavaScript에서 getHandler 호출로 암묵적으로 생성된 JavaScript 인터페이스 객체의 수명에 의해 결정된다. 즉, 이는 Java 측의 Handler 인스턴스가 해당 JavaScript 인터페이스 객체가 여전히 참조되는 동안 유지되어야 함을 의미한다.

> 참조되고 있는 클래스의 인스턴스가 살아있어야 한다는 걸 보면 Java측에서 Handler 인스턴스를 생성해두고 이를 JavaScript에서 참조하여 사용하는 구조인 것 같다.

```javascript
// in JavaScript
{
   let handler = myObject.getHandler();
}
```

아래의 그림은 이전 예제에서 생성된 Java와 JavaScript 개체 간의 관계를 보여준다.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1717308769504/485a1580-95bb-42f5-849d-c75c4a52c2b1.png align="center")

Java 및 JavaScript VM은 완전히 독립적이며 서로의 존재를 인식하지 못한다. 이들은 서로 다른 프로세스에서 작동할 수 있으며 이론적으로는 서로 다른 물리적 시스템에서도 작동할 수 있다. 따라서 JavaScript 객체에서 Java 객체로의 묘사된 참조는 가상이며 직접적으로 존재하지 않는다. 대신, 필요한 기간 동안 Java 객체를 보관하는 것은 Java Bridge 이다. 위 사진에는 그 부분이 생략되었다.(의역)

지금까지 우리는 추상적인 용어로 Java Bridge 를 생각했다. 그러나 실제로는 WebView 기반 애플리케이션의 맥락에서 사용된다. 브리지의 Java 측면은 WebView 클래스의 인스턴스와 긴밀하게 결합되어 있는 반면 JavaScript 측면은 HTML 렌터링 엔진과 바딩인되어있다. 이는 Chromium 아키텍처 렌더러가 제어 엔터티와 격리되어 있고 Chromium이 주로 C++로 구현되지만 Java로 구현되는 Android 프레임워크와 상호작용해야 한다는 사실로 인해 더욱 복잡해집니다.

따라서 Java 브리지의 아키텍처를 묘사하려면 Java Bridge에 연결된 Chromium 프레임워크의 일부도 포함해야 한다.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1717309462518/1bd26d22-43e3-41bc-aadd-914cff8bc661.png align="center")

> 위 사진은 Java Bridge 에 연결된 Chromium 프레임워크의 일부를 도식화한 것이다.

이제 그 모습이 훨씬 더 무섭다. 여기에 무엇이 있는지 알아보자.

Java VM(브라우저측): WebView 는 android.webkit.WebView 클래스이다. 이는 엠베더에 노출되어 Chromium 렌더링 기계와 상호작용한다. WebView는 모든 Java Bridge를 통해 사용되고 있는 객체가 가비지 컬렉팅 되는 것을 막기 위해 모든 객체를 유지하는(retaining) `Set<Object>` 를 가지고 있다(의역). WebView 클래스는 WebContents 라고 불리는 C++ 객체를 관리합니다(실제로는 이보다 복잡하지만 이런 세부 정보는 우리와 관련이 없습니다). Java Bridge 는 실제로 C++로 구현되어 있으므로 앞서 언급한 *retaining set*는 실제로 C++ 측에서 관리된다. 그러나 C++(*native side*)측의 객체는 retaining set 에 대한 강한 참조를 유지하지 않는다. 그것이 순환 참조를 유발하거나 WebView가 가비지 컬렉팅 되는 것을 막을 수 있기 때문이다.

C++ 브라우저 측: 여기에는 Java 앞서 언급한 **WebContents** 객체가 있다. 이 객체는 JavaBridge와 관련된 모든 요청을 **JavaBridgeHost**에 위임한다. WebContents는 Chromium의 IPC 매커니즘을 통해 렌더러 측의 객체와 통신한다.

> IPC(Inter-Process Communication)란 한 프로세스가 다른 프로세스와 데이터를 교환하는 방법을 의미한다.

C++ 렌더러 측: **RenderFrame**은 단일 HTML 프레임과 대응되고 JavaScript 전역 컨텍스트 개체(일명 **window**)를 "소유"한다. <mark>각 JavaScript 인터페이스 객체에 대해서 대응되는 JavaBrideObject 인스턴스가 유지됩니다.</mark> Chromium 용어에서는 이 객체를 "wrapper"라고 부릅니다(연결된 JavaScript 인터페이스 별로 C++ 에서는 이를 관리하는 JavaBrideObject가 생성됨). Gin 기반 구현에서는 래퍼는 참조 주기로 인한 메모리 누수를 방지하기 위해 해당 JavaScript 인터페이스 객체에 대한 강한 참조를 보유하지 않는다. 래퍼는 해당 JavaScript 객체가 가비지 수집된 이후 JavaScript VM으로부터 알림을 받는다.

앞의 사진에는 한 가지 더 중요한 세부 사항이 누락되어 있다. WebView는 여러 프레임(일반적으로 태그를 사용하여 삽입됨)으로 구성된 복잡한 HTML 문서를 로드할 수 있다. 실제로 이러한 각 프레임에는 고유한 전역 컨텍스트가 있다(다른 프레임들로부터 엑세스 하는 것을 방지할 수 있음). Java Bridge 규칙에 따라서, 각각의 명명된(named) 객체는 모든 프레임들에 주입됩니다. 그리서 우리가 HTML 문서를 WebView에 로드하고, 위에서 설명한 호출을 메인 문서와 내부에서 반복했다고 가정하면, 다음과 같은 그림이 나타날 것이다.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1717312129029/72022e73-4895-49d6-be6c-099dff40acf9.png align="center")

**MyObject.getHandler()** 가 새로운 핸들러 객체를 매번 반환한다면, 우리는 두 개의 핸들러 인스턴스를 갖게 됩니다(하나의 프레임 당 하나). 그러나 여전히 하나의 Object를 가집니다.

만약 **getHandler** 가 매번 같은 핸들러 인스턴스를 반환한다면, JavaScript VM(*latter*)은 이(핸들러)를 참조하는 여러 JavaScript 인터페이스를 갖을 것이다. 따라서 임지 자바 객체는 활성 상태로 있어야 한다. 하나의 이 임시 자바 객체에 대응되는 JavaScript 객체가 존재하는 동안에. Java 측에서는 그것이 반환하는 단일 핸들러 인스턴스에 대한 약한 참조만 유지할 수 있다. 그러니 Java Bridge는 반환하는 핸들러에 대해 강한 참조를 유지해야 한다.

수명 주기 주제를 요약하기 위한 다음은 Java Bridge의 관점에서 본 Java 객체 수명주기의 상태 다이어그램이다.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1717312910202/60ae77f6-4f04-4337-8865-38209a977ece.png align="center")

굵은 테두리에 있는 상태(가운데, 아래, 우측)에서는 Java 객체가 Java Bridge에 의해 가비지 컬렉팅되는 것으로부터 보호됩니다. 가비지 수집된 개체에 여전히 JavaScript 래퍼가 있을 수 있다(즉, "주입된" 상태로 유지됨). 이 경우 메서드를 호출하려는 시도는 실패한다.

"보유되지 않음(*Not retained, injected*) 과 일반 자바 객체(*Ordinary java object*) 상태 사이의 유일한 차이점은 전자의 경우 Java 개체가 여전히 JavaScript 측에 "알려져" 있으므로 이를 호출할 수 있다는 것이다.

또한 명명된 주입 개체가 일시적인 개체가 될 수 있는 방법은 없지만, 일시적인 개체가 명명된 주입 객체가 되는 것은 가능하다.

### 스레딩

주입된 개체의 메서드 호출을 처리할 때 스레팅 문제를 고려해야 한다. API 정의에 따라 WebView가 유지 관리하는 전용 스레드에서 메서드가 호출된다.

인터페이스 메서드에 대한 호출은 동기식이다. JavaScript VM은 중지되고 호출된 메서드에서 결과가 반환될 때까지 기다린다. Chromium에서는 이는 렌더러에서 브라우저로 전송된 IPC 메시지가 동기식이어야 함을 의미한다(이러한 메시지는 실제로 Chromium에서는 거의 사용되지 않음).

* **동기적 호출**
    
    * 자바스크립트 코드에서 자바 메서드를 호출하면 그 호출은 동기적으로 처리된다. 자바 스크립트 엔진은 호출된 자바 메서드가 실행을 완료하고 결과를 반환할 때까지 기다립니다.
        
    * <mark>이 과정동안 자바스크립트 코드 실행은 멈춘 상태가 된다.</mark>
        
* **Java 스레드의 동작**
    
    * 자바 메서드 호출은 WebView의 전용 스레드에서 실행된다. 자바의 UI 스레드가 아닌 별도의 스레드에서 작업이 이뤄진다.
        
    * <mark>자바 스레드는 멈추지 않으며, 요청된 메서드를 실행하고 결과를 반환한다.</mark>
        
* **Chromium의 처리 방식**
    
    * Chromium 브라우저에서는 자바스크립트 엔진이 자바 메서드를 호출할 때 IPC(Inter-Process Communication) 메시지를 사용합니다.
        
    * 이 IPC 메시지는 동기적으로 처리되어야 하기 때문에, 백그라운드 스레드에서 요청을 처리하여 UI 스레드가 차단되지 않도록 합니다.
        

# 요약

#### @JavascriptInterface 애너테이션

* **정의**: 자바 메서드를 자바스크립트에 노출시키는 애너테이션.
    
* **API 레벨**: API 레벨 17(Jelly Bean MR1) 이상에서만 사용 가능.
    
* **애너테이션 설정**:
    
    * `@Retention(RetentionPolicy.RUNTIME)`: 런타임 동안 유지.
        
    * `@Target({ElementType.METHOD})`: 메서드에만 적용 가능.
        

#### addJavascriptInterface 메서드

* **기능**: Java 객체를 WebView에 주입하여 JavaScript에서 Java로 부터 주입된 객체의 메서드에 접근 가능하게 함.
    
* **API 레벨 차이**:
    
    * API 레벨 17 이상: @JavascriptInterface 애너테이션이 달린 메서드만 접근 가능.
        
    * API 레벨 16 이하: 모든 public 메서드 접근 가능, 보안 위험 있음.
        
* **객체 주입 조건**:
    
    * 페이지 로드 전 객체를 주입해야 함.
        
    * WebView의 JavaScript 사용을 활성화해야 함.
        
* **모든 프레임 접근 가능**: 주입된 인터페이스는 웹 페이지 내의 모든 프레임(iframe)에서도 접근 가능.
    

#### 보안

* **위험성**:
    
    * 믿을 수 없는 콘텐츠가 WebView에 로드되면 보안 위험이 큼.
        
    * Jelly Bean 이하: JavaScript가 모든 public 메서드와 필드에 접근할 수 있음, 공격자가 악의적인 코드를 실행할 가능성 있음.
        
* **권장 사항**: 보안을 위해 API 레벨 17 이상을 타겟팅하고, Android 4.2 (Jelly Bean MR1) 이상에서만 사용.
    

#### 스레딩 및 동기적 호출

* **동기적 호출**:
    
    * 자바스크립트에서 자바 메서드를 호출하면, 자바스크립트 실행이 일시 중지됨.
        
    * 자바 메서드가 실행을 완료하고 결과를 반환할 때까지 기다림.
        
* **Java 스레드 동작**:
    
    * 자바 메서드 호출은 WebView의 전용 스레드에서 실행.
        
    * UI 스레드가 아닌 별도의 스레드에서 실행됨.
        
* **Chromium의 처리 방식**:
    
    * 자바스크립트 엔진이 자바 메서드를 호출할 때 IPC(Inter-Process Communication) 메시지를 사용.
        
    * IPC 메시지는 동기적으로 처리되어야 함.
        

#### 객체의 라이프사이클

* **Java Bridge**: Java와 JavaScript 객체 간의 참조를 관리.
    
* **가비지 컬렉션**:
    
    * 주입된 객체가 참조될 때까지 가비지 컬렉션되지 않음.
        
    * JavaBridge는 반환된 객체에 대해 강한 참조를 유지해야 함.
        

---

# 참고

* [Android Developer Documentation on JavascriptInterface](https://developer.android.com/reference/android/webkit/JavascriptInterface)
    
* [Android Developer Documentation on WebView#addJavascriptInterface](https://developer.android.com/reference/android/webkit/WebView#addJavascriptInterface(java.lang.Object,%20java.lang.String))
    
* [Chromium Documentation on Java Bridge](https://chromium.googlesource.com/chromium/src/+/refs/heads/main/android_webview/docs/java-bridge.md)