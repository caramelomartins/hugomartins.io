---
title: "Halftime at Hacktoberfest 2020"
date: 2020-10-15
summary: "A small recap of what I've been up to during Hacktoberfest 2020 so far." 
authors: Hugo Martins
categories: [hacktoberfest]
---

Even though [shitoberfest](https://twitter.com/shitoberfest) threatened to ruin Hacktoberfest 2020, I still pushed forward and I'm participating in it, as usual. I've written about my participation in [2016](https://hugomartins.io/essays/2016/10/hacktoberfest-the-end/) and [2019](https://hugomartins.io/essays/2019/12/minor-followup-on-hacktoberfest/). In both I have failed (0 PRs in 2016 and 3 PRs in 2019), I have succeeded in 2018 and it has introduced me to a few open-source projects - for example, `pytest`.

We are now halfway into Hacktoberfest 2020 and I have submitted 2 PRs - one already approved and another under review. To start, I submitted a quick PR to a project I maintain ([awesome-linters](https://awesome-linters.hugomartins.io/)) to resolve a simple grammatical error that a user spotted. Since the user didn't want to open the PR, I ended up creating the PR and accepting it as an Hacktoberfest contribution - all in all I have to maintain it so I thought it _kind of made sense_.

For a second PR, which is still under review, I tried to contribute something more substantial. Even though it is still a beginner-friendly PR, [this PR](https://github.com/pytest-dev/pytest/pull/7875) I opened in `pytest` aims to improve the reporting of the tool when it is executed with `--collection-only`. A user reported that [it was a bit confusing](https://github.com/pytest-dev/pytest/issues/7701) and I tried to understand where the reporting issue came from and "fixed it". I hope his small 29 LOC PR gets accepted, as it will be my first PR to `pytest`'s core - I made a few contributions to documentation but not to the core codebase.

Let's see what I can still do with the rest of Hacktoberfest 2020 but I am already happy about this core contribution to `pytest`. 
