---
author: ["glaucusec"]
title: "How to Move Commits to a New Branch in Git"
date: "2025-03-13"
description: "f you accidentally committed changes to the wrong branch (e.g., `release/dev`) and meant to commit them to a new branch instead, don’t worry! Here’s a step-by-step guide to fix this issue."
summary: "A step-by-step guide on How to Move Commits to a New Branch in Git"
tags: ["git"]
categories: ["development", "frontend", "nextjs", git, version-control]
series: ["Version Control"]
ShowToc: true
TocOpen: true
---


# How to Move Commits to a New Branch in Git

If you accidentally committed changes to the wrong branch (e.g., release/dev) and meant to commit them to a new branch instead, don’t worry! Here’s a step-by-step guide to fix this issue.

## 1. Create a New Branch to Save Your Changes
Save your current work by creating a new branch from your current state:

```bash
git checkout -b new-branch-name
```
Replace `new-branch-name` with your desired branch name.

## 2. Reset the Original Branch
Now that your changes are safely in the new branch, reset the original branch (`release/dev`) to its state before your commit.

First, find the commit hash of the last commit before your changes:

```
git log
```
Then, reset the branch:

```
git checkout release/dev
git reset --hard <commit-hash>
```
Replace <commit-hash> with the actual commit hash.

## 3. Force-Push the Original Branch (If Needed)
If you’ve already pushed the changes to the remote release/dev branch, you’ll need to force-push the reset branch:

```
git push --force origin release/dev
```
⚠️ Caution: Force-pushing overwrites the remote branch history, so use it carefully.

## 4. Push Your New Branch
Finally, push your new branch to the remote repository:

```
git push -u origin new-branch-name
```

## Summary
1. Create a new branch to save your changes:
`git checkout -b new-branch-name`

2. Reset the original branch to its previous state:
`git checkout release/dev and git reset --hard <commit-hash>`

3. Force-push the original branch if needed:
`git push --force origin release/dev`

4. Push your new branch:
`git push -u origin new-branch-name`

