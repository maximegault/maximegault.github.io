---
title: Git branch
date: 2021-04-29 8:30:00 HH:MM:SS +0100
categories: [Development, Git, GitLab, GitHub]
tags: [development, git, gitlab, github]
---

## Create branches

### Create a local branch

Simply create a new branch and push it:

```bash
git checkout -b myCustomBranch
```

### Create a remote branch

Simply create a new branch and push it directly to the remote repository:

```bash
git checkout -b myCustomBranch
git push -u origin HEAD
```

### Get a remote branch

We have a cloned a repository, and so only its `master` branch. It contains ie. a `develop` branch and we need it locally. To get it:

```bash
git checkout develop
```

## List branches

### List local branches only

```bash
git branch
  master
* myCustomBranch
```

### List local and remote branches

With `-a` argument:

```bash
git branch -a
  master
* myCustomBranch
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
  remotes/origin/myCustomBranch
```

### List with verbosity

The `-v` argument gives the commit the branch is on:

```bash
git branch -v
  master            a0ca238 Simple fix
* myCustomBranch    a0ca238 Simple fix
```

### List with more verbosity

The `-vv` argument gives the commit the branch is on and the corresponding remote branch:

```bash
git branch -vv
  master            a0ca238 [origin/master] Simple fix
* myCustomBranch    a0ca238 [origin/myCustomBranch] Simple fix
```

### Combine verbosity and remote branches details

With the combination of the `-avv` arguments:

```bash
git branch -avv
  master                            a0ca238 [origin/master] Simple fix
* myCustomBranch                    a0ca238 [origin/myCustomBranch] Simple fix
  remotes/origin/HEAD               -> origin/master
  remotes/origin/master             a0ca238 Simple fix
  remotes/origin/myCustomBranch     a0ca238 Simple fix
```

## Delete branches

### Delete a local branch

```bash
git branch -d myCustomBranch
```

### Delete a remote branch

```bash
git push origin --delete myCustomBranch
```

## Clone branches

### Clone a specific branch

Instead of cloning a repo like:

```bash
git clone myRepo
```

Specify the branch with `-b`:

```bash
git clone -b neededBranch myRepo
```
