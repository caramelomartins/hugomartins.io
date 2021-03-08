---
title: "Understanding Differences Between List and MutableList in Kotlin"
date: 2021-03-09
draft: false
summary:  "A potentially exaggerated exploration of how Kotlin implements `List` and `MutableList`, featuring Kotlin's source code examples."
authors: Hugo Martins
categories: [ kotlin, immutability ]
---

I've been thinking about immutability a lot...interesting, I know. I can almost feel your excitement through this screen! In particular, I've been trying to investigate and learn ways in which you can break immutability in Kotlin, either knowingly or unknowingly. My last attempt involves casting immutable lists as mutable lists, in order to modify a _supposedly_ immutable list into a mutable one. While I have been unsuccessful in my attempts, with the particulars of what I want to do, I learned quite an interesting amount about what makes a `List` different from a `MutableList`, and I'll share a bit more about what I've learned here.

## Interfaces, Interfaces, Interfaces

A quick glance at [Kotlin's documentation](https://kotlinlang.org/docs/collections-overview.html#list) provides a good overview about what the difference between them is: while `List` provides read-only access to stored elements, `MutableList` provides "list-specific write operations" that allow us to add or remove particular elements from an existing list. What peaked my interest was the last sentence of the documentation:

> In Kotlin, the default implementation of List is ArrayList which you can think of as a resizable array.

Uhh, exciting!! So, `List` isn't _really_ a simple list per se but is actually an interface for `ArrayList`. We can assume that `MutableList` is simply another interface for `ArrayList`, I guess. We could assume or we can simply go and check how the Kotlin standard library actually implements these things.

In package `kotlin.collections` we can see a definition for `List`:

```kotlin
public interface List<out E> : Collection<E> {  (...)  }
```

`List` inherits from `Collection` and features a series of function definitions, all of them read-only, such as `get`.  `Collection` is another interface which inherits from `Iterable`.

In the same package, we can see a definition for `MutableList`:

```kotlin
public interface MutableList<E> : List<E>, MutableCollection<E> { (...)  }
```

`MutableList` inherits from `MutableCollection`. `MutableList` features a series of additional methods such as `add` and `remove`, granting mutability capacities. `MutableCollection` inherits from `Collection` and `MutableIterable`, which inherit from `Iterator`.

In itself, this doesn't let us know much more but it already unveils that `List` and `MutableList` are essentially two different interfaces, one with read-only methods and the other with additional write-specific methods.

## Functions, Functions, Functions

Let's look at it from a different perspective. What happens when we invoke `List()` for example? Invoking `List()` surprisingly doesn't initiate an object immediately:

```kotlin
@SinceKotlin("1.1")
@kotlin.internal.InlineOnly
public inline fun <T> List(size: Int, init: (index: Int) -> T): List<T> = MutableList(size, init)
```

As we can see from the above snippet, when we invoke `List()` we are actually calling a function that returns a `List` , the interface we saw above, but it actually invokes another function:

```kotlin
@SinceKotlin("1.1")
@kotlin.internal.InlineOnly
public inline fun <T> MutableList(size: Int, init: (index: Int) -> T): MutableList<T> {
    val list = ArrayList<T>(size)
    repeat(size) { index -> list.add(init(index)) }
    return list
}
```

`MutableList()` creates a new `ArrayList`, initializes it with the appropriate values and then returns. Upon returning, `ArrayList` is transformed into a `MutableList`. We can now clearly see _why_, in Kotlin's documentation, it appears so explicitly that the default implementation of lists is an `ArrayList`.

For the curious among you, `ArrayList` is just a typed alias for `java.util.ArrayList`:

```kotlin
@SinceKotlin("1.1") public actual typealias ArrayList<E> = java.util.ArrayList<E>
```

If we go inspect what `java.util.ArrayList` actually is...it turns out that it extends `java.util.function.Consumer.AbstractList` and implements `java.util.List`. We are starting to see some connections here!

## Mixing Interfaces and Functions

So, in essence, `List` and `MutableList` can be two separate things at once:

1. Interfaces, where `List` provides only read-only properties and `MutableList` provides write operations.
2. Functions, which can be used to instantiate objects from those interfaces that rely on `ArrayList` for their actual implementation.

Let's take another step back now...in Kotlin's documentation we are usually greeted with `listOf` and `mutableListOf`. What are these?

Let's take a sneak peek at one of the `listOf` functions:

```kotlin
/**
 * Returns a new read-only list of given elements.  The returned list is serializable (JVM).
 * @sample samples.collections.Collections.Lists.readOnlyList
 */
public fun <T> listOf(vararg elements: T): List<T> = if (elements.size > 0) elements.asList() else emptyList()
```

We know elements from `vararg` will be arrays, which means that `listOf` essentially transforms an array of elements into a list with `List` interface.

`mutableListOf` is a bit different but in essence the same:

```kotlin
/**
 * Returns a new [MutableList] with the given elements.
 * @sample samples.collections.Collections.Lists.mutableList
 */
public fun <T> mutableListOf(vararg elements: T): MutableList<T> =
    if (elements.size == 0) ArrayList() else ArrayList(ArrayAsCollection(elements, isVarargs = true))
```

In both cases above, the functions call internal implementations of `ArrayList` but they return different interfaces which, as we have seen above, provide different mutability functions: while `List` only offers read-only operations, `MutableList` offers write operations too.

Maybe I've exaggerated the amount of digging into Kotlin's source code but I believe the differences between `List` and `MutableList` became clearer with this exploration, even though some internal magic of how Kotlin works might still be a bit of a mystery.
