---
title: "[Docs Review] Filtering collections"
datePublished: Fri Jan 12 2024 15:51:55 GMT+0000 (Coordinated Universal Time)
cuid: clratgmez000t09l37jp27gtp
slug: docs-review-filtering-collections

---

Filtering is one of the most popular tasks in collection processing. In Kotlin, filtering conditions are defined by ***predicates*** – lambda functions that take a collection element and return a boolean value: `true` means that the given element matches the predicate, `false` means the opposite.

The standard library contains a group of extension functions that let you filter collections in a single call.

> These functions leave the original collection unchanged, so they are available for both [mutable and read-only](https://kotlinlang.org/docs/collections-overview.html#collection-types) collections.

To operate the filtering result, you should assign it to a variable or chain the functions after filtering.

## Filter by predicate﻿

The basic filtering function is [`filter()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/filter.html). When called with a predicate, `filter()` returns the collection elements that match it.

For both `List` and `Set`, the resulting collection is a `List`, for `Map` it's a `Map` as well.

> List and set -&gt; List  
> Map -&gt; Map

```kotlin
val numbers = listOf("one", "two", "three", "four")  
val longerThan3 = numbers.filter { it.length > 3 }
println(longerThan3) // [three, four]

val numbersMap = mapOf("key1" to 1, "key2" to 2, "key3" to 3, "key11" to 11)
val filteredMap = numbersMap.filter { (key, value) -> key.endsWith("1") && value > 10}
println(filteredMap) // {key11=11}
```

If you want to use **element positions** in the filter, use [`filterIndexed()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/filter-indexed.html).

To filter collections by **negative conditions**, use `filterNot()`.

```kotlin
val numbers = listOf("one", "two", "three", "four")

val filteredIdx = numbers.filterIndexed { index, s -> (index != 0) && (s.length < 5)  }
val filteredNot = numbers.filterNot { it.length <= 3 }

println(filteredIdx) // [two, four]
println(filteredNot) // [three, four]
```

[`filterIsInstance()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/filter-is-instance.html) returns collection elements of a given type.

Being called on a `List<Any>`, `filterIsInstance<T>()` returns a `List<T>`, thus allowing you to call functions of the `T` type on its items.

```kotlin
val numbers = listOf(null, 1, "two", 3.0, "four")
println("All String elements in upper case:")
numbers.filterIsInstance<String>().forEach {
    println(it.uppercase())
}

/*
All String elements in upper case:
TWO
FOUR
*/
```

[`filterNotNull()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/filter-not-null.html) returns all non-nullable elements.

Being called on a `List<T?>`, `filterNotNull()` returns a `List<T: Any>`, thus allowing you to treat the elements as non-nullable objects.

```kotlin
val numbers = listOf(null, "1", "two", null)
numbers.filterNotNull().forEach {
    println(it.length)   // length is unavailable for nullable Strings
}
/*
1
3
*/
```

> To filter by element, use `filter()`
> 
> To filter by index and element, use `filterIndexed()`
> 
> To filter by negative conditions, use `filterNot()`
> 
> To filter by elements of a specific type. use `filterIsInstance()`
> 
> To filter by non-nullable elements, user `filterNotNull()`

# Partition

Another filtering function – [`partition()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/partition.html) – filters a collection by a predicate and keeps the elements that don't match it in a separate list. **So, you have a** `Pair` **of** `List`**s as a return value**: the first list containing elements that match the predicate and the second one containing everything else from the original collection.

```kotlin
val numbers = listOf("one", "two", "three", "four")
val (match, rest) = numbers.partition { it.length > 3 }

println(match) // [three, four]
println(rest) // [one, two]
```

# Test predicates

Finally, there are functions that simply test a predicate against collection elements:

* [`any()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/any.html) returns `true` if at least one element matches the given predicate.
    
* [`none()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/none.html) returns `true` if none of the elements match the given predicate.
    
* [`all()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/all.html) returns `true` if all elements match the given predicate.
    
    * Note that `all()` returns `true` when called with any valid predicate on an empty collection. Such behavior is known in logic as [***vacuous truth***](https://en.wikipedia.org/wiki/Vacuous_truth).
        

`any()` **and** `none()` **can also be used without a predicate**: in this case they just check the collection emptiness.

`any()` returns `true` if there are elements and `false` if there aren't; `none()` does the opposite.

```kotlin
val numbers = listOf("one", "two", "three", "four")
val empty = emptyList<String>()

println(numbers.any()) // true
println(empty.any()) // false

println(numbers.none()) // false
println(empty.none()) // true
```

# References

[https://kotlinlang.org/docs/collection-filtering.html](https://kotlinlang.org/docs/collection-filtering.html)