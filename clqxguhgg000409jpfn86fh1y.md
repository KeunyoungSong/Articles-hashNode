---
title: "[kotlin-stdlib]LongArray"
datePublished: Wed Jan 03 2024 07:37:46 GMT+0000 (Coordinated Universal Time)
cuid: clqxguhgg000409jpfn86fh1y
slug: kotlin-stdliblongarray

---

# Summary

##### For Native, An array of longs.

## Constructors

* #### init
    

```kotlin
<init>(size: Int, init: (Int) -> Long)
```

Creates a new array of the specified size, where each element is calculated by calling the specified init function.

The function init is called for each array element sequentially starting from the first one. It should return the value for an array element given its index.

### source

```kotlin
public final class ByteArray {
    public constructor(@Suppress("UNUSED_PARAMETER") size: Int) {}
    public inline constructor(size: Int, init: (Int) -> Byte): this(size) {
        for (i in 0..size - 1) {
            this[i] = init(i)
        }
    }
```

* #### init
    

```kotlin
<init>(size: Int)
```

Creates a new array of the specified size, with all elements initialized to zero.

### Properties

* size
    

### Extension Properties

* indices
    
* lastIndex
    

# Tip

1. Once you use this class's constructor, there is no need to create a `mutableList` and convert it to a `LongArray`.
    

# In Practice

