---
date: "2017-03-27T00:17:46Z"
title: "On Elevating Shell Scripts"
draft: false
aliases: ["/blog/on-elevating-shell-scripts/"]
categories:
    - bash

---

Automatically obtaining superuser permissions always seemed like
black magic. I have a vague idea of how it works and I am very aware of their potential interesting uses.

I have always seen those magic installers doing their black magic behind a (not so) beautiful GUI and wondered "how the hell can they install things in places my user can't seem to reach?".

Turns out that's not that difficult and it can be achieved with relative transparency.

Recently, I have been tasked with writing some installation scripts which, in turn, had to access `/usr/local/`. As most will know, this is usually forbidden land for regular users. Most can only read and can't even read anything.

So I set out to figure out how would I be able to install files in places users can't access, without the help of a GUI.

My version of occurrence was the usual one: make them `sudo` the script. This solution would result in something like:

```
sudo script.sh
```
I find this obnoxious because you are not helping your users. They need to know before hand that they need to run this as superuser and actually run the command as a superuser. I discarded this idea as simple but not optimal.

I went on to Google-land to search for a better wait to do it. Right away I found a couple of answers ranging from "this is awful" to "you can do it this way".

One of the best resources I found was [this](http://unix.stackexchange.com/questions/28791/prompt-for-sudo-password-and-programmatically-elevate-privilege-in-bash-script) answer on [unix.stackexchange](http://unix.stackexchange.com/).

Reading through that answer I am intriged by five things, in those snippets: `$EUID`, `$0`, `$@`, `$?` and `$UID`. These looked like interesting, useful Bash environment variables. I research a little bit more to find out what they are.

Again, I turned my attention to StackOverflow, specifically [this answer](http://stackoverflow.com/questions/27669950/difference-between-euid-and-uid), [this one](http://stackoverflow.com/questions/27669950/difference-between-euid-and-uid) and Wikipedia. I not always use StackOverflow but sometimes it helps a ton. `$EUID` and `$UID`, represent two separate things. `$UID` gives the user identifier which identifies the real user ID of the calling process, in the system. We can use `$EUID` to give the effective user, in the system. Apparently, there is also the file system, saved and real (`ruid`). [1] The differences between all this types of users are still unclear to me but, what I know so far, it that a superuser will always have `$EUID` as 0. An important note is that this works only for `bash`.

The remaining parts of this are also `bash` variables:

- `$0` is a positional parameter that refers to the filename of the script in execution. So, it is effectively argument - 1.

- `$@` represents the parameters, "without interpretation or expansion". It will output the parameters as they were written.

- `$?` will get you the exit status of a command.

All of this can be further investigated on [Advanced Bash-Scripting Guide's Chapter 9](http://tldp.org/LDP/abs/html/internalvariables.html).

The top voted answer was an interesting way to ask the system: "Are you running this as superuser?". If the system reply that it wasn't running it as super user, it would re-run the script, along with its parameters, preprending `sudo`. The full answer is very short but very powerful:

```
if [ $EUID != 0 ]; then
    sudo "$0" "$@"
    exit $?
fi
```

So, anywhere on top of this solution, you let the user know that elevated permissions will be required. This provides an interesting command line interface, to elevate your permission level, without the user having to know that he must run it as a superuser.

Of course, all this has security implications that must be consider. Then again, running anything, as a superuser, without trusting what you are running wouldn't be much of an option either.

[1] http://man7.org/linux/man-pages/man2/getuid.2.html
