---
title: "Git commands"
datePublished: Tue Nov 21 2023 10:42:16 GMT+0000 (Coordinated Universal Time)
cuid: clp87i4cp003c09kz76e6a2zx
slug: git-commands
tags: git

---

# A Scenario-Based Guide
---
## How to edit your commit message
### If you want to edit the previous commit message
```
git commit --amend -m "New Title" -m "New Content"
// An editor (e.g., vim) will open.
// Edit the commit message and save.
```
### If you want to edit a specific commit message while having ongoing work
```
git stash save
git rebase -i HEAD~n 
// An editor will open, displaying commit history.
// Change the "pick" keyword to "reword" for the commit you want to edit then save the file.
// Another editor will open with the commit information you selected in the previous step.
// Edit the commit message and save.
// The changes are applied.
// **Note: All hash codes after the edited commit will be changed.**
git stash pop
```
## How to force push a Git branch

```
git push --force
```