---
title:  "Pytest 6.2.0 is Out!"
date: 2020-12-12
summary: "Celebrating the release of pytest 6.2.0 which includes my first core contribution to pytest: improving pytest's --collect-only output."
authors: Hugo Martins
categories: [pytest, oss, python]
---

I'm not one to write about release notes and release announcements but this one is special for me. [pytest 6.2.0](https://docs.pytest.org/en/latest/announce/release-6.2.0.html) has been released and it includes my [ first core contribution to pytest]({{< ref "/essays/2020/11/improving-pytests-collection-output" >}}) in which I helped improve the output of running `pytest` in collection-only mode. You can see more details about this in [#7875](https://github.com/pytest-dev/pytest/pull/7875). I'm thrilled about this release and I advise anyone with motivation and time to contribute to this project which has such awesome maintainers!

`pytest` 6.2.0 also brings along more awesomeness such as:

- Improved handling of unraisable exceptions and unhandled thread exceptions: [#5299](https://github.com/pytest-dev/pytest/issues/5299).
- Improved verbose mode, showing more information about skipped tests: [#2044](https://github.com/pytest-dev/pytest/issues/2044).

Take a look at the [changelog](https://docs.pytest.org/en/stable/changelog.html#pytest-6-2-0-2020-12-12) with a description of all changes.
