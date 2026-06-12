# Day 4 — Git and GitHub Fundamentals

## Overview

Today I started learning **Source Code Management** with Git and GitHub from scratch.

The main goal was to understand why Git is needed, how it is different from a normal file system, how files move through different Git stages, and how Git helps us track history, restore deleted files, and manage source code safely.

This was a foundation-level session, but it was very important because Git is the base of collaboration, GitHub workflows, and future CI/CD pipelines.

---

## 1. What is Source Code Management?

Source Code Management means managing source code changes in a structured way.

In real projects, code changes again and again. New files are created, old files are edited, bugs are fixed, features are added, and sometimes files are deleted by mistake.

Source Code Management helps us answer important questions:

- Who changed the file?
- When was the file changed?
- What exactly was changed?
- Which version was working before?
- Can we restore an older version?
- Can multiple developers work on the same project safely?

In DevOps, Source Code Management is very important because most automation workflows start from source code repositories. CI/CD pipelines, build systems, testing workflows, and deployments usually begin when code is pushed to a Git repository.

---

## 2. Who Created Git?

Git was created by **Linus Torvalds**.

Linus Torvalds is also known as the creator of Linux. Git was created to manage large source code projects efficiently, especially the Linux kernel.

This is why Git is highly optimized for speed, history tracking, branching, merging, and distributed collaboration.

---

## 3. Git vs GitHub

Today I learned that Git and GitHub are not the same thing.

| Git | GitHub |
|---|---|
| Git is a tool | GitHub is a platform |
| Works locally on our system | Works online |
| Tracks file history | Hosts Git repositories |
| Used through commands | Provides a web UI |
| Helps with commits, branches, restore, merge | Helps with collaboration, pull requests, issues, and remote repositories |

### Simple Summary

```text
Git = Version control tool
GitHub = Online platform for hosting Git repositories
```

Git is used to track changes locally. GitHub is used to store and share Git repositories online.

---

## 4. File System vs Git

A normal file system stores files on a local disk.

It can show basic information such as:

- File name
- File size
- Created date
- Modified date
- File owner
- Permissions

However, a file system does not properly track the full history of changes.

For example, if we have a folder called `scripts` with multiple shell scripts:

```text
scripts/
├── backup.sh
├── deploy.sh
└── monitor.sh
```

The file system can store these files, but it cannot properly answer:

- Who edited `deploy.sh`?
- What exact line was changed?
- What was the previous version?
- Who deleted a file?
- Can we restore a deleted file easily?

This is where Git becomes useful.

Git does not just store files. Git tracks the history of files.

### Key Difference

```text
File system stores files.
Git tracks the history of files.
```

---

## 5. Directory vs Repository

In Linux, a folder is called a **directory**.

Example:

```text
scripts/
```

But in Git, that same directory becomes a **repository** only after Git is initialized inside it.

```bash
git init
```

After running `git init`, Git creates a hidden `.git` folder.

```text
scripts/
├── backup.sh
├── deploy.sh
├── monitor.sh
└── .git/
```

The `.git` folder stores Git-related data such as history, commits, branches, and configuration.

### Important Line

```text
Every repository is a directory,
but every directory is not a repository.
```

A directory stores files.  
A repository tracks files and their history.

---

## 6. Bringing Files from File System to Version Control System

Sir explained that files do not directly become part of Git history.

A file has to pass through different Git stages before it becomes part of version control history.

The main stages are:

```text
Untracked → Staged → Committed
```

This is the basic Git file lifecycle.

---

## 7. Untracked Stage

A file is **untracked** when it exists in the folder, but Git is not tracking it yet.

Example:

```bash
touch script.sh
git status
```

Git may show:

```text
Untracked files:
  script.sh
```

This means:

```text
The file exists in the file system,
but Git has not started tracking its history yet.
```

At this point, if the file is deleted before being added and committed, Git cannot restore it because Git never saved its snapshot.

---

## 8. Staged Stage

A file becomes **staged** when we add it to the staging area.

```bash
git add script.sh
```

The staging area means:

```text
This file is ready to be committed.
```

If we want to stage all files, we can use:

```bash
git add .
```

Staging is important because Git allows us to choose which changes should be included in the next commit.

---

## 9. Committed Stage

A file becomes **committed** when staged changes are saved into Git history.

```bash
git commit -m "Add script file"
```

A commit is like a checkpoint or snapshot of the project at a specific point in time.

After a file is committed, Git can track its history and restore it later if needed.

### Important Point

```text
Staged files are ready for commit.
Committed files are saved in Git history.
```

---

## 10. Why Commit Message is Important

A commit message explains what changed in that commit.

Bad commit message:

```bash
git commit -m "changes"
```

Better commit message:

```bash
git commit -m "Add backup shell script"
```

A meaningful commit message makes project history easier to understand.

When we check history later, we should be able to understand what happened from the commit messages.

---

## 11. Basic Git Commands Practiced

### `git init`

```bash
git init
```

This converts a normal directory into a Git repository.

It creates a hidden `.git` folder and allows Git to start tracking changes in that directory.

---

### `git status`

