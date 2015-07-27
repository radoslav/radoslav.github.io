---
layout:     post
title:      "Git mini HowTo - cheatsheet."
subtitle:   "Distributed revision control system."
date:       2015-07-27 09:00:00
author:     "radoslav"
header-img: "img/post-bg-04.jpg"
---

## Info

Distributed revision control system, originaly designed and developed by Linus Trovalds for Linux kernel development in 2005.

[Git website](http://git-scm.com/) 

[Git wikipedia](https://en.wikipedia.org/wiki/Git_(software))

[Git in 15 minutes](https://try.github.io)

[Github - free hosting for open source](https://github.com/)

[Bitbucket - free hosting for open source and private repository](https://bitbucket.org/)

### Git basic

#### Help

	$ git help
	$ git help command

#### Start new repository

Create local repository, all required files are created in hiden folder ".git" .

	$ git init

#### Git stage

* create new file "README.md - starts as untracked
* add file to staging area - $ git add README.md
* git status - check what's changed since last commit
* commit changes - $ git commit -m 'add file README.md'
* Snapshot of this stage

#### Add files to stage (Staging area)

Add list of files.

	$ git add file1.txt file2.txt file3.txt

Add all new or modified files.

	$ git add --all
	$ git add -A

Stages new and midified without deleted. It does not add any 'rm' action.

	$ git add .

Stages modifiled and deleted without new. It does not add any new file, it only stages changes to already tracked files.

	$ git add -u

Add all txt files in current project.

	$ git add *.txt

Add all txt files in the whole project.

	$ git add '*.txt'

Add all files in 'code' directory

	$ git add code/

#### Unstaging files

	$ git reaset HEAD file

Blow away all changes since last commit.

	$ git checkout -- file

#### Commiting changes

Very important! Commit massages! Present time.

	$ git commit -m 'add text message'

Skip staging and commit. Add achanges from all tracked files. Does not add new untracked files.

	$ git commit -a -m 'modify README.md'

View log with commits

	$ git log

Show unstaged differences since last commit

	$ git diff

View staged differences

	$ git diff --staged

Adding to last commit (ex. forgot to add a file). Whatever has been staged is added to last commit.

	$ git commit -amend -m 'modify README.md and add new file.txt'

Undo last commit, put changes into staging.

	$ git reset soft HEAD^

Undo last commit and all changes

	$ git reset --hard HEAD^

Undo last 2 commits nad all changes

	$ git reset --hard HEAD^^

### How to share

Git is distributed revision control so there is no central server, all repository are equals. Git does not take care of access control.

	$ git remote

Show remote repositories

	$ git remote -v

#### Remote repository

- 'add' - new remote
- 'origin' - our name for this remote
- 'https://' - address

	$ git remote add origin https://git.repository.pl/user/test.git

Remove remotes

	$ git remote rm origin

#### push and pull to remote

Pushing to remote. 

- 'origin' - remote repository name
- 'master' - local branch to push

	$ git push origin master

Pushing to remote and set default repository name and branch

	$ git push -u origin master

Pull changes down from the remote. It is good to do this often.

	$ git pull

### Cloning and branching

####  Cloning

1. Downloads the entire repository into a new project directory.
2. Adds the 'origin' remote, pointing it to the clone url.
3. Checs out inital branch (likely master) sets the HEAD.

	$ git clone https://git.repository.pl/user/test.git

- 'project-name' - local folder name

	$ git clone https://git.repository.pl/user/test.git project-name

#### Branching

Use case - need to work on a feature that will take some time.

Check current branch

	$ git branch

Create branch from current branch

	$ git branch dev

Swiching to a new branch.

	$ git checkout dev

Create and checks out new branch

	$ git checkout -b dev

### Merge

- 'Fast-forward' - nothing change in branch, only add files from another branch
- 'Non-fast-forward' - touch files in current branch

Switch to branch master

	$ git checkout master

Merge brings one branch's changes into another

	$ git merge dev

Branch clean up. When you are done with branch, you can safely remove it.

	$ git branch -d cat

### Remote branches and tags

- when you need other people to work on you branch
- any branch that will last more than a day

Links local branch to the remote branch (traking)

	$ git push origin dev

List all remote branches

	$ git branch -r

Show all branches on the remote

	$ git remote show origin

Remove remote branch

	$ git push origin :dev

Must delete local branch manually

	$ git branch -d dev

If "error: the branch 'dev' is not fully merged" use force with capital '-D'

	$ git branch -D dev

To clean up deleted repo branches. From time to time.

	$ git remote prune origin

Change remote branch name

- 'dev' - local name
- 'master' - remote name

	$ git push github dev:master

### Tagging

A tag is a reference to a commit (used mostly for relese versioning)

List for all tags

	$ git tag 

Check out code at commit

	$ git checkout v0.0.1

Add a new tag

	$ git tag -a v0.0.2 -m 'version 0.0.3'

Push new tags to remote, if not 'push --tags' tags are local.

### Rebase (alternative to merge)

Not prefer (may lose history, commit messages), but very useful.

Rebase never use it on public branches.

ToDo

### History

#### Log

- '%h' - SHA hash
- '%s' - subject
- '%an' - author

	$ git help log
	$ git log
	$ git log --pretty=oneline
	$ git log --pretty=format:"%h %ad- %s [%an]"
	$ git log --pretty=format:"%h - %s"

Show changes

	$ git log --oneline -p

Stats

	$ git log --oneline --stat

Visual representation of the branch merging into master

	$ git log --oneline --graph

Date ranges (minute, day, hour, month, weeks)

	$ git log --until=1.minute.ago
	$ git log --since=1.day.ago
	$ git log --since=2000-01-01 --until=2015-12-12

#### Diff

Diff to HEAD

	$ git diff
	$ git diff HEAD

Parent of latest commit

	$ git diff HEAD^

Grandparent of latest commit

	$ git dif HEAD^^

Five commits ago

	$ git diff HEAD~5

Second most recent commit vs. most recent

	$ git diff HEAD^..HEAD

Eariler commits by SHA

	$ git diff 2erw35..rw2312

Diff between two branches

	$ git diff master dev

Time-based diff

	$ git diff --since=1.week.ago --until=1.minute.ago

Git blame

	$ git blame index.html --date short

### Configuration

	$ git config --global color.ui true
	$ git config --global user.name "YOUR NAME"
	$ git config --global user.email "YOUR@EMAIL.com"

	$ git config --global core.editor vim // change editor to vim

#### Excluding files (only in local repo)

Can be put in file .git/info/exclude

#### Ignor files (not only in local repo)

.gitignore

### Removing and untraking files

Remove file

	$ git rm README.md

Untracking files, delete from git, not from file system

	$ git rm --cache file.log

### Aliases

#### Common alliases

	$ git config --global alias.st status
	$ git config --global alias.co checkout
	$ git config --global alias.st branch
	$ git config --global alias.st commit

	$ git config --global alias.mylog "log --pretty=format:'%h %s [%an]' --graph

