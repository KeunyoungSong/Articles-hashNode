---
title: "[Doc Review]Numbers"
datePublished: Tue Jan 02 2024 07:18:03 GMT+0000 (Coordinated Universal Time)
cuid: clqw0p9ye000s08ifeoh20pi7
slug: kotlin-docsnumbers
tags: kotlin

---

# Integer types

Kotlin provides a set of built-in types that represent numbers.  
For integer numbers, there are four types with different sizes and, hence, value ranges:

| Type | Size (bits) | Min value | Max value |
| --- | --- | --- | --- |
| `Byte` | 8 | \-128 | 127 |
| `Short` | 16 | \-32768 | 32767 |
| `Int` | 32 | \-2,147,483,648 (-2<sup>31</sup>) | 2,147,483,647 (2<sup>31</sup> - 1) |
| `Long` | 64 | \-9,223,372,036,854,775,808 (-2<sup>63</sup>) | 9,223,372,036,854,775,807 (2<sup>63</sup> - 1) |

When you initialize a variable with no explicit type specification, the compiler automatically infers the type with the smallest range enough to represent the value starting from `Int`. If it is not exceeding the range of `Int`, the type is `Int`. If it exceeds, the type is `Long`. To specify the `Long` value explicitly, append the suffix `L` to the value. Explicit type specification triggers the compiler to check the value not to exceed the range of the specified type.

> The Compiler infers the type when a variable doesn't have a type specification
> 
> A numeric variable without a type can be inferred in `Int` or `Long`.

```kotlin
val one = 1 // Int
val threeBillion = 3000000000 // Long
val oneLong = 1L // Long
val oneByte: Byte = 1
```

Kotlin also provides unsigned integer types.

# Floating-point types

