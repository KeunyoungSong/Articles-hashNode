---
title: "[Article Review]The 2022 Android Developer Roadmap: part 1"
datePublished: Fri Nov 24 2023 11:24:14 GMT+0000 (Coordinated Universal Time)
cuid: clpcjbnh4000a08jug2kce1da
slug: reviewthe-2022-android-developer-roadmap-part-1
tags: android

---

#  References
GitHub: https://github.com/skydoves/android-developer-roadmap  
Article1: https://getstream.io/blog/android-developer-roadmap/  
Article2: https://getstream.io/blog/android-developer-roadmap-part-2/  
Article3: https://getstream.io/blog/android-developer-roadmap-part-3/  
Article4: https://getstream.io/blog/design-patterns-and-architecture-the-android-developer-roadmap-part-4/

---

# Review - Article 1

In this article, we will cover 4 sections of the loadmap
1. Languages
1. Android OS
1. The Android Platform
1. App Manifest

## Application Fundamentals
- Android Package
- Languages

### Languages

#### Android Programming Languages
All Android applications must be written using Kotlin, Java and C/C++.
C++ is used to write performance-oriented or hardware-based features that use the Java Native Interface (JNI) to call native functions.

#### Java
When Java came out, Java was one of the most suitable languages.

#### Kotlin
Kotlin was born from JetBrains. It was initially designed for the JVM environment and combines functional and object-oriented programming.
> It has both features from functional language and object-oriented programming language.
  
Now, it has become a rising star in Android development.  
Here are a few reasons why Kotlin pairs so well with Android:
- Interoperability: Kotiln is 100% interoperable with Java and supports great interoperability with JVM environments.
- Safety: Android apps built with Kotlin code are 20% less likely to crash according to the Android docs.
- Asynchronicity: Kotlin's use of coroutines allows it to provide asynchronous(non-blocking) programming support.

## Android Operating Systems
- Multi-User Linux
- File Permissions
- Resource isolations
- Process Management

The foundation of the Android platform is the Linux kernel. Linux has been used in millions of security-sensitive systems since its creation in 1991.  

According to the Android docs, Android utilizes several key Linux security features including:
- A user-based permissions model
- Process isolation
- An extensible mechanism for secure inter-process communication(IPC)
- The ability to remove unnecessary and/or insecure parts of the kernel

The Android platform takes advantage of the Linux multiuser system with its own Application Sandbox, which isolates app resources from each other and protects apps and the system from malicious apps.

## Android Platform Architecture
- The Linux Kernel
- Hardware Abstraction Layer
- Android Runtime
- Native Libraries
- Java API Framework
- System Apps

### Linux Kernel
The Linux Kernel is the core of the Android Platform architecture. It manages low-level memory and all the available hardware drivers - such as the WiFi driver, camera driver, audio driver, Bluetooth driver, display driver, and all peripheral drivers - via its Low Memory Killer Daemon.

### Hardware Abstraction Layer(HAL)
The hardware abstraction layer bridges hardware capabilities to the higher-level Java API Framework by defining standard interfaces, allowing you to implement the low-level functionalities without the need to make modifications to the fundamental components of the computer system, such as the operating system kernel or device drivers.  

HAL implementations are packaged into modules, which are sorted as a shared library (*.so file) and loaded by the Android system at the appropriate time.

### Android Runtime
Android Runtime(ART) is an application Runtime System used by Android OS and one of the core features in the Android ecosystem. ART was invented to replace the Dalvik virtual machine(DVM) for devices running Android version 5.0(Lollipop) or higher.  

The main role of ART is to execute the Dalvik Executable format(DEX) by translating DEX(Dalvik Executable) bytecode into machine code that your system can understand.  

> Kotlin code -> Java code -> Dex bytecode -> running on ART or DVM  

Some key features are directly related to the speed of running the Android applications. Key features of ART include the following:

- Ahead-of-time(AOT) and just-in-time(JIT) compilation
- Enhanced garbage collection(GC)
- Converting an app package's DEX files to condensed machine code(on Android 9 (API level 28+))

### Native C/C++ Libraries

The Android platform includes Native APIs, which work on top of the Native Development Kit(NDK).
> NDK is a set of tools provided by Google for Android app development. It is distributed alongside Android Studio and command-line tools, allowing developers to integrate native code into Android apps.
  
> Android Studio is commonly used for incorporating the NDK. It seamlessly integrates with the NDK, enabling developers to set up and use it to include native code, typically written in C or C++, into their Android apps.  

> The NDK is included in the Android Development Tools(ADT), providing developers with a comprehensive set of tools for Android app development.

The Native APIs let you manage native activities and access physical device components such as cameras, sensors, graphics, and audio. They are also exposed to the higher-level layers, so you can control the components of the physical device on the Java API framework.

### Java API Framework(Application Framework)
The Java API framework is a collection of Android libraries written in Java and Kotlin that provide the entire feature-set of the Android OS. Android APIs include an extensible View System, reusable components, and system managers which are used to build your Android applications by simplifying the reuse of interfaces.  

One of the more powerful APIs in the Java APIs framework is Android Jetpack, which is pushed by Google. Jetpack accelerates development speed by reducing boilerplate code so that developers can focus on the code they are about.  

Also, it provides solutions like <u>Lifecycle, UI toolkit, Navigation, Security, Caching, Scheduler, Dependency Injection, and a lot more.</u>

### System Apps
System apps are pre-installed apps such as email, SNS messaging, calendars, internet browsing, contacts, and more, which are located in the system partition with your ROM. The configuration of the System Apps may vary depending on the mobile phone manufacturer. 

## App Manifest
Evey Android project must have an AndroidManifest.xml file which describes essential information about the application such as the package name, entry points, components, permissions, and metadata.

### Package Name and Application ID
The package attribute is the only guaranteed way to identify your application on the Android system(Physical device) and in Google Play. The Android build tools use the package attribute for the following:  

- Applying the package name as the namespace for your app resources.
- Resolving implicit class names that are declared in the manifest file.
    - For example, with the above manifest, an activity declared as <activity android:name=".MainActivity"> is resolved to be io.getstream.chat.android.ui.MainActivity. So the classes also must be in the same package folder.














