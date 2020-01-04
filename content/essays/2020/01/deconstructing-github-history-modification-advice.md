---
title: Deconstructing Github's History Modification Advice
date: 2020-01-07
draft: true
summary: ""
authors: Hugo Martins
categories: []
---


Github has a tutorial in which it explains how people can [change commit author information](https://help.github.com/en/github/using-git/changing-author-info). This is supposed to be used when things go wrong massively, as it is destructive, rewriting repository history, and it is considered a bad practice. Nonetheless, their advice is to run the following snippet on a bare repository:

```
#!/bin/sh

git filter-branch --env-filter '

OLD_EMAIL="your-old-email@example.com"
CORRECT_NAME="Your Correct Name"
CORRECT_EMAIL="your-correct-email@example.com"

if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_COMMITTER_NAME="$CORRECT_NAME"
    export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
fi
if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_AUTHOR_NAME="$CORRECT_NAME"
    export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
fi
' --tag-name-filter cat -- --branches --tags
```

I have used it before on personal repositories and I enjoy understanding what I am doing. We should Github's advice of only doing this in an emergency...but, if you really have to use it, it is relevant to know how it works! So, how what does this do?

Starting with the basics, a bare repository, usually cloned with the `--bare` flag, is a repository that doesn't have a working tree. It has no source code files. It only has git commit objects, revision history and references, for example. The reasoning behind this is that we are only going to be working on git's internal information, rather than the source files so there's no need to be cloning the entire repository.

Next we have the `filter-branch` command. According to its [documentation](https://git-scm.com/docs/git-filter-branch), it allows you to _"rewrite Git revision history by rewriting the branches mentioned in the <rev-list options>, applying custom filters on each revision."_ It seems appropriate, given that we want to modify author information, to use something that allows us to rewrite our revision history, and associated metadata. An incredible part of `filter-branch` is that it allows you to modify only parts of branches, while keeping others intact...

We execute `filter-branch` with `env-filter`, `tag-name-filter` and `--`. `--env-filter` allows you to modify information about the environment in which the commit is (or better, was) executed, particularly through modifying environment variables. `--tag-name-filter` allows us to update tags that were pointing at rewritten objects. As the documentation explains, by using `--tag-name-filter cat` we are simply updating the tag references without modifying their names. `--` separates the options for `filter-branch` from the options used for `rev-list`, which is called internally by `filter-branch`. `rev-list` is used here to filter commit objects (branches and tags, for example) that need to be rewritten. By using `--branches` and `--tags`, we are essentially forcing `filter-branch` to go through all the branches and tags in the repository and rewrite them, based on the filters passed to `filter-branch`.

Now, what about the snippet passed to `--env-filter`?


```
OLD_EMAIL="your-old-email@example.com"
CORRECT_NAME="Your Correct Name"
CORRECT_EMAIL="your-correct-email@example.com"

if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_COMMITTER_NAME="$CORRECT_NAME"
    export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
fi
if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_AUTHOR_NAME="$CORRECT_NAME"
    export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
fi
```

This snippet checks, for every single commit object, that is passed to the `filter-branch`, if the `GIT_COMMITTER_EMAIL` or `GIT_AUTHOR_EMAIL` have the incorrect email (`OLD_EMAIL`). If they have the incorrect email it will swap them for the correct email (`CORRECT_EMAIL`) and name (`CORRECT_NAME`).

Back to our original question, what does this snippet do? In essence, it goes through all the commit objects, in all branches and tags on a git repository, and replaces their existing committer and author information for updated information. By pushing the updated repository to a remote repository, it will force an history rewrite, removing the incorrect emails from it. 