```bash
git status
```

This command shows the current state of the working directory and staging area.

It tells us whether files are:

- Untracked
- Modified
- Staged
- Deleted
- Clean

This is one of the most important Git commands because it tells us what is happening right now.

---

### `git add`

```bash
git add fileName
```

This moves a file to the staging area.

Example:

```bash
git add script.sh
```

To add all files:

```bash
git add .
```

---

### `git rm --cached`

```bash
git rm --cached fileName
```

This removes a file from Git staging/tracking, but it does not delete the file from the local file system.

### Simple Meaning

```text
Remove from Git tracking, not from local disk.
```

This is useful when a file was added to Git by mistake.

---

### `git commit`

```bash
git commit -m "message"
```

This saves staged changes into Git history.

Example:

```bash
git commit -m "Add initial script files"
```

A commit should always have a meaningful message.

---

### `git restore`

```bash
git restore fileName
```

This restores a committed file if it was deleted or modified locally.

Example:

```bash
rm script.sh
git status
git restore script.sh
```

If `script.sh` was already committed before, Git can bring it back.

### Important Rule

```text
Git can restore only those files that were previously committed.
```

If a file was never committed, Git cannot restore it.

---

## 12. Git Configuration

Before making commits, Git needs to know the user identity.

Correct commands:

```bash
git config --global user.name "Ahsan"
```

```bash
git config --global user.email "your-email@example.com"
```

To check the configuration:

```bash
git config --global --list
```

This information appears in commit history as the author of the commit.

Example:

```text
Author: Ahsan <your-email@example.com>
```

---

## 13. Git Log

To see detailed commit history:

```bash
git log
```

To see short commit history:

```bash
git log --oneline
```

Example output:

```text
a1b2c3d Add initial script files
```

This output contains:

- Short commit ID
- Commit message

### Important Difference

```text
git status = current file state
git log --oneline = committed history
```

If there are no commits yet, `git log --oneline` will not show any history.

---

## 14. File Restore Concept

Sir explained that if a committed file is removed from the file system, it still exists in Git history.

Example:

```bash
rm fileName
```

Git detects it as deleted:

```bash
git status
```

To restore it:

```bash
git restore fileName
```

This is one of the biggest benefits of Git. It protects us from losing committed work.

---

## 15. Common Mistake I Faced

During practice, I saw a commit failure because Git user identity was not configured.

Error type:

```text
Author identity unknown
```

Reason:

```text
Git user.name and user.email were not configured.
```

Fix:

```bash
git config --global user.name "Ahsan"
git config --global user.email "your-email@example.com"
```

After setting the Git identity, commits worked properly.

---

## 16. Practical Workflow Practiced Today

The complete workflow practiced today was:

```text
Create a folder
Initialize Git
Create files
Check status
Stage files
Commit files
View history
Delete a committed file
Restore it using Git
```

This helped me understand how Git moves files from the file system into version control history.

---

## 17. Commands Practiced Today

```bash
mkdir scripts
cd scripts
git init
touch script.sh
git status
git add script.sh
git add .
git rm --cached script.sh
git commit -m "Add script file"
git restore script.sh
git config --global user.name "Ahsan"
git config --global user.email "your-email@example.com"
git config --global --list
git log
git log --oneline
```

---

## 18. Common Bugs and Fixes

| Error / Issue | Reason | Fix |
|---|---|---|
| `fatal: not a git repository` | Command is running outside a Git repository | Run command inside a repo or use `git init` |
| `Author identity unknown` | Git username/email is not configured | Set `user.name` and `user.email` |
| File is untracked | Git sees the file but is not tracking it | Use `git add fileName` |
| File staged by mistake | File was added to staging accidentally | Use `git rm --cached fileName` |
| Deleted committed file | File was removed from local folder | Use `git restore fileName` |
| Empty Git log | No commit has been created yet | Create a commit first |

---

## 19. Interview Questions

1. What is Git?
2. Who created Git?
3. What is Source Code Management?
4. What is the difference between Git and GitHub?
5. What is the difference between a file system and Git?
6. What is the difference between a directory and a repository?
7. What happens when we run `git init`?
8. What is an untracked file?
9. What is the staging area?
10. What is a commit?
11. Why is a commit message important?
12. What does `git status` show?
13. What is the purpose of `git add`?
14. What does `git rm --cached` do?
15. What is the purpose of `git restore`?
16. What is the difference between `git status` and `git log --oneline`?
17. Why do we configure `user.name` and `user.email`?
18. Can Git restore a file that was never committed?

---

## 20. Key Takeaways

- File system stores files, but Git tracks file history.
- Git is a local version control tool.
- GitHub is an online platform for Git repositories.
- A normal directory becomes a Git repository after `git init`.
- Files move through stages: untracked, staged, and committed.
- Staged files are ready for commit.
- Commit messages make history understandable.
- Git can restore committed files.
- `git status` shows the current state.
- `git log --oneline` shows committed history.
- Git user identity must be configured before committing.

---

## One-Line Summary

```text
Git helps us track changes, save history, restore files, and manage source code safely.
```
