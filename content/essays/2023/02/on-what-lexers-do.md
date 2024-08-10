---
title: "On What Lexers Do"
date: 2023-02-21T15:31:16Z
draft: false
categories: [lexing, lexers, compilers]
authors: Hugo Martins
summary: "On reconnecting with something I was always curious about: compilers and interpreters. It turns out lexers can actually be fun."
---

A couple of weeks ago, I’ve started reading [Writing An Interpreter In Go](https://interpreterbook.com/). Reading this book means I’ll be building an interpreter for a language called `monkey` ([you can follow my implementation](https://github.com/caramelomartins/monkeylang)). It is a topic that I’ve always been curious about but never actually took the plunge and decided to study it.

I’ve dabbled a little bit with it in university. But we never actually wrote any of the internal tools. We always used generators for the code and focused more on the concepts. Rest assured, I can’t remember most of it by now.

While reading this book, I reconnected with the basic concepts of how interpreters and compilers work. I've been taking notes and trying to expand my knowledge with other sources. I’ve always heard that to know something, you need to be able to explain it. I’m trying to do that here.

**Lexers transform source code into symbols (or tokens).** They execute what is called “lexical analysis”. From [Writing an Interpreter in Go](app://obsidian.md/Writing%20an%20Interpreter%20in%20Go):

> (...) from source code to tokens, is called "lexical analysis", or "lexing" for short. It's done by a lexer (...)

Wikipedia [defines](https://en.wikipedia.org/wiki/Lexical_analysis) “lexical analysis” as _“the process of converting a sequence of [characters](https://en.wikipedia.org/wiki/Character_(computing) "Character (computing)") (…) into a sequence of _lexical tokens_ (…)”_. This aligns well with what we read in the book.

So, when we execute a lexer, we transform the source code (_text_) into a series of data structures (_tokens_). It is an integral component of writing interpreters (or compilers) as we need to transform source code into a representation that we understand and can process in later stages (e.g. parsing).

As an example, in Go, if we have the statement `str := "string"`, we are in the presence of three tokens: an identifier token (`str`), an assignment token (`:=`) and a string token (`string`).  **Tokens are meaningful data structures**, allowing us to approach source code as data structures, rather than dealing with text inside of interpreters.

Lexers also help us ignore elements of source code that aren't relevant (e.g. whitespaces in some languages) and focus just on the elements that actually matter to us. After lexing, all irrelevant whitespace has been stripped off. For example, in C or Go, I wouldn’t expect whitespace to appear as tokens after lexing. In Python, where whitespace is relevant, I expect it to be taken into account in lexing.

One concept I was particularly interested in, as I read, was the fact that **lexers need to look-ahead**. This isn’t always done the same way, it depends on the approach used, but it is integral to lexing, to be able to resolve conflicts.

As an example, reading a “=“ sign might represent different things. It can be an equal sign (e.g. assignment) or it can be the start of a comparison (e.g “\==“). Without looking ahead, it is impossible to resolve conflicts and accurately know what a given character represents in source code.

Lexical analysis is only but a small step, in the process of interpreting source code and its results, that will need to be fed into a parser, which will perform additional steps with the tokens.

It has been a good experience reading these initial sections of [Writing An Interpreter In Go](https://interpreterbook.com/) and finally feel I’m starting to understand the basics of these concepts.
