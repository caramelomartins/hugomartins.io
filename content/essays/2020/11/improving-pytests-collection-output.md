---
title:  "Improving pytest's --collect-only Output"
date: 2020-11-07
summary: "A small account of contributing with improvements to pytest's 'collect-only' functionality. I have made a few changes in the way pytest processes the results of test collection, when using 'collect-only', based on feedback from an issue that was reported to pytest."
authors: Hugo Martins
categories: [pytest, oss, python]
---

In this essay, I want to delve deeper into one of the pull-requests I opened in [this year's hacktoberfest](https://hugomartins.io/essays/2020/10/halftime-hacktoberfest-2020/). `pytest` is a great project, for which I have contributed with a few documentation improvements, particularly in the fixtures section, which can been seen in [pytest-3.6.4](https://docs.pytest.org/en/latest/announce/release-3.6.4.html) and above. I have tried contributing to  `pytest` because I've been a happy user for a few years, this project is important in Python's ecosystem and I want to have Python-based projects to practice on, given that I am now doing more Kotlin and Go professionally.

## What's in a Problem

On that note, I went through `pytest`'s issues a few weeks ago and found an issue that was perfect for me to get my feet wet in `pytest`'s codebase. Issue [#7701](https://github.com/pytest-dev/pytest/issues/7701) mentioned that the output of using `pytest` with `--collect-only` is confusing for users and it requires users to scroll all the way to the beginning of tests' execution to get the full picture of what was collected.

Why does this happen? Currently, when using `--collect-only`, `pytest` only outputs the number of tests deselected and the amount of time it took to deselect them. As an example:

```shell
# Taken from examples in #7701.

# pytest --collect-only
==== no tests ran in 1.66s ====
# pytest --collect-only -k filter
==== 544 deselected in 1.67s ====
```

This misses some crucial information such as: how many tests were found and how many tests were collected. Furthermore, to complicate things, the output says _"no tests ran"_ if it doesn't collect any tests, which is confusing because `--collect-only` shouldn't ever run tests.

## A Solution

In order to make the output less confusing, the reporter suggests that there should be a bit of the output that displays the number of selected tests, such as:

```shell
# Example from #7701.

# pytest --collect-only
==== 123 selected, no tests ran in 1.66s ====
# pytest --collect-only -k filter
==== 123 selected, 544 deselected in 1.67s ====
```

After some discussion, between me, the reporter and one of `pytest`'s maintainers, we reached the following conclusions:

* We need to display the number of selected and deselected tests.
* We need to display the total number of collected tests.
* We should remove `no tests ran` from the output.

We ended up with the following suggestion: 

```shell
=== 10/50 tests found (40 deselected) in 1.23s ===
=== 50 tests found in 1.23s ===
```

This would convey the correct information about what tests have been collected, selected and deselected at the end of the execution, making it very easy to read this information. It is also a compressed and intuitive version for what the `--collect-only` functionality stands for. Additionally, this will also be applicable if we use the `-k` option to filter out tests while using `--collect-only`.

## What Has Been Made

Given that I understood the issue, I understood where in the codebase this needed changing and I wanted to contribute, I opened PR [#7875](https://github.com/pytest-dev/pytest/pull/7875) with changes to improve output of `--collect-only`. Apart from changes to documentation and modifying the tests to match the new behavior, I have made changes to `src/_pytest/terminal.py` in function `build_summary_stats_line`, adding the following logic (so far), which is specific to `--collect-only`:

```python
        if self.config.getoption("collectonly"):

            if self._numcollected == selected:
                co_output = "%d %s found" % _make_plural(self._numcollected, "test")
            else:
                deselected = self._numcollected - selected
                co_output = f"{selected}/{self._numcollected} tests found ({deselected} deselected)"

            parts = [(co_output, {main_color: True})]

            # Sticking with "0/0 tests found (0 deselected)" would be confusing.
            if self._numcollected == 0:
                parts = [("no tests found", {_color_for_type_default: True})]

            # Sanity check for errors that might have ocurred. Otherwise, it
            # will never let the user know that an error ocurred during
            # collection.
            if errors:
                color = _color_for_type.get(key, _color_for_type_default)
                markup = {color: True, "bold": color == main_color}
                parts = [("%d %s" % _make_plural(errors, "error"), markup)]
```

It is still under review and it might take a bit of time until it gets merged because there are a lot of PRs in `pytest` but I believe this is a good contribution to the project and it was an interesting challenge to go through.

`pytest` also has an incredible set of code maintainers that are mostly proactive and will guide you through PRs and issues, in order to make it as easy as possible for you to contribute. It eases a lot of the anxiety attach to beginner contributions in the open-source ecosystem.
