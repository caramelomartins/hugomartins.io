---
title: "Abstractions can be expensive"
date: 2024-08-10T00:00:00
draft: false
categories: [performance, complexity]
authors: Hugo Martins
summary: "Reflecting on layers of abstractions and their inherent complexity."
---

Daniel Lemire wrote in *[Performance tip: avoid unnecessary copies](https://lemire.me/blog/2024/06/22/performance-tip-avoid-unnecessary-copies/)*, comparing [Bun](https://bun.sh/) to [Node.js](https://nodejs.org/en), about a particular case where copying data becomes expensive:

> It turns out that the copy was not happening as part of base64 decoding but in a completely separate function. There is an innocuous function in Node.js called  `StringBytes::Size` which basically must provide an upper on the memory needed by the `Buffer.from` function.

As shown in the benchmarks, in the piece, Node.js becomes much slower than Bun when computing the same thing (e.g decoding base64). Even though both of these runtimes use the same library, an unsuspicious copy of data deep in the code of Node.js made it more expensive and slower.

I’ve encountered a similar situation in Go. A service was misbehaving and using too much memory. Profiling and investigating, I narrowed it down to the fact that it was using [io.ReadAll](https://pkg.go.dev/io#ReadAll) to transform a response into a slice of bytes (`[]byte`). In itself, there is nothing wrong with this idea *but* there’s something particular about `io.ReadAll` ([source]([https://cs.opensource.google/go/go/+/go1.22.5:src/io/io.go;l=709](https://cs.opensource.google/go/go/+/refs/tags/go1.20.14:src/io/io.go))):

```
func ReadAll(r Reader) ([]byte, error) {
	b := make([]byte, 0, 512)
	for {
		if len(b) == cap(b) {
			// Add more capacity (let append pick how much).
			b = append(b, 0)[:len(b)]
		}
(...)
```

Inside a `for` loop, `ReadAll` will validate that the buffer it created (*512 bytes*) has enough capacity to store the data. If it doesn’t, it duplicates the size of the existing buffer. This happens until the buffer has enough space to store all of the data. It inherently means we will use *a lot more memory* than the original size of the data. All of this might be unexpected.

If we don’t know the size of the data *a priori*, there’s not much we can do to reduce this (*I believe*) but, if we do know the size of the buffer, [io.Copy](https://pkg.go.dev/io#Copy) might be a good alternative. It allows you to specify the size of the buffer, before writing data to it, avoiding the memory-intensive capacity increases.

This was true up until, at least, Go 1.20.4. In recent versions, the implementation has changed. But it doesn’t change the underlying fact that this abstraction could potentially be expensive memory-wise, or even contribute to a program performing poorly or misbehaving. As Lemire points out:

> The story illustrates why our software is slower than it should be. We have layers of abstractions to fight against. Sometimes you win, sometimes you lose.
>
> These layers are there for a reason, but they are not free.
>
> To make matters worse… these abstraction layers often thicken over time… and the friction goes up.

It is not that the layers are unnecessary: they do good work. They allow us to avoid reimplementing the same things over and over again. They allow us to build on top of several individuals’ expertise and they support frameworks that allow us to be more productive...but **they are not always free**, there’s a cost associated with these abstractions. And it is important to keep an eye (_and monitoring_) open to the possibility that sometimes, only sometimes, they are more expensive than we are willing to accept.
