---
title: "Giving Back with Hacktoberfest 2019"
date: 2019-11-04T10:08:42Z
draft: false
summary: Hacktoberfest 2019 just finished and this is my personal story of achievement and failure, all at the same time. I only started Hacktoberfest halfway into October but I still achieved 3 PRs. I am proud of that outcome though.
aliases: ["/blog/giving-back-with-hacktoberfest-2019/"]
categories:
- oss
---

Hacktoberfest 2019 just finished and this is my personal story of achievement and failure, all at the same time. I only started Hacktoberfest halfway into October but I still achieved 3 PRs. I am proud of that outcome though.

After having missed Hacktoberfest in 2018, I have decided to try it again in 2019. I didn’t try it in 2018 due to being in the middle of [writing my thesis](https://hugomartins.io/research/). In 2017, I successfully  completed the challenge by submitting pull-requests to [CFDocs](https://github.com/foundeo/cfdocs) and [Nextcloud Docs](https://docs.nextcloud.com/). In 2016, I [registered]({{< ref "/essays/2016/09/let-hacktoberfest-begin" >}}) but [didn’t attempt]({{< ref "/essays/2016/10/hacktoberfest-the-end" >}}) any pull request.

I’ve been more active in the open source community lately, even though I’m still a bit shy. I’ve been commenting more on issues, suggesting fixes and opening pull requests to resolve documented issues in some projects. Most notably, I have made some [small documentation patches to pytest]({{< ref "/essays/2018/08/contributing-to-pytest" >}})) and attended [FOSDEM 2018]({{< ref "/essays/2019/02/fosdem-2019" >}}). I thought that this year I would try to contribute with something more than simply fixing typos or documentation - nothing against that but it was the objectives I set for myself.

## Choices, So Many Choices

In order to be able to contribute with more than simple typo fixes, I thought that the best approach would be to go through some of the projects I use because *(1)* I understand them better which makes it easier to contribute to and *(2)* I have used them and it feels more intuitive to contribute to something I know and use.

