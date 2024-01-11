---
title: "[Docs Review]State holders and UI State"
datePublished: Thu Jan 11 2024 11:56:59 GMT+0000 (Coordinated Universal Time)
cuid: clr95mnh5001508jx49lcceu9
slug: docs-reviewstate-holders-and-ui-state

---

The [UI layer guide](https://developer.android.com/topic/architecture/ui-layer) discusses unidirectional data flow (UDF) as a means of producing and managing the UI State for the UI layer.  

![](https://developer.android.com/static/images/topic/architecture/ui-layer/udf.png align="center")

**Figure 1**: Unidirectional data flow

It also highlights the benefits of delegating UDF management to a special class called a state holder. **You can implement a state holder either through a** `ViewModel` **or a plain class.** This document takes a closer look at state holders and the role they play in the UI layer.

At the end of this document, you should have an understanding of how to manage application state in the UI layer; that is the UI state production pipeline. You should be able to understand and know the following:

* Understand the types of UI state that exist in the UI layer.
    
* Understand the types of logic that operate on those UI states in the UI layer.
    
* Know how to choose the appropriate implementation of a state holder, such as a `ViewModel` or a simple class.
    

# Elements of the UI state production pipeline

The UI state and the logic that produces it defines the UI layer.

## UI state

[UI state](https://developer.android.com/topic/architecture/ui-layer#define-ui-state) is the property that describes the UI. There are two types of UI state:

* **Screen UI state** is *what* you need to display on the screen. For example, a `NewsUiState` class can contain the news articles and other information needed to render the UI. This state is usually connected with other layers of the hierarchy because it contains app data.
    

## Logic

# Android lifecycle and the types of UI state and logic

## The UI state production pipeline

# State holders and their responsibilities

## Types of state holders

# Business logic and its state holder

## The ViewModel as a business logic state holder

# UI logic and its state holder

# Choose between a ViewModel and plain class for a state holder

# State holders are compoundable

# References

[https://developer.android.com/topic/architecture/ui-layer](https://developer.android.com/topic/architecture/ui-layer)

---

*'Portions of this page are reproduced from work created and* [*shared by the Android Open Source Project*](http://code.google.com/policies.html) *and used according to terms described in the* [*Creative Commons 2.5 Attribution License*](http://creativecommons.org/licenses/by/2.5/)*.*