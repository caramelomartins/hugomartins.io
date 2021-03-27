---
title: "What's the Difference Between val and var in Kotlin?"
date: 2021-03-03
draft: false
summary:  "`val` and `var` forced a mentally shift on me, coming from a background of loosely typed languages, due to what they entails. What is really the difference between `val` and `var`?"
authors: Hugo Martins
categories: [ kotlin, immutability ]
popular: true
---

Coming from a background of Python and PHP, I'm used to interpreted loosely typed languages in which declaring variables is done in an almost frivolous way. Suffice it to say, when I started writing Kotlin, I was a bit confused about the fact that there are two different ways of declaring essentially the same variables. Even when I wrote C#, back in the day, I never encountered anything similar to this.

Those declarations didn't differ in type or visibility but rather on their mutability. Kotlin has quite a diverse offering of built-in functionality that encourages immutability. One of those is declaring properties with `val` and `var`. But what's the difference between `val` and `var` then?

`val` is a keyword in Kotlin that allows us to define properties as read-only. Because they are read-only, they cannot be modified. `var`, on the other hand,  is a keyword that can be used to declare properties in Kotlin that are mutable. These properties are not read-only (as with `val`) and they can be modified at will.

What does that mean in practice? Let's consider the following example:

```kotlin
data class Car(
    val model: String
)

fun main() {
    val car = Car("Tesla")
    car.model = "Ford"
}
```

If we try to compile this, Kotlin's compiler complains with a `Val cannot be reassigned` error. A property declared with `val` cannot be safely modified at runtime. In fact, these properties don't even have a setter accessor that we can use to set its value. On the other hand, let's consider the following example:

```kotlin
data class Car(
    var model: String
)

fun main() {
    val car = Car("Tesla")
    car.model = "Ford"
}
```

with the above example, Kotlin's compiler won't complain because the property `model` in the data class `Car` is declared with a `var`. By declaring with a `var`, it can be modified without any issues, even though `car` is declared with `val`. `var` has both getters and setters as accessors.

In essence, `val` and `var` are both property declaring keywords, that allow us to declare properties with varying types and visibility modes, differing on the mutability of those properties. While `var` allows properties to be mutable, `val` does not, enforcing immutability straight from the source code at runtime.

While it forced a shift of mentality, I've come to appreciate more and more being able to use `val` and enforcing immutability. I've come to rely on it so much that I end up missing it whenever I go back to other languages that don't prize immutability as much as Kotlin.
