title: Git - The Missing Guide
date: 2016-03-20 11:29:01
tags: git
---

![Git](http://i.imgur.com/QRZsSQI.jpg)

In this article you'll get a *synthesized* list of **important** and **useful** *concepts*, *commands* and *workflow* to work **professionally** with [`git`](https://git-scm.com/), this awesome *version control system*.

Such knowledge will really **boost** your workflow and have a **big impact** in your *development cycle*, and of your team too.

## Table of Contents

- [References](#References)
- [About Git](#About-Git)
- [Installation](#Installation)
- [Starting with Git](#Starting-with-Git)
- [Commit Structure]()
- [Managing the index]()
- [Stashes and branches]()
- [Merge and rebase]()
- [Centralized model]()
- [Distributed model]()
- [Gitflow]()
- [More commands]()
- [Summing up]()

## References

This article is mostly based in [this](http://www.akitaonrails.com/2010/08/17/screencast-comecando-com-git) course, one of the best that I've ever seen. **Many thanks** to the author for providing this course for free!

Other relevant resources are: [A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/) and [Comparing Workflows](https://www.atlassian.com/git/tutorials/comparing-workflows/).

> **Disclaimer**: What you are about to learn will **radically** change your life ... For **much**, **much better**!

**Fasten your seatbelt and let's get started!**

## About Git

- Git was created by [Linus Torvalds](https://en.wikipedia.org/wiki/Linus_Torvalds) to help him handle the creation of the Linux Kernel.
- **Efficient**: has good algorithms to handle all the versioning system, keeping the files really small and the operations very fast.
- **Secure** uses SHA1 to recognize commits. It's almost impossible losing data using git.
- Is a distributed system based in **snapshots**.

## Installation

Just go to the [downloads](https://git-scm.com/downloads) page on Git official website and follow the instructions for your respective operating system.

=)

## Starting with Git

After you install the git on your machine, you need to setup your identity to be recognized as the author of the commits that will be done.

First, let's setup your `email` and your `name`. Open your terminal and type:

```sh
git config --global user.name "Your Name"
git config --global user.email "youremail@example.com"
```

**Important**: This e-mail will be public when you send a commit to GitHub, for example. If you don't want to make public your e-mail, put another one in this place.

All of your configuration will be saved in the `.gitconfig` file.

```sh
cat ~/.gitconfig

[user]
    email = youremail@example.com
    name = Your Name
```

> **PROTIP**: instead of use the command `clear` to clear your terminal, press the keys `Ctrl + L`.

Another interesting configuration is [colorize the output of git](http://unix.stackexchange.com/a/44283).

It's also helpful to know how to change the default editor that git will use to edit our commit and tag messages. To change the default editor, use the following command:

```sh
git config --global core.editor vim
```

For more information about configuration, you can read this resource: [Customizing Git - Git Configuration](https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration).

### Initial Project

In this tutorial we'll use a language agnostic example. I'll show to you how to use git by versioning simple text files, but you will be able to use what you learned with any type of file. 

You'll understand how easy, powerful and useful this amazing tool is!

Let's create a folder called `tasks`, and two simple text files called `todo.txt` and `calendar.txt`.

```sh
mkdir tasks && cd $_
touch {todo.txt,calendar.txt}
```


