title: Git - The Missing Guide
date: 2016-03-25 08:29:23
tags: git
---

## Git Useful Commands and Concepts

- `git config --global user.name "Your Name"`
- `git config --global user.email "youremail@example.com"`
- `git config --global core.editor vim`
- `git init`: Start a new git repository
- `git status` (`gst`): See the project status
- Working areas: **.git directory**, **staging area** and **working directory**. [Image](https://i.imgur.com/B0w11nb.png)
- `git add <filename>` (`ga`): Add a file to the Staging Area
- `git add .` (`gaa`): Add all files to the Staging Area
- `git add *.js`: Add all `.js` files to the Staging Area
- `git rm --cached <file>`: Remove a **new file** from the Staging area
- `git commit -m "My first commit"` (`gcmsg`): Create a commit with a message
- **`git commit -v -a` (`gca`)**: What I use most. `-v` is verbose, shows the diff at bottom and more meaningful information. `-a` is like `git add .`, so it add **all** files that have been modified and deleted, but new files you have not told Git about are not affected.
- `git help <command>`: Open the manual for the respective command. **USEFUL**!
- `git log` (`glg`, `glgg`, `glo`, `glog`): Show all commits - history of the project
- `git reset HEAD <filename>` (grh): Remove an modified file from the Staging area
- `git reset HEAD` (grh): Remove all modified files from the Staging area
- `git checkout <filename>` (`gco`): Remove an modified file from the Staging area and undo its alterations
- `git commit -m "My first commit" --amend` (``): Add the files/modifications in the Staging area in the last commit
- **`git commit -v -a --amend` (`gca!`): Add the files/modifications in the Staging area in the last commit**
- **PROTIP**: don't use `--amend` after send the modification to some remote repository. This command is just for local development
- `.gitignore`: file that tells to git what files should not be tracked. (You can add a file that's ignored with `git add <filename> -f`)
- `git diff <filename>` (`gd`): Shows the modifications in the current file based in its last commit
- `git diff` (`gd`): Shows the modifications in all files based in their last commit
- `git reset HEAD~2`: Remove the last two commits from the project history but **DO NOT DISCARD** the modifications 
- `git reset HEAD~2 --hard`: Remove the last two commits from the project history but **DISCARD** the modifications and all new files that was created in such commits
- `git reflog`: show all commits that were "deleted"
- `git merge <commit hash>`: restore the commit
- **`git add -i` (`ga -i`): Open an interactive way to add files. REALLY USEFUL!**
	- ps: use with the *4: add untracked* option to add files quickly
	- 1 <enter>: Add file 1
	- 1,3 <enter>: Add files 1 and 3
	- 1-5 <enter>: Add files 1, 2, 3, 4 and 5
	- -2 <enter>: Remove the file 2
	- -2-4 <enter>: Remove files 2, 3 and 4

