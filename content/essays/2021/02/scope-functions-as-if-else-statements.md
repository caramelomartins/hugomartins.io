---
title: "Don't Abuse Kotlin Scope Functions as an If-Else for Null Checks"
date: 2021-02-09
draft: false
summary: An exploration behind the reasons why we shouldn't use statements based on scope function, in particular `let`, as if-else checks to validate nullability in Kotlin.
authors: Hugo Martins
categories: ["kotlin"]
---

I've been trying to understand more about Kotlin and I've written about Kotlin topics more lately, such [using `require` and `check` functions in Kotlin]({{< ref "http://localhost:1313/essays/2021/02/using-require-and-check-in-kotlin" >}}). This time I want to write about a pattern I've been thinking about: abusing scope functions, in particular `let`, and using them as if-else for null checks. 

Now, Kotlin is a very powerful language and I've been enjoying it more and more. Its use of functional constructs provides developers with power and flexibility that goes way beyond some languages out there - I had been programming in Python and Go previously. On the other hand, Kotlin is a fairly new language, it has about 10 years, needing some more time to mature, which brings along some dangers because usage of particular constructs can have its perils, without us even realizing.

One such example is something I've come to see more and more with time: using `a?.let {} ?: (...)`, where `a` is a given object and `(...)` represents another statement. Another variation of this is `a?.let {} ?: run {}` but, with this variation, you could have a more viable justification to write it, as `run` is a function that can execute multiple statements. Now, by themselves, these aren't particularly dangerous, as we can have multiple use cases that justify them. I've use them before a lot of times.  `let` can be extremely flexible and really improve the readability of a block of code, improving on the way we can reliably check for null values. Nonetheless, using `let` can be particularly dangerous when we have `a?.let {} ?: (...)` written as a substitution of a simple if-else statement for null checks.

Kotlin documentation suggests that `let` ["is often used for executing a code block only with non-null values"](https://kotlinlang.org/docs/reference/scope-functions.html). This can be achieved by using the null safe call operator `?`. Great! Now we know that we can use `let` to validate that a given statement or object _is not null_ and then we just have to use the Elvis operator (`?:`) to execute another statement, if the initial statement actually is null. This seems promising...we have found an idiomatic substitution for if-else statements for performing null checks.

Unfortunately, not so fast. Although it might seem as `a?.let {} ?:  (...)` is a good substitution of if-else for checking for null values, we will quickly find out that it actually doesn't behave the same as an if-else statement. When we use an Elvis operator, the Elvis operator will return the right-side operation in case the left side operation returns null. What if `a?.let {}` returns null? If `a?.let {}` returns null, the right-side of the Elvis operator will still execute. This means that we have a potential situation where all branches are executed: the statements inside `a?.let {}` as well as the right-side of the Elvis operator. This would be similar to having an if-else statement where both the `if` _and_ `else` can be executed _in a single run_, which is definitely not the correct behavior most of us are expecting from an if-else statement.

As a practical example, let's consider the following code:

```kotlin
fun main() {
    // nullString is null.
    val string = "non-empty"

    // Evaluate nullString with "let" and Elvis operator.
   	val one = string?.let { null } ?: "another-string"

    // Evaluate nullString with if-else.
    val two = if(string != null) string else null
    
    println(one)
    println(two)
}
```

In this case, `one` stores the value of validating if `nullString`  is null with `let` and an Elvis operator, while `two` stores the value of validating if `nullString` is null with an if-else statement. If we run this, we will get the following output:

```
another-string
non-empty
```

As we can see, even when `string` isn't null we might not necessarily get the value returned by `let`  if that value is actually null. What happened above is that the lambda result from `let` was null so a third value was used, neither `non-empty` nor null, because the way the code was structured allowed for having three branches in the code.

It is not that the `a?.let ?: (...)` structure is wrong on its own but rather that the mental model we have constructed of it executing only one side of the Elvis operator is incorrect. By abusing the use of this construct, in a lot of situations where we could've written a simple if-else statement, instead of writing more idiomatic code, we might simply be writing incorrect code. For _a lot_of situations `a?.let ?: (...)` is appropriate. It is more idiomatic, it makes for cleaner and more readable code _but_ don't ever forget that it _is not_ a replacement for if-else statements if all we are doing is validating that a given statement is null, and executing _another_ statement in case it is.