[An array of n numbers with intervals of x](https://school.programmers.co.kr/learn/courses/30/lessons/12954)

```kotlin
class Solution {
    fun solution(x: Int, n: Int): LongArray {
        return LongArray(n){x.toLong() * (it+1)}
        
        // var answer = mutableListOf<Long>()
        // (0 until n).fold(x.toLong()){ acc, _ ->
        //     answer.add(acc)
        //     acc + x
        // }
        // return answer.toLongArray()
    }
}
```

# Supported functions

### Functions

* get
    
* iterator
    
* set
    

### Extension functions

* all
    
* any
    
* aslterable
    
    * AsIterable returns an object that does not support lazy evaluation.
        
* asSequence
    
    * The returned Sequence, on the other hand, supports lazy evaluation and computes elements only when needed.
        
* associate
    
    * Returns a Map containing key-value pairs provided by transform function applied to elements of the given array.
        
    * val charCodes = intArrayOf(72, 69, 76, 76, 79)  
        val byCharCode = charCodes.associate { it to Char(it) }  
        println(byCharCode) // {72=H, 69=E, 76=L, 79=O}
        
* associateBy
    
* associateTo
    
* associateWith
    
* associateWith
    
* associateWithTo
    
* asUlongArray
    
* average
    
* binarySearch
    
    * The list is expected to be sorted into ascending order.
        
* component1~5
    
    * Returns 1~5th element from the array
        
* contains
    
    * Returns `true` if element is found in the array.
        
* contentEquals
    
* contentHashCode
    
* contentToString()
    
* count
    
* distinct
    
    * Returns a list containing only distinct elements from the given array.
        
    * val list = listOf('a', 'A', 'b', 'B', 'A', 'a') println(list.distinct()) // \[a, A, b, B\] println(list.distinctBy { it.uppercaseChar() }) // \[a, b\]
        
* distinctBy
    
* drop
    
    * val chars = ('a'..'z').toList() println(chars.drop(23)) // \[x, y, z\] println(chars.dropLast(23)) // \[a, b, c\] println(chars.dropWhile { it &lt; 'x' }) // \[x, y, z\] println(chars.dropLastWhile { it &gt; 'c' }) // \[a, b, c\]
        
* dropLast
    
* dropLastWhile
    
* dropWhile
    
* elementAtOrElse
    
    * Returns an element at the given index or the result of calling the defaultValue function if the index is out of bounds of this array.
        
    * println(emptyList.elementAtOrElse(0) { "no int" }) // no int
        
* elementAtOrNull
    
    * println(emptyList.elementAtOrNull(0)) // null
        
* filter
    
    * Returns a list containing only elements matching the given predicate.
        
* fileterIndexed
    
    * val numbers: List = listOf(0, 1, 2, 3, 4, 8, 6)  
        val numbersOnSameIndexAsValue = numbers.filterIndexed { index, i -&gt;  
        index == i }
        
        println(numbersOnSameIndexAsValue) // \[0, 1, 2, 3, 4, 6\]
        
* filterIndexedTo
    
* filterNot
    
    * val notMultiplesOf3 = numbers.filterNot { number -&gt; number % 3 == 0 }
        
* filterNotTo
    
* filterTo
    
* find
    
    * Returns the first element matching the given predicate, or `null` if no such element was found.
        
    * val numbers = listOf(1, 2, 3, 4, 5, 6, 7)  
        val firstOdd = numbers.find { it % 2 != 0 }  
        val lastEven = numbers.findLast { it % 2 == 0 }
        
* findLast
    
    * Returns the last element matching the given predicate, or `null` if no such element was found.
        
* first
    
    * Returns the first elements
        
* firstOrNull
    
    * Returns the first element, or `null` if the array is empty.
        
* flatMap
    
    * Returns a single list of all elements yielded from results of transform function being invoked on each element of original array.
        
    * val list = listOf("123", "45")  
        println(list.flatMap { it.toList() }) // \[1, 2, 3, 4, 5\]
        
* flatMapIndexed
    
* faltMapIndexedTo
    
* flatMapTo
    
* fold
    
* foldIndexed
    
* foldRight
    
    * Accumulates value starting with initial value and applying operation from right to left to each element and current accumulator value.
        
* foldRightIndexed
    
* forEach
    
    * Performs the given action on each element.
        
* forEachIndexed
    
* getOrElse
    
    * Returns an element at the given index or the result of calling the defaultValue function if the index is out of bounds of this array.
        
* getOrNull
    
* groupBy
    
    * val words = listOf("a", "abc", "ab", "def", "abcd")  
        val byLength = words.groupBy { it.length }
        
        println(byLength.keys) // \[1, 3, 2, 4\]  
        println(byLength.values) // \[\[a\], \[abc, def\], \[ab\], \[abcd\]\]
        
    * val nameToTeam = listOf("Alice" to "Marketing", "Bob" to "Sales", "Carol" to "Marketing")  
        val namesByTeam = nameToTeam.groupBy({ it.second }, { it.first }) println(namesByTeam) // {Marketing=\[Alice, Carol\], Sales=\[Bob\]}
        
* groupByTo
    
    * val mutableNamesByTeam = nameToTeam.groupByTo(HashMap(), { it.second }, { it.first })  
        // same content as in namesByTeam map, but the map is mutable println("mutableNamesByTeam == namesByTeam is ${mutableNamesByTeam == namesByTeam}") // true
        
* indexOf
    
    * Returns first index of element, or -1 if the array does not contain element.
        
* indexOfFirst
    
* indexOfLast
    
* intersect
    
* isEmpty
    
* isNotEmpty
    
* joinTo
    
    * Appends the string from all the elements separated using separator and using the given prefix and postfix if supplied.
        
    * val sb = StringBuilder("An existing string and a list: ")  
        val numbers = listOf(1, 2, 3)  
        println(numbers.joinTo(sb, prefix = "\[", postfix = "\]").toString()) // An existing string and a list: \[1, 2, 3\]
        
* joinToString
    
    * Creates a string from all the elements separated using separator and using the given prefix and postfix if supplied.
        
    * val numbers = listOf(1, 2, 3, 4, 5, 6)  
        println(numbers.joinToString()) // 1, 2, 3, 4, 5, 6 println(numbers.joinToString(prefix = "\[", postfix = "\]")) // \[1, 2, 3, 4, 5, 6\]
        
* last
    
* lastIndexOf
    
* lastOrNull
    
* map
    
    * val numbers = listOf(1, 2, 3)  
        println(numbers.map { it \* it }) // \[1, 4, 9\]
        
* mapIndexed
    
* mapIndexedTo
    
* mapTo
    
* maxByOrNull
    
    * Returns the first element yielding the largest value of the given function or `null` if there are no elements.
        
    * val nameToAge = listOf("Alice" to 42, "Bob" to 28, "Carol" to 51)  
        val oldestPerson = nameToAge.maxByOrNull { it.second }  
        println(oldestPerson) // (Carol, 51)
        
* maxOf
    
    * Returns the largest value among all values produced by selector function applied to each element in the array.
        
* maxOfOrNull
    
* maxOfWith
    
* maxOfWithOrNull
    
* maxOrNull
    
* maxWith
    
* maxWithOrNull
    
* minByOrNull
    
    * Returns the first element yielding the smallest value of the given function or `null` if there are no elements.
        
    * val list = listOf("abcd", "abc", "ab", "abcde")  
        val shortestString = list.minByOrNull { it.length }  
        println(shortestString) // ab
        
* minOf
    
    * Returns the smallest value among all values produced by selector function applied to each element in the array.
        
* minOfOrNull
    
* minOfWith
    
* minOfWithOrNull
    
* minOrNull
    
* minWith
    
* minWithOrNull
    
* none
    
    * Returns `true` if the array has no elements.
        
* onEach
    
    * Performs the given action on each element and returns the array itself afterwards.
        
    * `forEach` is used when you want to modify the collection.
        
    * `onEach` is used when you do not want to modify the collection.
        
* onEachIndexed
    
* partition
    
    * Splits the original array into pair of lists, where *first* list contains elements for which predicate yielded `true`, while *second* list contains elements for which predicate yielded `false`.
        
    * val list = listOf(Person("Tom", 18), Person("Andy", 32), Person("Sarah", 22)) val result = list.partition { it.age &lt; 30 } println(result) // (\[Tom - 18, Sarah - 22\], \[Andy - 32\])
        
* random
    
    * Returns a random element from this array.
        
* radomOrNull
    
    * Returns a random element from this array, or `null` if this array is empty.
        
* reduce
    
    * Accumulates value starting with the first element and applying operation from left to right to current accumulator value and each element.
        
* reduceIndexed
    
* reduceIndexedOrNull
    
* reduceOrNull
    
* reduceRight
    
* reduceRightIndexed
    
* reduceRightIndexedOrNull
    
* reuceRightOrNull
    
* refTo
    
* reverse
    
    * Reverses elements in the array in-place.
        
* reversed
    
    * Returns a list with elements in reversed order.
        
* reversedArray
    
    * Returns an array with elements of this array in reversed order.
        
* runningFold
    
    * Returns a list containing successive accumulation values generated by applying operation from left to right to each element and current accumulator value that starts with initial value.
        
    * val strings = listOf("a", "b", "c", "d") println(strings.runningFold("s") { acc, string -&gt; acc + string }) // \[s, sa, sab, sabc, sabcd\]
        
* runningFoldIndexed
    
* runningReduce
    
* runningReduceIndexed
    
* scan
    
* scannindexed
    
* shuffle
    
    * Randomly shuffles elements in this array in-place.
        
* single
    
    * Returns the single element, or throws an exception if the array is empty or has more than one element.
        
* singleOrNull
    
* slice
    
    * Returns a list containing elements at specified indices.
        
* sliceArray
    
    * Returns an array containing elements of this array at specified indices.
        
* sort
    
    * Sorts the array in-place according to the order specified by the given comparison function.
        
    * val intArray = intArrayOf(4, 3, 2, 1)
        
        intArray.sort()
        
        println(intArray.joinToString()) // 1, 2, 3, 4
        
    * val intArray = intArrayOf(4, 3, 2, 1)
        
        intArray.sort(0, 3)
        
        println(intArray.joinToString()) // 2, 3, 4, 1
        
* sortDescending
    
    * Sorts elements in the array in-place descending according to their natural sort order.
        
* sorted
    
    * Returns a list of all elements sorted according to their natural sort order.
        
* sortedArray
    
    * Returns an array with all elements of this array sorted according to their natural sort order.
        
* sortedArrayDescending
    
* sortedBy
    
    * Returns a list of all elements sorted according to natural sort order of the value returned by specified [selector](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/sorted-by.html#kotlin.collections$sortedBy(kotlin.Array((kotlin.collections.sortedBy.T)),%20kotlin.Function1((kotlin.collections.sortedBy.T,%20kotlin.collections.sortedBy.R?)))/selector) function.
        
    * val list = listOf("aaa", "cc", "bbbb")  
        val sorted = list.sortedBy { it.length }  
        println(sorted) // \[cc, aaa, bbbb\]
        
* sortedByDescending
    
* sortedDescending
    
* sortedWith
    
    * Returns a list of all elements sorted according to the specified comparator.
        
* subtract
    
    * Returns a set containing all elements that are contained by this array and not contained by the specified collection.
        
* sum
    
    * val total = numbers.sum()
        
* sumBy
    
    * Returns the sum of all values produced by selector function applied to each element in the array.
        
* sumByDouble
    
* sumOf
    
    * val totalAge = people.sumOf { it.age }
        
* take
    
    * Returns a list containing first [n](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/take.html#kotlin.collections$take(kotlin.LongArray,%20kotlin.Int)/n) elements.
        
* takeLast
    
    * Returns a list containing last [n](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/take-last.html#kotlin.collections$takeLast(kotlin.LongArray,%20kotlin.Int)/n) elements.
        
* takeLastWhile
    
    * Returns a list containing last elements satisfying the given predicate.
        
* takeWhile
    
    * Returns a list containing first elements satisfying the given predicate.
        
* toCollection
    
    * Appends all elements to the given destination collection.
        
* toCValues
    
* toHashSet
    
* toList
    
* toMutableList
    
* toSet
    
* toSortedSet
    
* toULongArray
    
* union
    
    * Returns a set containing all distinct elements from both collections.
        
* withIndex
    
    * Returns a lazy Iterable that wraps each element of the original array into an IndexedValue containing the index of that element and the element itself.
        
* zip
    
    * Returns a list of pairs built from the elements of `this` array and the other array with the same index. The returned list has length of the shortest collection.
        
    * val listA = listOf("a", "b", "c")  
        val listB = listOf(1, 2, 3, 4)  
        println(listA zip listB) // \[(a, 1), (b, 2), (c, 3)\]
        

# References

[https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-long-array/](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-long-array/)

[https://github.com/JetBrains/kotlin/blob/0938b46726b9c6938df309098316ce741815bb55/core/builtins/native/kotlin/Arrays.kt#L175](https://github.com/JetBrains/kotlin/blob/0938b46726b9c6938df309098316ce741815bb55/core/builtins/native/kotlin/Arrays.kt#L175)

[https://github.com/JetBrains/kotlin/blob/0938b46726b9c6938df309098316ce7418](https://github.com/JetBrains/kotlin/blob/0938b46726b9c6938df309098316ce741815bb55/kotlin-native/runtime/src/main/kotlin/kotlin/Arrays.kt#L305)  
[15bb55/kotlin-native/runtime/src/main/kotlin/kotlin/Arrays.kt#L305](https://github.com/JetBrains/kotlin/blob/0938b46726b9c6938df309098316ce741815bb55/kotlin-native/runtime/src/main/kotlin/kotlin/Arrays.kt#L305)