---
title: Another Note on Elevating Shell Scripts
date: 2019-11-18
author: "Hugo Martins"
summary: "A review of previous notes on how to elevate shell scripts from within the scripts themselves, after understanding my previous approach had issues."
categories:
    - bash
---

A long while ago I wrote about how to [elevate shell scripts](https://hugomartins.io/blog/on-elevating-shell-scripts/) from within the scripts themselves. It was an interesting approach, in which we would recursively run the script as a superuser, if wasn’t already being run as a superuser.

I’ve recently come to figure out that this approach presents problems. It all started when I was refactoring my [dotfiles](https://github.com/caramelomartins/dotfiles) and I started noticing the amount of `sudo`-based commands I was using. I wrote an installation script - `dotfiles.sh` in that repository - which would, among other things, run `apt` and `dpkg`, necessarily requiring superuser permissions. 

I don’t appreciate randomly being forced to enter my password when installing software and it would also randomly stop the script and wait for user input, ruining the whole *automate-it-away* thingy. I decided I would run the script as superuser right from the start. That proved to be a mistake.  

As a quick reminder, the previous approach suggested:


    if [ $EUID != 0 ]; then
        sudo "$0" "$@"
        exit $?
    fi

This allows us to auto-elevate the bash script, if the script isn’t being run as a superuser, by identifying the user that is running the script. I missed something essential though…

If I run a script that, apart from installing software, also tries to create things in user-based directories - such as `$HOME` - it will install everything inside root user’s directories. I made a complete mess of it and the script started failing.

I still want to be able to install software though, therefore having superuser permissions is mandatory. But I don’t want to *run* the script as a superuser. In this way, I can properly configure my personal directories while being able to install software at will.

What I came up with seems to be an interesting compromise, although it still feels a bit *hackish*. Usually, when running `sudo` in Ubuntu, you get the ability to run it afterwards without being prompted for a password again for 15 minutes [1]. I thought that if I was able to run `sudo`  at the start of the script, without performing any relevant action, I would get those 15 minutes to run the remaining of the script.

I found that if we run a command such as `sudo bash -c "echo \"Running script as $USER with root permissions.\"``"` the only thing that happens is that you’ll get prompted for a password, if needed:


    $ sudo bash -c "echo \"Running script as $USER with root permissions.\""
    Running script as hmartins with root permissions.

This will allow us to elevate our permissions, while running the script as the current user we want to run it as. It also doesn’t prompt a user for a password if there’s no need for it and it doesn’t force our application to parse any sensitive input, rather redirecting all of that processing to `sudo`.

This approach only has one lasting problem: what if the script takes more than 15 minutes (or whatever the default is on the system) to complete? In order to solve that, my best approach is to use `sudo -v` somewhere in the script. `-v` option allows us to extend *”the sudo timeout for another 15 minutes by default”* [1] **but it doesn’t allow you to run a command at the same time.

It isn’t a silver bullet for everything but you win some and you lose some. For now, it serves my purposes. It is also interesting how our learning keeps evolving. As we are faced with different challenges and needs, we tend to go deeper into understanding the tools we are using and their functionalities.

[1] You can run `man sudo` to check confirm this.

