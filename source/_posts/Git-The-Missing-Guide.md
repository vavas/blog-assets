title: Git - The Missing Guide
date: 2016-03-25 08:29:23
tags: git
---

![git](http://i.imgur.com/QRZsSQI.jpg)

## Table of Contents

- [Basic Commands](#Basic-Commands)
- [Commit Structure](#Commit-Structure)
- [Managing the Staging Area](#Managing-the-Staging-Area)
- [Stashes and Branches](#Stashes-and-Branches)
- [Managing a Remote Repository](#Managing-a-Remote-Repository)

## Basic Commands

- `git config --global user.name "Your Name"`
- `git config --global user.email "youremail@example.com"`
- `git config --global core.editor vim`
- `git init`: Start a new git repository.

## Commit Structure

- `git status` (`gst`): See the project status
- Working Areas: 
	- **.git Directory**, 
	- **Staging Area**
	- **working Directory**
![Illustration of the working areas](https://i.imgur.com/B0w11nb.png)
- `git add <filename>` (`ga`): Add a file to the Staging Area.
- `git add .` (`gaa`): Add all files to the Staging Area.
- `git add *.js`: Add all `.js` files to the Staging Area.
- `git rm --cached <file>`: Remove a **new file** from the Staging area.
- `git commit -m "My first commit"` (`gcmsg`): Create a commit with a message.
- **`git commit -v -a` (`gca`)**: What I use most. `-v` is verbose, shows the diff at bottom and more meaningful information. `-a` is like `git add .`, so it add **all** files that have been modified and deleted, but new files you have not told Git about are not affected.
- `git help <command>`: Open the manual for the respective command. **USEFUL**!.
- `git log` (`glg`, `glgg`, `glo`, `glog`): Show all commits - history of the project.

## Managing the Staging Area

- `git reset HEAD <filename>` (grh): Remove an modified file from the Staging area.
- `git reset HEAD` (grh): Remove all modified files from the Staging area.
- `git checkout <filename>` (`gco`): Remove an modified file from the Staging area and undo its alterations
- `git commit -m "My first commit" --amend` (``): Add the files/modifications in the Staging area in the last commit.
- **`git commit -v -a --amend` (`gca!`): Add the files/modifications in the Staging area in the last commit**
- **PROTIP**: don't use `--amend` after send the modification to some remote repository. This command is just for local development.
- `.gitignore`: file that tells to git what files should not be tracked. (You can add a file that's ignored with `git add <filename> -f`).
- `git diff <filename>` (`gd`): Shows the modifications in the current file based in its last commit.
- `git diff` (`gd`): Shows the modifications in all files based in their last commit.
- `git reset HEAD~2 --soft`: Remove the last two commits from the project history but **DO NOT DISCARD** the modifications.
- `git reset HEAD~2 --hard`: Remove the last two commits from the project history but **DISCARD** the modifications and all new files that was created in such commits. 
- `git reset <commit> --soft --hard`:
	- `--soft`: Leaves all your changed files "Changes to be committed".
	- `--hard`: Any changes to tracked files in the working tree since <commit> are discarded.
- `git reflog`: show all commits that were "deleted".
- `git merge <commit hash>`: restore the commit.
- **`git add -i` (`ga -i`): Open an interactive mode. REALLY USEFUL!**
	- **obs**: use with the *4: add untracked* option to `git add` files quickly.
	- `1 <enter>`: Select file 1 to be added to the Staging area.
	- `1,3 <enter>`: Select files 1 and 3 to be added to the Staging area.
	- `1-5 <enter>`: Select files 1, to 5 to be added to the Staging area.
	- `-2 <enter>`: Deselect file 2 to be added to the Staging area.
	- `-2-4 <enter>`: Deselect files 2 to 4 to be added to the Staging area.
- `git clean -f`: Remove (delete) untracked files from the working tree.

## Stashes and Branches

### Stash

- `git stash` (`gsta`): Remove all files in the Staging Area to the *"Stash Area"*, that works as another type of *Working Area*.
- `git stash list`: Show a list with all stashes.
- `git stash apply`: Return all files from the last created stash to the Staging Area.
- `git stash apply stash@{2}`: Return all files from the stash@{2} to the Staging Area.
	- **obs**: `stash@{0}` is always the most recent stash.
- `git stash clear`: Clear all stashes.
- `git stash save "name of the stash"`: Save a new stash with a particular name.
- `git stash pop` (`gstp`): Return all files from the last created stash to the Staging Area and remove the stash from the list.
- `git stash drop` (`gstd`): Remove the last created stash (`stash@{0}`) from the list. **Be careful!**
- `git stash drop stash@{2}`: Remove the `stash@{2}` stash from the list. **Be careful!**

### Branch

- `git checkout -b develop` (`gco`): Create a new branch called *develop* and change from the current branch to the *develop* branch.
- `git branch` (`gb`): List all branches.
- `git checkout master` (`gcm`): Switch for the `master` branch.
- `git merge <branch>` (`gm`): Merge a branch into another.
- `gitk --all &`: Open a GUI to visualize your branches and commits.
	- You can test [GitKraken](http://www.gitkraken.com/) too :)
- `git rebase master`: Add the modifications from the `master` branch into the current branch and add move the alterations in the current branch to the top of what was added. *"...rewinding head to replay your work on top of it"*
  - `git rebase --continue`: after resolve conflicts
- `git branch -d <branch>`: delete a branch
  - `-D`: force to delete a branch
- **PROTIP**: one branch for each functionality or bugfix. There is no problem in create lots of branches!
- `git merge <branch> --squash`: Concat several commits into one
  - `--squash` workflow:  
    1. Go to the `master` branch: `git checkout master`
    1. Create a `temp` branch: `git checkout -b temp`
    1. Merge the `feature/x` branch into the `temp` using `--squash`: `git merge feature/x --squash`
    1. See the new modifications/files in the Staging Area: `git status`
    1. Create the unified commit: `git commit -m "Add feature/x"`
    1. Delete the `feature/x` branch: `git branch -D feature/x`
- Differences between `rebase` and `merge`:
  - **rebase**: 
    - keeps the history in a linear fashion;
    - **destructive**: remove the last commit and creates a new one;
    - do not use this method if the commit is already in the remote server.
  - **merge**:
    - useful for keeping the forking history;
    - creates a new commit to unify two branches.

## Managing a Remote Repository

- `git remote add <name> <url>`: Add a new remote repository that will be tracked.
- `git remote rm <name>`: Remove a remote repository.
- `git push <remote> <remote-branch>` (`gp`, `ggp`): Push local commits to a remote repository in the specified branch.
- `git fetch <remote> <remote-branch>` (`gfa`): Fetch new commits from a remote repository into a special branch `<remote>/<branch>`.
- `git pull <remote> <remote-branch>` (`gl`, `ggl`): Incorporates changes from a remote repository into the current branch. In its default mode, git pull is shorthand for git fetch followed by git merge.
  - `git pull --rebase` (`gup`): Runs `git rebase` instead of `git merge`.
