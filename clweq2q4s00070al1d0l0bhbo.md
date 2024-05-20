---
title: "Image Picker Android library 배포하기(4)"
datePublished: Mon May 20 2024 08:46:44 GMT+0000 (Coordinated Universal Time)
cuid: clweq2q4s00070al1d0l0bhbo
slug: image-picker-android-library-4
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1716194723158/6e7265cc-3f32-4b13-8b0f-289194ca5ca4.png
tags: android, publishing, android-libraries

---

[이전 포스팅(Image Picker Android library 배포하기(3))](https://hashnode.com/post/clwen5nzm000x09l45a3veliq)에서 Fragment 를 사용하며 생긴 문제를 어떻게 해결했는지를 중점적으로 작성했었다. 이번 포스팅에서는 일단? 완성한 라이브러리를 어떻게 배포했는지에 대한 내용을 작성하려고한다. 시작해보자.

### 배포는 어떻게 하는 건데?

사실 라이브러리 배포는 이번이 처음이다. 그래서 인터넷을 좀 뒤져봤고 Jitpack 이라는 서비스를 찾을 수 있었다. Github 레포를 통해 라이브러리를 제공할 수 있게 해주는 서비스라고 한다.

> JitPack은 GitHub, GitLab, Bitbucket 등 다양한 Git 저장소에서 소스 코드를 가져와 Maven 또는 Gradle을 통해 빌드하고, 이를 개발자가 사용할 수 있는 라이브러리로 배포합니다. 이 과정은 매우 간단하며, 몇 가지 설정만으로 라이브러리를 손쉽게 배포할 수 있습니다.

프로젝트에 몇 가지 세팅을 한 뒤 깃헙에 올리고 태그를 만들면 Jitpack 에서 그 태그를 버전삼아 프로젝트에서 다운받을 수 있게 주소를 제공한다.

### 배포를 위한 고군분투...

우선 라이브러리를 배포하기 위해선 모든 기능을 Android Library Moudule 에 작성해야 한다.

그럼 프로젝트는 2개의 모듈 app, mylibrary 를 갖게 된다.

이후 mylibrary 모듈에 jitpack 관련 세팅을 해줘야 한다. 나는 kts 를 사용했다.

build.gradle.kts(:mylibrary) 파일을 수정한다.

```kotlin
plugins {
	alias(libs.plugins.androidLibrary)
	alias(libs.plugins.jetbrainsKotlinAndroid)
	id("maven-publish") // 추가
}
...

dependencies {
	...
}

publishing { // 추가
	publications {
		afterEvaluate {
			register<MavenPublication>("release") {
				from(components["release"])
				groupId = "com.github.KeunyoungSong" 
				artifactId = "repo name"
				version = "1.0"
			}
		}
	}
}
```

이후 github repo 에 올린 후 tag 를 만들어 올리면 아래 같이 해당 태그를 기준으로 라이브러리가 배포된다.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1716192634722/cf661056-eef1-4e9f-8e91-dfab77594c63.png align="center")

위에보면 1.0.0 버전의 log 문서 색이 빨간색이지 않는가? 그렇다 방금전 배포는 정확하게 배포되지 않았다.

### 로그를 보자...

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1716192787524/64e46782-50c3-4768-b27f-49dd18433b28.png align="center")

으악.. 뭐가 많이 나왔다. 천천히 보니 중복되는 내용이 좀 있고 버전에 문제가 있는 것 같다.

> Incompatible because this component declares a component for use during compile-time, compatible with Java 11 and the consumer needed a component for use during runtime, compatible with Java 8

현재 컴포넌트는 Java 11 버전과 호환이 되는데 개발자(consumer)는 Java 8 버전을 사용하고 있다고 한다.

### mylibrary 모듈의 build.gradle.kts 를 확인해보자

```kotlin
compileOptions {
	sourceCompatibility = JavaVersion.VERSION_1_8
	targetCompatibility = JavaVersion.VERSION_1_8
}
```

1\_8? 뭐야 18 버전을 사용하고 있는 건가?

아니다.

해당 버전을 타고 들어가보면 11 버전 이전에는 1\_1, 1\_2 ... 이후에는 11, 12 처럼 네이밍 되고 있다는 것을 알 수 있다.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1716193219883/a2d9a25e-c9b8-4a44-938a-623ccbd14546.png align="center")

