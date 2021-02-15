---
title: "How To Pass Multiple Parameters To Same Value in Query Strings"
date: 2021-02-16
draft: false
summary: "A quick exploration of RFC3986 (\"Uniform Resource Identifier (URI): Generic Syntax\") Section 3.4."
authors: Hugo Martins
categories: ["http", "rfcs"]
---

While writing REST APIs, I've frequently needed to pass multiple values for the same parameter in a query string. In particular, this happens when using `GET` requests and we want to, for example, pass a filter of identifiers to the particular request. At first, it appears simple but it is actually undefined behavior in terms of what the HTTP standards specify. For clarity, a query string is a string attached to an HTTP URL specifying values for particular parameters, allowing us to pass data in a request. For example, in `https://example.com/api/examples?id=123`, the string `?id=123` would be a query string.

It seems to be a [frequently](https://stackoverflow.com/questions/24059773/correct-way-to-pass-multiple-values-for-same-parameter-name-in-get-request) [asked](https://stackoverflow.com/questions/13261377/how-to-pass-multiple-values-for-a-single-url-parameter) [question](https://stackoverflow.com/questions/50820245/pass-multiple-values-of-a-http-request-parameter) in Stack Overflow. It turns out, apparently, that there's no standard for how parameters that can have multiple values should behave in HTTP.

With query parameters, multiple values can be represented as `value=1&value=2` or `value=1,2`. This will come down to what the applications receiving the requests will accept. [RFC 3986](https://tools.ietf.org/html/rfc3986) has no definition on how to behave when the same parameter has multiple values. It neither recommends one option or the other.

This means that the behavior of the query parameters will be defined by the API - they can either accept `value=1&value=2`, `value=1,2` or both. It is the wild west of HTTP. It can also be defined by the framework powering the API in question, which can define how it supports multiple parameters. For example, in Python, Django has built-in support to handle this via `django.http.QueryDict.getlist()` while in Flask, there's a need to modify the way parameters are processed in order for the parameters to be appropriately parsed.

As a personal preference, I would go for using multiple `key=value` pairs because it becomes more readable when reading the request. If the framework supports it, the fact that it becomes more readable can be important while troubleshooting issues with requests such as these.