---
title: "[kotlin-stdlib]fold"
datePublished: Tue Jan 02 2024 14:07:00 GMT+0000 (Coordinated Universal Time)
cuid: clqwfb6or000908l2gqakhn3x
slug: kotlin-stdlibfold

---

# Summary

Accumulates value starting with initial value and applying operation from left to right to current accumulator value and each element.

Returns the specified [initial](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/fold.html#kotlin.collections$fold(kotlin.Array((kotlin.collections.fold.T)),%20kotlin.collections.fold.R,%20kotlin.Function2((kotlin.collections.fold.R,%20kotlin.collections.fold.T,%20)))/initial) value if the array is empty.

### Parameters

`operation` - function that takes current accumulator value and an element, and calculates the next accumulator value.

```kotlin
inline fun <T, R> Iterable<T>.fold(
    initial: R,
    operation: (acc: R, T) -> R
): R
```

# Tip

1. Similar to reduce() but fold allows you to set an initial value.
    
2. Like `reduce()`, it accumulates the return values of the `operation`. (In the case of a lambda, it is the last line.)
    
3. The return type is the same as the data type of the initial value T.
    

# In Practice

[Numbers with intervals of x up to n](https://school.programmers.co.kr/learn/courses/30/lessons/12954)

```kotlin
class Solution {
    fun solution(x: Int, n: Int): LongArray {
        // return LongArray(n){x.toLong() * (it+1)}
        var answer = mutableListOf<Long>()
        (0 until n).fold(x.toLong()){ acc, _ ->
            answer.add(acc)
            acc + x
        }
        return answer.toLongArray() 
    }
}
```

# Supported Data Types

* Array
    
* ByteArray
    
* ShortArray
    
* IntArray
    
* LongArray
    
* FloatArray
    
* DoubleArray
    
* BooleanArray
    
* CharArray
    

# References

[https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/fold.html](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/fold.html)