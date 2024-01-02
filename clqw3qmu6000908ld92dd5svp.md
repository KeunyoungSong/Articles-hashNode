---
title: "[kotlin-stdlib]average"
datePublished: Tue Jan 02 2024 08:43:05 GMT+0000 (Coordinated Universal Time)
cuid: clqw3qmu6000908ld92dd5svp
slug: kotlin-stdlibaverage

---

# Summary

Returns an average value of elements in the array.

```kotlin
@kotlin.jvm.JvmName("averageOfByte")
public fun Array<out Byte>.average(): Double {
    var sum: Double = 0.0
    var count: Int = 0
    for (element in this) {
        sum += element
        ++count
    }
    return if (count == 0) Double.NaN else sum / count
}
```

# Tip

1. Supports only numeric types
    
2. Always returns a Double
    

# In Practice

[Average](https://school.programmers.co.kr/learn/courses/30/lessons/120817)

```kotlin
class Solution {
    fun solution(numbers: IntArray): Double {
        // return numbers.sum() / numbers.size.toDouble()
        return numbers.average()
    }
}
```

# Supported Data Types

* ByteArray
    
* ShortArray
    
* IntArray
    
* LongArray
    
* FloatArray
    
* DoubleArray
    

# References

[https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/average.html](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/average.html)