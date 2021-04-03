---
title: "contributing to pytest"
date: 2018-08-16T23:00:00+00:00
draft: false
summary: A tale of contributing to `pytest` and becoming a member of the [pytest-dev](https://github.com/pytest-dev) organization.
aliases: ["/essays/contributing-to-pytest/", "/blog/contributing-to-pytest/"]
categories:
  - python
  - oss
---

A few weeks back, I started collaborating on a project that I had just recently been using. I started writing Python and need a test framework to go along with it. After searching for a bit online, I found out that `pytest` [1] is currently an alternative to the standard `unittest` [2]. By looking at the examples I felt that `pytest` was less verbose and less cumbersome than `unittest` so I started using it and was quickly fascinated. This is a post about how I started contributing to `pytest`. I’ve considered not writing this as it seems to be too personal for people to want to know but this is my blog after all.

Upon starting my usage of `pytest` I started looking around for easy issues I could solve. Since I’m not an expert in Python, nor an average coder in it, to be honest, I started looking around for documentation issues that I might help with. Documentation also seems to be something people always try to get away from, I guessed my help would have been appreciated. I landed on [Fixture docs - possible point of confusion](https://github.com/pytest-dev/pytest/issues/3592) which seemed easy enough. There was a confusing wording in the documentation of the pytest _fixtures_ functionality and there was a need to change that wording. A solution had been proposed and I just started changing the documentation. After I had made my changes, I submitted a [pull request](https://github.com/pytest-dev/pytest/pull/3642). It didn’t go as smoothly as I had planned, unfortunately. I forgot to go through one step of contributing guidelines and it ended up breaking my build. Nonetheless, `pytest`'s maintainers were very understanding and helped me review the PR and fix all the problems. In the end, it got merged in 2 weeks. Given it was a rather large change to the documentation, I was pretty excited.

With that submission resolved I decided to try another one. I submitted another small [pull request](https://github.com/pytest-dev/pytest/pull/3683) to give users a formal way to academically cite `pytest` whenever they used it in academia. It wasn’t that hard and again most of it had been previously defined by the maintainers. Either way, it was very enjoyable to be able to involve myself with the community and give back. That PR has already been merged too, which means that I have made 2 full contributions!

After that, something very interesting happened. I ended up being added as a _Member_ to the [pytest-dev](https://github.com/pytest-dev) organization which means I can now contribute more freely and to more `pytest` projects under that umbrella. This was really unexpected for me since I screwed up that initially PR but, at the same time, it was very exciting to feel that my contributions were of value. This is also an incentive to keep trying to contribute to OSS which is something I’ve been trying to get active at for quite some time.

It is not that I suddenly have a wealth of knowledge on the topic or that I can, out of a sudden, magically contribute to anything but it is important to understand that everything starts with first steps. Those steps then build, incrementally, to form something entirely new that was unexpected. That’s the importance of trying, to me at least. As a closing note, I’m now trying to work around an issue ([`-p` does not pick up value if there is no space](https://github.com/pytest-dev/pytest/issues/3532)). Let’s see if I can make something out of it.

## References

[1] https://docs.pytest.org/en/latest/  
[2] https://docs.python.org/3/library/unittest.html  
