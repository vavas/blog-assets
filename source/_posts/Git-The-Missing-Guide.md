title: Git - The Missing Guide
date: 2016-03-25 08:29:23
tags: git
---

## Table of Contents

- [Basic Commands](#Basic-Commands)
- [Commit Structure](#Commit-Structure)
- [Managing the Staging Area](#Managing-the-Staging-Area)
- [Stashes and Branches](#Stashes-and-Branches)

## Basic Commands

- `git config --global user.name "Your Name"`
- `git config --global user.email "youremail@example.com"`
- `git config --global core.editor vim`
- `git init`: Start a new git repository

## Commit Structure

- `git status` (`gst`): See the project status
- Working areas: 
	- **.git directory**, 
	- **staging area**
	- **working directory**
![Illustration of the working areas](https://i.imgur.com/B0w11nb.png)
- `git add <filename>` (`ga`): Add a file to the Staging Area
- `git add .` (`gaa`): Add all files to the Staging Area
- `git add *.js`: Add all `.js` files to the Staging Area
- `git rm --cached <file>`: Remove a **new file** from the Staging area
- `git commit -m "My first commit"` (`gcmsg`): Create a commit with a message
- **`git commit -v -a` (`gca`)**: What I use most. `-v` is verbose, shows the diff at bottom and more meaningful information. `-a` is like `git add .`, so it add **all** files that have been modified and deleted, but new files you have not told Git about are not affected.
- `git help <command>`: Open the manual for the respective command. **USEFUL**!
- `git log` (`glg`, `glgg`, `glo`, `glog`): Show all commits - history of the project

## Managing the Staging Area

- `git reset HEAD <filename>` (grh): Remove an modified file from the Staging area
- `git reset HEAD` (grh): Remove all modified files from the Staging area
- `git checkout <filename>` (`gco`): Remove an modified file from the Staging area and undo its alterations
- `git commit -m "My first commit" --amend` (``): Add the files/modifications in the Staging area in the last commit
- **`git commit -v -a --amend` (`gca!`): Add the files/modifications in the Staging area in the last commit**
- **PROTIP**: don't use `--amend` after send the modification to some remote repository. This command is just for local development
- `.gitignore`: file that tells to git what files should not be tracked. (You can add a file that's ignored with `git add <filename> -f`)
- `git diff <filename>` (`gd`): Shows the modifications in the current file based in its last commit
- `git diff` (`gd`): Shows the modifications in all files based in their last commit
- `git reset HEAD~2 --soft`: Remove the last two commits from the project history but **DO NOT DISCARD** the modifications.
- `git reset HEAD~2 --hard`: Remove the last two commits from the project history but **DISCARD** the modifications and all new files that was created in such commits. 
- `git reset <commit> --soft --hard`:
	- `--soft`: Leaves all your changed files "Changes to be committed".
	- `--hard`: Any changes to tracked files in the working tree since <commit> are discarded.
- `git reflog`: show all commits that were "deleted"
- `git merge <commit hash>`: restore the commit
- **`git add -i` (`ga -i`): Open an interactive mode. REALLY USEFUL!**
	- **obs**: use with the *4: add untracked* option to `git add` files quickly
	- `1 <enter>`: Selects file 1 to be added to the Staging area
	- `1,3 <enter>`: Selects files 1 and 3 to be added to the Staging area
	- `1-5 <enter>`: Selects files 1, to 5 to be added to the Staging area
	- `-2 <enter>`: Deselects file 2 to be added to the Staging area
	- `-2-4 <enter>`: Deselects files 2 to 4 to be added to the Staging area
- `git clean -f`: Remove (delete) untracked files from the working tree

## Stashes and Branches