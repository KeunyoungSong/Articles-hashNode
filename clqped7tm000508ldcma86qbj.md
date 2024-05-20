---
title: "[Doc Review]Properties"
datePublished: Thu Dec 28 2023 16:06:12 GMT+0000 (Coordinated Universal Time)
cuid: clqped7tm000508ldcma86qbj
slug: docs-reviewproperties

---

## Declaring properties

Properties in Kotlin classes can be declared either as mutable, using the var keyword, or as read-only, using the val keyword.

## Getters and setters

The full syntax for declaring a property is as follows:

```kotlin
var <propertyName>[: <PropertyType>] [= <property_initializer>]
    [<getter>]
    [<setter>]
```

The initializer, getter, and setter are optional. The property type is optional if it can be inferred from the initializer or the getter's return type, as shown below:

```kotlin
var initialized = 1 // has type Int, default getter and setter
// var allByDefault // ERROR: explicit initializer required, default getter and setter implied
```

The full syntax of a read-only property declaration differs from a mutable one in two ways: it starts with val instead of var and does not allow a setter:

```kotlin
val simple: Int? // has type Int, default getter, must be initialized in constructor
val inferredType = 1 // has type Int and a default getter
```

You can define custom accessors for a property. If you define a custom getter, it will be called every time you access the property (this way you can implement a computed property). Here's an example of a custom getter:

```kotlin
class Rectangle(val width: Int, val height: Int) {
    val area: Int // property type is optional since it can be inferred from the getter's return type
        get() = this.width * this.height
}
```

You can omit the property type if it can be inferred from the getter:

```kotlin
val area get() = this.width * this.height
```

If you define a custom setter, it will be called every time you assign a value to the property, except its initialization. A custom setter looks like this:

```kotlin
var stringRepresentation: String
    get() = this.toString()
    set(value) {
        setDataFromString(value) // parses the string and assigns values to other properties
    }
```

> By convention, the name of the setter parameter is value, but you can choose a different name if you prefer.

### Backing fields

1. Declaration of Fields: In Kotlin, it is not allowed to directly declare a field for storing the value of a property. Directly declaring a field as shown below is not permitted:
    

```kotlin
// Directly declaring a field like this is not allowed
// var counterField: Int = 0 // Error
```

1. Automatic Generation of Backing Fields: When a property needs a backing field, Kotlin generates it automatically. The backing field is responsible for storing the actual value of the property, and it is used when the getter and setter of the property are called.
    

```kotlin
var counter = 0 // Automatically generated backing field is used
```

1. Reference to Backing Fields: Within the property's accessors, the field identifier is used to reference the backing field. This allows for additional logic or restrictions on the property's value.
    

```kotlin
set(value) {
    if (value >= 0)
        field = value
        // counter = value // Error: Using the actual name 'counter' would make the setter recursive
}
```

> The use of field provides direct access to the backing field. It's important to note that using the property's name (e.g., counter) within its setter can lead to recursive calls and should be avoided.

### Backing properties

If you want to do something that does not fit into this implicit backing field scheme, you can always fall back to having a backing property:

```kotlin
private var _table: Map<String, Int>? = null
public val table: Map<String, Int>
    get() {
        if (_table == null) {
            _table = HashMap() // Type parameters are inferred
        }
        return _table ?: throw AssertionError("Set to null by another thread")
    }
```

> You can make custom `backing field` by making `backing properties`

## Compile-time constants

If the value of a read-only property is known at compile time, mark it as a compile time constant using the const modifier. Such a property needs to fulfil the following requirements:

* It must be a top-level property, or a member of an object declaration or a companion object.
    
* It must be initialized with a value of type String or a primitive type
    
* It cannot be a custom getter
    

The compiler will inline usages of the constant, replacing the reference to the constant with its actual value. However, the field will not be removed and therefore can be interacted with using reflection.

Such properties can also be used in annotations:

```plaintext
const val SUBSYSTEM_DEPRECATED: String = "This subsystem is deprecated"

@Deprecated(SUBSYSTEM_DEPRECATED) fun foo() { ... }
```

## Late-initialized properties and variables

Normally, properties declared as having a non-nullable type must be initialized in the constructor. However, it is often the case that doing so is not convenient. For example, properties can be initialized through dependency injection, or in the setup method of a unit test. In these cases, you cannot supply a non-nullable initializer in the constructor, but you still want to avoid null checks when referencing the property inside the body of a class.

```kotlin
public class MyTest {
    lateinit var subject: TestSubject

    @SetUp fun setup() {
        subject = TestSubject()
    }

    @Test fun test() {
        subject.method()  // dereference directly
    }
}
```

> This modifier can be used on var properties declared inside the body of a class (not in the primary constructor, and only when the property does not have a custom getter or setter), as well as for top-level properties and local variables.

> The type of the property or variable must be non-nullable, and **it must not be a primitive type.**

Accessing a lateinit property before it has been initialized throws a special exception that clearly identifies the property being accessed and the fact that it hasn't been initialized.

### Checking whether a lateinit var is initialized

To check whether a `lateinit var` has already been initialized, use `.isInitialized` on the reference to that property:

```kotlin
if (foo::bar.isInitialized) {
    println(foo.bar)
}
```

This check is only available for properties that are lexically accessible when declared in the same type, in one of the outer types, or at top level in the same file.

## Overriding properties

[Kotlin: Overriding\_properties](https://kotlinlang.org/docs/inheritance.html#overriding-properties)

## Delegated properties

The most common kind of property simply reads from (and maybe writes to) a backing field, but custom getters and setters allow you to use properties so one can implement any sort of behavior of a property. Somewhere in between the simplicity of the first kind and variety of the second, there are common patterns for what properties can do.

A few examples:

* Lazy values
    
* Reading from a map by a given key
    
* Accessing a database
    
* Notifying a listener on access.
    

# References

[https://kotlinlang.org/docs/properties.html](https://kotlinlang.org/docs/properties.html)