For real numbers, Kotlin provides floating-point types `Float` and `Double` that adhere to the [IEEE 754 standard](https://en.wikipedia.org/wiki/IEEE_754). `Float` reflects the IEEE 754 ***single precision***, while `Double` reflects ***double precision***.

These types differ in their size and provide storage for floating-point numbers with different precision:

| Type | Size (bits) | Significant bits | Exponent bits | Decimal digits |
| --- | --- | --- | --- | --- |
| `Float` | 32 | 24 | 8 | 6-7 |
| `Double` | 64 | 53 | 11 | 15-16 |

You can initialize `Double` and `Float` variables with numbers having a fractional part. It's separated from the integer part by a period (`.`) For variables initialized with fractional numbers, the compiler infers the `Double` type:

```kotlin
val oneLong = 1L // Long
val pi = 3.14 // Double
// val one: Double = 1 // Error: type mismatch
val eFloat = 2.7182818284f // Float, actual value is 2.7182817
```

Unlike some other languages, there are no implicit widening conversions for numbers in Kotlin. For example, a function with a `Double` parameter can be called only on `Double` values, but not `Float`, `Int`, or other numeric values:

```kotlin
fun main() {
    fun printDouble(d: Double) { print(d) }

    val i = 1
    val d = 1.0
    val f = 1.0f

    printDouble(d)
//    printDouble(i) // Error: Type mismatch
//    printDouble(f) // Error: Type mismatch
}
```

To convert numeric values to different types, use [explicit conversions](https://kotlinlang.org/docs/numbers.html#explicit-number-conversions).

# Literal constants for numbers

There are the following kinds of literal constants for integral values:

* Decimals: `123`
    
* Longs are tagged by a capital `L`: `123L`
    
* Hexadecimals: `0x0F`
    
* Binaries: `0b00001011`
    

> Octal literals are not supported in Kotlin.

Kotlin also supports a conventional notation for floating-point numbers:

* Doubles by default: `123.5`, `123.5e10`
    
* Floats are tagged by `f` or `F`: `123.5f`
    

You can use underscores to make number constants more readable:

```kotlin
val oneMillion = 1_000_000
val creditCardNumber = 1234_5678_9012_3456L
val socialSecurityNumber = 999_99_9999L
val hexBytes = 0xFF_EC_DE_5E
val bytes = 0b11010010_01101001_10010100_10010010
```

# Numbers representation on the JVM

On the JVM platform, numbers are stored as primitive types: `int`, `double`, and so on. Exceptions are cases when you create a nullable number reference such as `Int?` or use generics. In these cases numbers are boxed in Java classes `Integer`, `Double`, and so on.

> Nullable numbers are boxed in Java classes, primitive types are not.

Nullable references to the same number can refer to different objects:

```kotlin
val a: Int = 100
val boxedA: Int? = a
val anotherBoxedA: Int? = a

val b: Int = 10000
val boxedB: Int? = b
val anotherBoxedB: Int? = b

println(boxedA === anotherBoxedA) // true
println(boxedB === anotherBoxedB) // false
```

All nullable references to `a` are actually the same object because of the memory optimization that JVM applies to `Integer`s between `-128` and `127`. It doesn't apply to the `b` references, so they are different objects.

On the other hand, they are still equal:

```kotlin
val b: Int = 10000
println(b == b) // Prints 'true'
val boxedB: Int? = b
val anotherBoxedB: Int? = b
println(boxedB == anotherBoxedB) // Prints 'true'
```

# Explicit number conversions

Due to different representations, smaller types ***are not subtypes*** of bigger ones. If they were, we would have troubles of the following sort:

```kotlin
// Hypothetical code, does not actually compile:
val a: Int? = 1 // A boxed Int (java.lang.Integer)
val b: Long? = a // Implicit conversion yields a boxed Long (java.lang.Long)
print(b == a) // Surprise! This prints "false" as Long's equals() checks whether the other is Long as well
```

> So equality would have been lost silently, not to mention identity.

As a consequence, smaller types ***are NOT implicitly converted*** to bigger types. This means that assigning a value of type `Byte` to an `Int` variable requires an explicit conversion:

```kotlin
val b: Byte = 1 // OK, literals are checked statically
// val i: Int = b // ERROR
val i1: Int = b.toInt()
```

All number types support conversions to other types:

* `toByte(): Byte`
    
* `toShort(): Short`
    
* `toInt(): Int`
    
* `toLong(): Long`
    
* `toFloat(): Float`
    
* `toDouble(): Double`
    

In many cases, there is no need for explicit conversions because the type is inferred from the context.

> Arithmetical operations are overloaded for appropriate conversions, for example:

```kotlin
val l = 1L + 3 // Long + Int => Long
```

# Operations on numbers

Kotlin supports the standard set of arithmetical operations over numbers: `+`, `-`, `*`, `/`, `%`. They are declared as members of appropriate classes

You can also override these operators for custom classes.

## Division of integers

> Division between integers numbers always returns an integer number.
> 
> Any fractional part is discarded.

This is true for a division between any two integer types:

```kotlin
val x = 5 / 2
//println(x == 2.5) // ERROR: Operator '==' cannot be applied to 'Int' and 'Double'
println(x == 2)  // true
```

> To return a floating-point type,
> 
> explicitly **convert one of the arguments** to a floating-point type:

```kotlin
val x = 5L / 2
println(x == 2L) // true

val x = 5 / 2.toDouble()
println(x == 2.5)  // true
```

## Bitwise operations

Kotlin provides a set of ***bitwise operations*** on integer numbers. They operate on the binary level directly with bits of the numbers' representation. Bitwise operations are represented by functions that can be called in infix form. They can be applied only to `Int` and `Long`:

```kotlin
val x = (1 shl 2) and 0x000FF000
```

Here is the complete list of bitwise operations:

* `shl(bits)` – signed shift left
    
* `shr(bits)` – signed shift right
    
* `ushr(bits)` – unsigned shift right
    
* `and(bits)` – bitwise **AND**
    
* `or(bits)` – bitwise **OR**
    
* `xor(bits)` – bitwise **XOR**
    
* `inv()` – bitwise inversion
    

## Floating-point numbers comparison

The operations on floating-point numbers discussed in this section are:

* Equality checks: `a == b` and `a != b`
    
* Comparison operators: `a < b`, `a > b`, `a <= b`, `a >= b`
    
* Range instantiation and range checks: `a..b`, `x in a..b`, `x !in a..b`
    

# References

[https://kotlinlang.org/docs/numbers.html](https://kotlinlang.org/docs/numbers.html)