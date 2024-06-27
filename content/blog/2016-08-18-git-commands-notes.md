---
title: "My commonly used commands in GIT"
description: "2016-08-18"
date: "2016-08-18"
paige:
  feed:
    hide_page: true
categories: ["Code"]
weight: 1
---

Update 2017/03/24: Transferred to a separate repository [here](https://github.com/flowerinthenight/git-cheatsheet).

For personal reference:

### Reset a file

```sh
git checkout HEAD -- my-file.txt
```

### Delete last commit

```sh
git reset --hard HEAD~1
```

### Delete local branch

```sh
git branch -d <branch-name>
```

or to force delete

```sh
git branch -D <branch-name>
```

### Delete branch from remote repository

```sh
git push origin --delete <remote-branch-name>
```

### Search for the merge commit from a specific commit

```sh
git log <SHA>..master --ancestry-path --merges
```

### Search for a commit message

```sh
git log | grep <pattern>
```

### List commits on range line of codes for one file

```sh
git blame -L<line#>,+<offset> -- <filename>
```

For example, three lines starting from line 257 of main.cpp

```sh
git blame -L257,+3 -- main.cpp
```

### History of a line (or lines) in a file

```sh
git log --topo-order --graph -u -L <line-start>,<line-end>:<file>
```

For example, history of line 155 of main.cpp

```sh
git log --topo-order --graph -u -L 155,155:main.cpp
```

### Compare (diff) a file from the current branch to another branch

```sh
git diff ..<target-branch> <path-to-file>
```

Or if `difftool` is configured

```sh
git difftool ..<target-branch> <path-to-file>
```

### Rebase/squash all branch commits

```sh
git checkout -b new-branch
modify...
commit...
...
git rebase -i master
(sometimes, I branch out of master for a clean branch and do a git rebase -i clean-branch)
git checkout master
git rebase new-branch
(delete clean-branch)
```

### Combine all branch commits to one before merging to master (sort of like the one above)

```sh
git checkout master
git checkout -b clean
git merge --squash branch_to_merge_to_one_commit
git commit
(add commit message)
git checkout master
git merge clean
```

### Custom format for log

Add to global `.gitconfig` using `git config --global alias.logp "..."`

```sh
git log --pretty=format:'%Cred%h %C(yellow)%d%Creset %s %Cgreen(%cr|%ci) %C(bold blue)[%an]%Creset'
```

<br>
