---
title: "[Design Pattern]Delegation pattern"
datePublished: Tue Jan 02 2024 03:55:32 GMT+0000 (Coordinated Universal Time)
cuid: clqvtgubt000108l50mn97vtv
slug: design-patterndelegation-pattern

---

In software engineering, the delegation pattern is an object-oriented design pattern that allows object composition to achieve the same code reuse as inheritance.

> In simple terms, the delegation pattern provides a way to reuse code by allowing an object to delegate some responsibilities to another object. This enables the establishment of relationships between objects and the sharing of code without relying on inheritance.

# Definition

In the Introduction to Gamma et al. 1994, delegation is defined as:

Delegation is a way to make composition as powerful for reuse as inheritance \[Lie86, JZ91\]. In delegation, two objects are involved in handling a request: a receiving object delegates operations to its delegate. This is analogous to subclasses deferring requests to parent classes. But with inheritance, an inherited operation can always refer to the receiving object through the this member variable in C++ and self in Smalltalk. To achieve the same effect with delegation, the receiver passes itself to the delegate to let the delegated operation refer to the receiver.

# Example

In the example below (using the Kotlin programming language), the class Window delegates the area() call to its internal Rectangle object (its delegate).

```kotlin
class Rectangle(val width: Int, val height: Int) {
    fun area() = width * height
}

class Window(val bounds: Rectangle) {
    // Delegation
    fun area() = bounds.area()
}
```

# References

[https://en.wikipedia.org/wiki/Delegation\_pattern](https://en.wikipedia.org/wiki/Delegation_pattern)