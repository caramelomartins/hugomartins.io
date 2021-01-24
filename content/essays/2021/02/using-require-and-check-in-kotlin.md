---
title: "Using require and check Functions in Kotlin"
date: 2021-02-02
draft: false
authors: Hugo Martins
categories: ["kotlin", "kotest"]
---

`require` and `check`are two functions from Kotlin's standard library that you can use to perform assertions on specific requirements and states that we wish to have at a given time.  Here we'll understand how we can use them and why. <!--more-->

## `require`

`require`, along with `requireNotNull`, can be used to require that a given condition is true. In the case of `require`  it will take a `Boolean` type as an argument while in the case of `requireNotNull` it will take a type as an argument. Both functions will throw a `IllegalArgumentException` if the conditions are not met - `false` in `require` or `null` in `requireNotNull`.

From `require`'s [official documentation](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/require.html):

```kotlin
fun getIndices(count: Int): List<Int> {
    require(count >= 0) { "Count must be non-negative, was $count" }
    // ...
    return List(count) { it + 1 }
}

// getIndices(-1) // will fail with IllegalArgumentException

println(getIndices(3)) // [1, 2, 3]
```

In this example, if we try to call `getIndices` with an index lower than 0, it will throw an `IllegalArgumentException`. Using this functions is important in order to validate arguments that are passed to functions in a idiomatic way.  Since validating arguments is considered a _best practice_, even if the arguments are thoroughly documented, to produce clean, maintainable and secure code, using these functions can help us do that.

## `check`

`check` , along with `checkNotNull`, are functions that can be used to validate that a given condition is true (or different than null in `checkNotNull`). Similar to the `require` functions, `check` will take a `Boolean` as an argument while `checkNotNull` will take a type as an argument. Both functions throw an `IllegalStateException` if the conditions are not met.

From `check`'s [official documentation](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/check.html):

```kotlin
var someState: String? = null
fun getStateValue(): String {
    val state = checkNotNull(someState) { "State must be set beforehand" }
    check(state.isNotEmpty()) { "State must be non-empty" }
    // ...
    return state
}

// getStateValue() // will fail with IllegalStateException

someState = ""
// getStateValue() // will fail with IllegalStateException

someState = "non-empty-state"
println(getStateValue()) // non-empty-state
```

In this example, we can see usage of both `check` and `checkNotNull`. Before returning the value of  `state` it uses: `checkNotNull` to validate that `someState` is not null and then it uses `check` to validate that `isNotEmpty()` doesn't return `false`. If any of these doesn't meet the criteria, it will throw an `IllegalStateException`.  Again, this is a great, idiomatic way of performing validation on state in Kotlin code.

## Conclusion

Kotlin has the standard `assert` function used in most programming languages but that function throws an `AssertionError` exception. There might be times where using `assert` is the appropriate thing to do but there are also times where leveraging `require` and `check` will be more appropriate. By using `require` and `check` we get three things: we are able to validate function arguments and state; we are able to throw particular exceptions depending on the case; we are able to write idiomatic Kotlin code that is clean and readable. `require` and `check`  also have different purposes, as their name indicates than `assert`, which makes using `assert` everywhere unnecessary.

Leveraging `require` and `check` will lead to more secure and clean code but it is not without its drawbacks. By using `require` and `check` prolifically, without considering where they are placed, it can lead to a codebase that is filled with blocks of code that use exceptions to control flow. These functions _should_ be used but use them with caution, where it makes sense. Given that Kotlin has nullable types, it can be difficult to understand where it makes sense to return a nullable type or where it makes sense to throw an exception.

I would say that `require` and `check` should be used where it is _impossible_  to continue flow if the conditions are not met. If there's any way that the flow can continue then `require` and `check` are possibly not the best options. In the examples above, `getIndices` cannot proceed with a negative value because it will only throw a different exception at a later stage, while `getStateValue` can't proceed because it won't be able to return a valid state if the state is empty.