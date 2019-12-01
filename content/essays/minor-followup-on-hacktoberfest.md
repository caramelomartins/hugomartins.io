---
title: "Minor Follow-up on Hacktoberfest 2019"
date: 2019-12-01
draft: false
summary: ""
authors: Hugo Martins
categories: ["oss"]
---

In a previous essay I wrote about the process behind my Hacktoberfest 2019 contributions. It is now worthwhile to make a quick follow-up, at this point, at the status of those contributions. <!--more-->

It is relevant to start by mentioning I made a bit of a mess of my  contributions, right after I wrote up the article. I tried to  correct an issue with the email and user of the contributions and re-wrote the history of my git repositories...bummer! That forced me to close my initial PRs and having to  open up new ones - and re-writing all the changes I had made.

I had opened [PR #4476](https://github.com/pyinstaller/pyinstaller/pull/4476), [PR #4499](https://github.com/pyinstaller/pyinstaller/pull/4499) and [PR #4500](https://github.com/pyinstaller/pyinstaller/pull/4500). PR #4499 had to be closed and re-opened as [PR #4516](https://github.com/pyinstaller/pyinstaller/pull/4516). PR #4500 had to be closed and re-opened as [PR #4517](https://github.com/pyinstaller/pyinstaller/pull/4517). That makes a total of 5 PRs, of which only 3 were actually valid for review by PyInstaller's maintainers.

PR #4476 was accepted, within a two-week period. This seems  to be a fairly usual time frame, by looking at previous PR reviews in the  project. There was a minor wrapping issues, which the maintainers of  PyInstaller promptly resolved and pushed to my branch. PR #4516 was merged within approximately the same two-week period, without any need for further modification.  PR #4517 took a bit longer to merge because I had made an error when  creating the changelog entry - plus there was a need for a small lint  correction.

I was very happy with the end result, getting all 3 PRs approved and  merged. I was surprised with how friendly the maintainers of PyInstaller  were, even with the small errors. I was also thankful they completely ignored my shenanigans of duplicating the PRs. 

They should be an example for the  entire community, how they deal with new contributors and  their mistakes. That is one of the reasons why they have 280+ contributors  on Github.

Now, looking towards the future, I hope I can be of more use to them by  adding more hooks. Adding hooks seems to be something that I can do, without creating a lot of work for the maintainers in terms  of reviews. But , it will also allow me to start understanding more of the codebase and contribute in different  areas.