그렇다 나는 Java 8 버전을 사용하고 있고 Jitpack 은 Java 11 버전을 사용하기 때문에 생긴 문제인 것이다. 변경해보자.

```kotlin
compileOptions {
	sourceCompatibility = JavaVersion.VERSION_11
	targetCompatibility = JavaVersion.VERSION_11
}
```

### 이제 다시 배포해보자

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1716192634722/cf661056-eef1-4e9f-8e91-dfab77594c63.png align="center")

또 문제가 생긴다. (1.0.2)

로그를 보자.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1716193403274/c8ebbd95-1725-4baa-a728-d27768c073e4.png align="center")

에러 로그가 뭔가 좀 바뀌었다.

> Android Gradle plugin requires Java 17 to run. You are currently using Java 11.

현재 내가 프로젝트에 세팅한 AGP(Android Gradle plugin)는 8.3.1 버전이고 이는 Java 17 을 필요로 하는데 나는 11을 사용하고 있다고 한다. 확인해보자.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1716193595166/69817902-79db-41ca-be89-139fa663fddb.png align="center")

맞다. 나는 AGP 8.3.1 을 사용하고 있다.

하지만 Java 11은 프로젝트에서 사용하는 버전이 아닌 Jitpack 에서 라이브러리를 만드는 과정에서 사용하는 것이다.

내가 Jitpack 에서 사용하는 자바 버전을 어떻게 세팅한단 말인가?

방법이 있다.

[Jitpack 의 공식 문서](https://docs.jitpack.io/building/)를 보면 CUSTOM COMMANDS 부분에 자바 버전을 설정할 수 있는 방법을 제공한다. 아래와 같은 코드를 작성했다.

```kotlin
jdk:
  - openjdk17
before_install:
  - sdk install java 17.0.9-open
  - sdk use java 17.0.9-open
```

위 코드를 jitpack.yml 이란 이름으로 프로젝트 최 상단에 추가한다.

Jitpack 의 Java 버전을 수정했읜 라이브러리로 제공될 모듈의 버전도 수정한다.

```kotlin
compileOptions {
	sourceCompatibility = JavaVersion.VERSION_17
	targetCompatibility = JavaVersion.VERSION_17
}
```

### 자 이제 새로 커밋을 하고 푸시한 뒤 태그를 달아 배포버전을 만들자

짜잔. 성공이다.

사용법은 사이트에 잘 나와있다.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1716194258902/334e0ef7-a0ec-48eb-bfa1-922b59de2bae.png align="center")

배포한 라이브러리를 사용하기 위해서는 settings.gradle.kts 에 아래 코드를 추가해 라이브러리를 다운 받을 수 있도록 해야한다.

```kotlin
dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
       google()
       mavenCentral()
       maven("https://jitpack.io") // 추가
    }
}
```

이제 라이브러리를 사용할 프로젝트로가 다른 라이브러리처럼 의존성을 추가한 뒤 싱크를 맞춰 다운을 받고 사용하면 된다.

```kotlin
dependencies {
    
    implementation(libs.androidx.core.ktx)
    implementation(libs.androidx.appcompat)
    implementation(libs.material)
    implementation(libs.androidx.activity)
    implementation(libs.androidx.constraintlayout)
    testImplementation(libs.junit)
    androidTestImplementation(libs.androidx.junit)
    androidTestImplementation(libs.androidx.espresso.core)
    
    implementation ("com.github.KeunyoungSong:SelectSaveImagePicker:1.0.0")
}
```

### 모듈이 라이브러리 형태로 다운받아진다!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1716194396532/4b6bb8a6-3857-4f35-b2af-177ec2e13bdc.png align="center")

처음에 배포할 프로젝트에 app 모듈과 배포를 위한 모듈 두 가지가 있어 app 모듈까지 배포하고 싶지 않아 프로젝트 수준에서 app 모듈을 지우고 배포를 하는 등 삽질을 많이 했다.

하지만 결과적으로 위 다운받은 내영을 보면 똑똑한 Jitpack 이 app 모듈을 제외하고 라이브러리 형태로 제공한다는 것을 알 수 있었다.

아래처럼 라이브러리가 제공하는 기능을 사용할 수 있었다.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1716194856875/14a960bc-60f1-434c-ac5e-2659a15b7bad.png align="center")

---

다음 포스팅에서는 첫 안드로이드 라이브러리 배포 프로젝트를 회고하고 개발 과정에서 발견한 개선 사항과 아쉬운 점에 대해 작성하겠다.