I was faced with a lot of choices (even looked at Hacktoberfest’s suggestions, such as [Up For Grabs](https://up-for-grabs.net/#/) and [CodeTriage](https://www.codetriage.com/)) but choosing is never easy…at least when you need to balance your knowledge and skills, with the needs of the projects.

Some projects that I evaluated for potential contributions, all of them I have used somehow - more or less - throughout last year:

- [asciimatics](https://github.com/peterbrittain/asciimatics): I used this a bit this year, for a small component, but couldn’t find any issue that I thought matched my knowledge.
- [pytest](https://github.com/pytest-dev/pytest): This would be a no-brainer. On one hand I am already a member of the pytest-dev contributors’ team and I used it extensively, on the other hand I couldn’t find something that I thought I could tackle in a short amount of time.
- [PyInstaller](https://github.com/pyinstaller/pyinstaller): PyInstaller looked promising. I have used it a lot this year and I have even commented on some issues, [helping find a workaround for an known issue](https://github.com/pyinstaller/pyinstaller/issues/2389#issuecomment-476414044) .  PyInstaller’s maintainers are also extremely generous to newbies, help them through the process of making pull-requests and their documentation is incredibly explicit. It seemed that I could help by implementing custom hooks, suggested by other users in existing issues.
- [click](https://github.com/pallets/click): I used this extensively for creating some CLIs but I had some trouble understanding which of their issues would be good first issues, which made me a little skeptical on tackling a specific issue. I also couldn’t understand how they triage issues, which made it harder to choose an issue.
- [Kubernetes](https://github.com/kubernetes/kubernetes): This was a stretch but I thought that, since they have dozens of issues tagged as *”good first issues"* I could browse around and see if I found something I could help with. I wouldn’t mind if I contributed to documentation for Kubernetes, given that I think it is a good project, which I have come to use a lot. It is also written in Go - a big plus.
- [Hugo](https://github.com/gohugoio): Written in Go - a plus. Another project I used frequently, as it powers some pages I use for documentation and it powers my own website and blog. I couldn’t find any issues that seemed easy enough for me to contribute for the first time and all *good first issue* issues were already taken…

I browsed through some more projects on Github - particularly in [awesome-for-beginners](https://github.com/MunGell/awesome-for-beginners) - but these are the ones that I have taken notes on, so they were the ones that most stand out to me when I was searching. 

It is important to understand that my objectives were to find projects that *(1)* were maintained, had clear guidelines and contribution processes, *(2)* I used frequently in the past year and *(3)* had issues that weren’t typos and documentation but were simple enough for an entry-level contribution. It would be a big plus if they happen to be written in either Python or Go, as those are the two languages with which I am more proficient now.


## Contributions

In the end I ended up making 3 contributions to PyInstaller, which I will detail below.

**Contribution #1 -** [PyInstaller#4476](https://github.com/pyinstaller/pyinstaller/pull/4476):  A very simple change on documentation for working with binary files that are not uploaded automatically by PyInstaller. Particularly, it improves documentation on on how binary files will be structured in the resulting package. Resolves [#4283](https://github.com/pyinstaller/pyinstaller/issues/4283). 

The existing documentation wasn’t expressive enough and induced users in error. As explained in the issue, the documentation seemed to suggest that the syntax should be `binaries = [ (<binary path>, <destination path>) ]` , where a user could define `<destination_path>`. In fact, when adding binary files, users should assume that they can specify only the `<destination_folder>` in which the binary files will be stored inside the package. 

I used the suggestion by PyInstaller’s maintainers as a starting point:


> You could adopt (correct) the example, maybe even add a second one, and refer to the [Adding Data Files](https://pythonhosted.org/PyInstaller/spec-files.html#adding-data-files) section for details.

**Contribution #2 -** [PyInstaller#4499](https://github.com/pyinstaller/pyinstaller/pull/4499)**:** New built-in hook for `django-babel`. Resolves [#4281](https://github.com/pyinstaller/pyinstaller/issues/4281).

The issue here was that `django-babel` uses `pkg_resources.get_distribution` in their `__init__.py`, without having imported `django-babel` explicitly. This means that when freezing, PyInstaller doesn't import `django-babel` and therefore raises exceptions when executing the resulting package.

**Contribution #3 -** [PyInstaller#4500](https://github.com/pyinstaller/pyinstaller/pull/4500)**:** New built-in hook for `pyarrow`. Resolves [#3720](https://github.com/pyinstaller/pyinstaller/issues/3720).

In this case, another user had submitted a PoC version of the code but didn’t want to submit a PR and maintainers hadn’t resolved the issue yet. I ended up submitting a slightly modified version of the initial code. I removed `pyarrow.formatting` and `pyarrow.compat.*` from the hidden imports as PyInstaller was alerting that those modules could not be found after installing `pyarrow`.

I searched for more contributions to make but I couldn’t find any I thought I was capable of resolving, in the projects I had an interest in or had used before. I could have made another pull-request in one of my own repositories but I didn’t feel that would be in the spirit of the challenge, to be perfectly honest.


## Conclusions

I didn’t complete the challenge but I accomplished most of my objectives. I intended to make contributions to open-source projects that I used and that I felt I could contribute to. In the end, although I didn’t officially complete the challenge, I am happy with the results and with the time and effort put into it.

I would like to advise anyone participating in the future edition of Hacktoberfest to start early. One of the main obstacles I faced was that most of the issues tagged as *good first issue* or similar, in the projects I was interested in, were already taken or resolved. This tells me that people that started earlier resolved those issues. 

If you have that possibility, try to remember if _you_ found any issue in any of the projects you use. If it impacts you, even better. It will make it easier for you to have something to contribute while boosting your motivation to resolve the issue.

On a separate note, as I said above, I could have opened a pull-request on one of my projects but I felt that was against the spirit of the challenge - that is only my opinion, in particular, for my own purposes. I think the most important thing is to define a set of objectives that one feels comfortable with. Once those objectives are established, I don’t believe adding a pull-request for the sake of adding a pull-request is worthwhile. In the near future, I wish to I keep contributing to open-source projects that I have used previously and that I feel good to contribute to, for the right reasons, not just to add one more pull-request to my belt.

With all of this being said, **congratulations** to everyone that successfully completed the challenge and to all the first time contributors that went out of their way to make a contribution. It is always never racking and should be cherished. Keep hacking away!
