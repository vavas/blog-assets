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

In this tutorial we'll use a "(programming) *language agnostic*" example. I'll show to you how to use git by versioning simple text files, but you will be able to use what you learned with any type of file. 

You'll understand how easy, powerful and useful this amazing tool is!

Let's create a folder called `tasks`, and two simple text files called `todo.txt` and `calendar.txt`.

```sh
mkdir tasks && cd $_
touch {todo.txt,calendar.txt}
```

Excellent! Now we can really start to play with git!

We need to state this project will be controlled by git, so let's initiallize our project in the git perspective:

```sh
git init

Initialized empty Git repository in /home/ericdouglas/github/tasks/.git/
```

Now if you look more closely to your directory structure, you will see that we have a new folder called `.git`.

```sh
ls -la

-rw-r--r-- 1 ericdouglas ericdouglas    0 Mar 22 08:20 calendar.txt
drwxr-xr-x 7 ericdouglas ericdouglas 4096 Mar 22 08:33 .git
-rw-r--r-- 1 ericdouglas ericdouglas    0 Mar 22 08:20 todo.txt
```

> **PROTIP**: read **all** messages that git returns to you! It’s common to know what you need to do next just by reading the messages that git returns.

All the information, metadata and  project history will be alocated on this `.git` folder.

To know more about one specific command, you can type `git help <command>`, ex: `git help init`.

**ps**: I use [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh) to boost my terminal. I'll put after the git original command the shortcut that I use for the respective command. You can see the oh-my-zsh git cheatsheet [here](https://github.com/robbyrussell/oh-my-zsh/wiki/Cheatsheet#git).

One of the most important/used commands in git is the `git status` command, or the shortcut `gst` with oh-my-zsh. It returns the status of your project. Let's type this command and see what we get:

```sh
git status

On branch master

Initial commit

Untracked files:
  (use "git add <file>..." to include in what will be committed)

  calendar.txt
  todo.txt

nothing added to commit but untracked files present (use "git add" to track)
```

### Work areas

One really important concept to understand now (and ever) is that git has 3 work areas.

The first area is the `.git` folder. All the project is contained in this directory.

The second area is what we call **Working area**. This is a snapshot, a moment in time of all this project/files.

The third area is invisible. It's not demonstrated in our filesystem. To see that area, we need to type `git status` (`gst`). This area is called **Staging area**. It's like a buffer, that we use to create modifications in our project before to pack those alterations and send through a commit to the repository that is located inside of the `.git` directory.

![Git areas](https://i.imgur.com/B0w11nb.png)

To add a file to the **Staging area** we need to use the `git add` command, or the shortcut `ga`. To add a single file you need to type `git add <filename>`.

Let's add the file `todo.txt` to the Staging area:

```sh
git add todo.txt

git status

On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

  new file:   todo.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)

  calendar.txt
```

Only files that are in the **Staging area** will be commited.

To add all files at once, you can use the `git add .` command, or the shortcut `gaa`.

We also have the option to add all files of one type. E.g: `git add *.txt`.

```sh
git add .

git status

On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

  new file:   calendar.txt
  new file:   todo.txt
```

If you follow one of my initial tips (read **all** messages that git returns), you saw this message:

> (use "git rm --cached <file>..." to unstage)

This command `git rm --cached <file>` is used in order to remove the specified file of the Staging area. In that way, this file will **not** be part of the next commit.

Let's remove our `todo.txt` file from the Staging area:

```sh
git rm --cached todo.txt

git status

On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

  new file:   calendar.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)

  todo.txt
```

Now our `todo.txt` is untracked again.

**Let's make our first commit!** Type the command `git commit -m "My first commit"`, or
