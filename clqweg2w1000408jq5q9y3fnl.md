---
title: "[Doc Review]filter"
datePublished: Tue Jan 02 2024 13:42:49 GMT+0000 (Coordinated Universal Time)
cuid: clqweg2w1000408jq5q9y3fnl
slug: kotlin-stdlibfilter

---

# Summary

Returns a list containing only elements matching the given [predicate](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/filter.html#kotlin.collections$filter(kotlin.Array((kotlin.collections.filter.T)),%20kotlin.Function1((kotlin.collections.filter.T,%20kotlin.Boolean)))/predicate).

```kotlin
public inline fun <T> Array<out T>.filter(predicate: (T) -> Boolean): List<T> {
    return filterTo(ArrayList<T>(), predicate)
}
```

# Tip

1. Implemented internally using `filterTo()`.
    
2. `filterTo()` determines the return type based on the destination(ArrayList&lt;T&gt;).
    

## filterTo()

```kotlin
public inline fun <T, C : MutableCollection<in T>> Array<out T>.filterTo(destination: C, predicate: (T) -> Boolean): C {
    for (element in this) if (predicate(element)) destination.add(element)
    return destination
}
```

# In Practice

[Sum of divisors](https://school.programmers.co.kr/learn/courses/30/lessons/12928)

```kotlin
class Solution {
    fun solution(n: Int): Int {
        return (1..n).filter{ n % it == 0}.sum()
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
    
* Iterable&lt;T&gt;
    

# References

[https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/filter.html](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/filter.html)