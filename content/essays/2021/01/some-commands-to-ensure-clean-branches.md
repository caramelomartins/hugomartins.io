---
title: "Some Commands to Ensure Clean Branches"
date: 2021-01-05
draft: false
summary: A Git sub-command and a Git alias I use on a daily basis to help me keep sane and organize local copies of Git repositories.
authors: Hugo Martins
categories: [git]
---

I like clean environments whenever I'm working. I hate mess and I hate messy `git` repositories even more. Because of this, I'm constantly looking at ways to clean up local copies of git repositories. Cleaning up frequently helps me keep my sanity and it helps ensure I'm leveled with remote copies, ensuring that work goes smoothly and faster, without any _operational_ issues with `git`.

I do this mostly with a custom `git` sub-command and a `git` alias: ` clean-branches` and `pbr`. I wrote both of these with a lot of help from StackOverflow folks because I'm not, _at all_, an expert on `git`. So, what do these two do? `clean-branches`  iterates through a list of references in `git` and deletes a branch whenever that branch does not exist remotely anymore. `pbr` is a quick alias to update information about remote branches and prune all of those that don't exist anymore remotely. These are specially useful if you work with a branch-based workflow where you tend to create a lot of branches on a daily basis. Let's get into more detail about both items. 

Starting with `clean-branches`, I wrote a script called `git-clean-branchs.sh`:

```shell
#!/bin/sh

set -e

git checkout master
git fetch -p

for branch in $(git for-each-ref --format '%(refname) %(upstream:track)' refs/heads | awk '$2 == "[gone]" {sub("refs/heads/", "", $1); print $1}');
do
    git branch -D "$branch"
done%
```

Going through the script we can see a few familiar things. `set -e` helps us keep sanity and try to exit the shell script if any of it starts erring. `git checkout master` guarantees that we are on our main branch and `git fetch -p` fetches information from remote and prunes references that no longer exists on remote (this is similar to what `git pbr` does actually). After everything is up-to-date, we move on to the juicy stuff.

`git for-each-ref --format '%(refname) %(upstream:track)' refs/heads` iterates all the `refs/heads` references (which map to the heads of each branch) and prints the difference between the local information and the remote information. You can read more about `for-each-ref` [here](https://git-scm.com/docs/git-for-each-ref) as it has a few interesting options. Executing this on a repository with branches that don't exist anymore remotely, it prints out something similar to:

```shell
$ git for-each-ref --format '%(refname) %(upstream:track)' refs/heads
refs/heads/master
refs/heads/test-branch [gone]
```

You can see now where `awk '$2 == "[gone]" {sub("refs/heads/", "", $1); print $1}'` helps, can't you? It really just takes all the branches that have `[gone]`, which means they have been removed remotely, removes the prefix `refs/heads` and prints out the name of the branch. There's some incredible stuff that can be done with `awk`, it is a powerful albeit complex command.

After filtering out the necessary branches, the loop just deletes all of them locally so that we are now synchronized with remote and don't have any local branches that don't exist remotely anymore. We give execution permissions to this script and put it somewhere that is in  our `PATH` variable. `git` will load it up and accept it as a valid command.

`pbr` is much simpler. It is a [git alias](https://git-scm.com/book/en/v2/Git-Basics-Git-Aliases) in `~/.gitconfig` mapping to `remote update origin --prune`. It will simply update all the the references in the repository and run pruning against all of the updated references - it will help keep clean all the references to branches that no longer exist remotely. A key difference between both is obvious: one deletes local branches, while the other one only manages remote references.

With these utilities I end up doing all that I need to keep my references (and branches) sync'ed with the remote ones, helping me keep my mental sanity and local repositories organized and clean.
