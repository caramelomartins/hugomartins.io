---
title: "What If Kotest Had HTML Reports?"
date: 2021-03-23
draft: false
summary:  "Another story about a small contribution to open-source. This time, I've opened a pull request in Kotest to implement generation of HTML reports."
authors: Hugo Martins
categories: [ kotlin, kotest ]
---

This is a story about contributing back to the open-source community. I've written other stories such as this before, namely: [Improving pytest's --collect-only Output]({{< ref "/essays/2020/11/improving-pytests-collection-output" >}}), [PyInstaller + PyFiglet = Trouble]({{< ref "/essays/2019/06/pyinstaller-pyfiglet-trouble" >}}) and a few [reports]({{< ref "/essays/2020/10/halftime-hacktoberfest-2020" >}}) [on]({{< ref "/essays/2019/12/minor-followup-on-hacktoberfest" >}}) [participating]({{< ref "/essays/2019/11/giving-back-with-hacktoberfest-2019" >}}) in Hacktoberfest. The aim of these stories isn't to brag but rather to explore the small steps I took to give back to the open-source community and potentially inspire others to do the same. I must note though that it only makes sense to give back when it means something to you and when you appreciate the projects. There's no reason to do so otherwise. I do not agree with any culture of _having to_ contribute to open-source projects, or having to code after work. It feels non-sensical to expect as much from everyone, each to its own hobbies. You do you.

I seem to have a knack for working on software that is related to testing in some way. As I've mentioned earlier, I've made two or three contributions to [pytest](https://docs.pytest.org/en/stable/index.html) because I appreciate it, due to a season of working with Python. This time, I'm writing about [Kotest](https://kotest.io/), which is a _"multiplatform test framework, assertions library and property test library for Kotlin"_. In a similar way to what happened with pytest, I've been drawn to Kotest due to recent work with Kotlin - as you might have noticed from so [many]({{< ref "/essays/2021/02/using-require-and-check-in-kotlin" >}}) [essays]({{< ref "/essays/2021/02/scope-functions-as-if-else-statements" >}}) [about]({{< ref "/essays/2021/03/difference-between-val-and-var-kotlin" >}}) [Kotlin]({{< ref "/essays/2021/03/understanding-differences-between-list-and-mutablelist-in-kotlin" >}}) lately.

Before moving on, I just want to say that even if you don't want to use Kotest as a testing framework (although you definitely _should_ check [Kotest's testing styles](https://kotest.io/docs/framework/testing-styles.html)), Kotest has an incredible and diverse library of assertions which you can use _without_ having to use the core framework. In fact, I've started using Kotest because of its assertions and only later moved on to using its core functionality.

Kotest allows you to write tests in a variety of [testing styles](https://kotest.io/docs/framework/testing-styles.html), which are essentially different approaches to implementing tests. For the usual JUnit-style tests you'd use `AnnotationSpec` but you have a few more that you can use to emulate different approaches in Kotlin such as: `BehaviorSpec` to emulate BDD-style tests, `DescribeSpec` for those familiar with Ruby or Javascript styles, or `FunSpec` for those familiar with Scala. Kotest also offers some styles created specifically for the test framework.

If you use Kotest, and you use any of the testing styles apart from `AnnotationSpec`, you might notice that your Gradle HTML reports start to get a little funky. As an example, consider the following test, written in `BehaviorSpec`:

```kotlin
// Example from: https://kotest.io/docs/framework/testing-styles.html#behavior-spec.

class MyTests : BehaviorSpec({
    given("a broomstick") {
        `when`("I sit on it") {
            then("I should be able to fly") {
                // test code
            }
        }
        `when`("I throw it away") {
            then("it should come back") {
                // test code
            }
        }
    }
})
```

Once you execute this, you might quickly realize that Gradle HTML reports, from your execution of the tests, will show up 5 tests...*wait, what?!?* Yes, Gradle will interpret these results from executing these tests as having ran 5 tests, which is wrong. Kotest has a smart way of handling this, by leveraging its extensions with a listener specifically built to properly [generate XML test results](https://kotest.io/docs/extensions/junit_xml.html) of Kotest tests that aren't based on `AnnotationSpec`: `JunitXmlReporter`. This listener works perfectly if all you want is to send your XML test results to a CI, or another system, but it doesn't work if you want to display them locally. Somehow, Gradle ignores the XML produced and uses its own binary representation of the results to generate the HTML reports.

Since I don't want to mess with Gradle, but I rather enjoy Kotest and Kotlin, I reached out to Kotest's maintainer about this situation and about potential solutions. Apparently, there were none.  After a quick chat, I realized that _I_ could _try_ to implement this in Kotest and I got to work, slowly.

I thought to myself, what if Kotest had HTML reports? While it might not be the most important feature, and it isn't, being able to analyze your test reports locally via HTML is more productive than doing so via XML. You'll be able to quickly inspect which tests are failing. Potentially, not a lot of people actually need to analyze their tests via HTML, there are plugins for Intellij Idea after all, but each of us has its own way of working, and I appreciate seeing the HTML reports locally. Who knows, maybe someone will one day want to put these reports on their internal server for an entire team to see? If they _do_ want that, Kotest would eventually be able to support it and that would have a cool contribution to Kotest.

After more than two months of scattered work on this, whenever I had a bit of time to spare, I've submitted a PR [#2130](https://github.com/kotest/kotest/pull/2130) in Kotest which should resolve [#2011](https://github.com/kotest/kotest/issues/2011). As with pytest, I have nothing but good things to say about Kotest's maintainer, who's been kind and patient through these two months. We've decided to implement this as another Kotest listener that will generate HTML reports based on results from `JunitXmlReporter`, which means that Kotest will eventually be able to generate both XML and HTML reports for tests. It is still basic and raw but it outputs the essential information about successes, failures and errors in executions of tests.

I've had a lot of fun implementing this, learned a ton, got to practice some Kotlin, and implemented something I wanted to see in an open-source project that may potentially help other people. That's good enough for me.