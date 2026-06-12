# Day 5 — Git Branching and Merge

## Overview

Today I learned one of the most important concepts in Git: **branching**.

The main goal was to understand how developers can work on new changes without disturbing the stable code. I also practiced creating a new branch, committing changes inside that branch, checking commit history, and merging one branch into another.

This topic made Git more practical because branching is used heavily in real development and DevOps workflows.

---

## 1. What is a Git Branch?

A branch in Git is like a separate workspace.

It allows us to work on new changes without directly affecting the main branch.

For example, if the `main` branch contains stable code and we want to add a new feature, we should not directly experiment on `main`.

Instead, we can create another branch and work safely there.

Example:

```text
main  → stable code
dev   → development work
```

---

## 2. Why Branching is Important

Branching helps developers and DevOps teams work safely.

It allows us to:

- Work on new features separately
- Test changes before merging them
- Avoid breaking stable code
- Keep production code clean
- Collaborate with other developers
- Manage different workflows like feature, dev, bugfix, and hotfix branches

In real projects, teams usually do not work directly on the main branch. They create separate branches, commit changes there, test them, and then merge them back.

---

## 3. Creating a New Branch

To create a new branch and switch to it at the same time, we use:

```bash
git checkout -b dev
```

This command does two things:

```text
1. Creates a new branch named dev
2. Switches to the dev branch
```

Modern alternative:

```bash
git switch -c dev
```

---

## 4. Main Branch and Dev Branch

Suppose we are on the `main` branch and we create a new branch named `dev`.

```bash
git checkout -b dev
```

At the time of creation, the `dev` branch contains the same files and folders that are present in the `main` branch.

Example:

```text
main branch:
├── file1.txt
└── README.md
```

After creating `dev`:

```text
dev branch:
├── file1.txt
└── README.md
```

The `dev` branch starts from the current state of `main`.

---

## 5. Important Correction: Branch, Not Repository

A common beginner mistake is saying:

```text
dev repository
```

But the correct word is:

```text
dev branch
```

The repository is the whole Git project.  
Branches are different lines of development inside that repository.

Correct sentence:

```text
The dev branch contains the same files and folders that were present in the main branch when the branch was created.
```

---

## 6. Creating a File in Dev Branch

After switching to the `dev` branch, I created a new file:

```bash
touch file2.txt
```

Then I staged and committed the file:

```bash
git add file2.txt
git commit -m "Adding file 2"
```

This commit was created on the `dev` branch.

---

## 7. Branch-Specific Commits

One of the most important concepts I learned:

```text
A commit belongs to the branch where it is created.
```

If I create and commit `file2.txt` in the `dev` branch, it will not automatically appear in the `main` branch.

To check this, I switched back to main:

```bash
git checkout main
```

Then I checked files using:

```bash
ls
```

The file from the dev branch was not present in main until I merged dev into main.

---

## 8. Important Practice Mistake and Learning

At first, I faced a confusing situation where a file created in the dev branch appeared in the main branch.

The reason was:

```text
The commit had failed because Git user identity was not configured.
```

Since the file was not successfully committed, it stayed as an uncommitted/staged change. Git can carry uncommitted changes when switching branches.

This helped me understand a very important rule:

```text
Branch-specific changes become branch-specific only after a successful commit.
```

If the commit fails, the file is not saved in that branch history.

---

## 9. Git User Identity Issue

The commit failed because Git did not know the author identity.

The fix was:

```bash
git config --global user.name "Ahsan"
git config --global user.email "your-email@example.com"
```

To verify:

```bash
git config --global --list
```

After setting the identity, commits worked correctly.

---

## 10. Checking Commit History

To see commit history in a short format, I used:

```bash
git log --oneline
```

This shows commits in a clean one-line format.

Example:

```text
519ee07 Adding file 2
632e88c Adding Initial Files
```

Each line contains:

- Short commit ID
- Commit message

This command helped me understand which branch had which commit.

---

## 11. Difference Between `git status` and `git log --oneline`

| Command | Purpose |
|---|---|
| `git status` | Shows current working directory and staging area state |
| `git log --oneline` | Shows committed history in short format |

Simple summary:

```text
git status = current state
git log --oneline = commit history
```

---

## 12. Switching Branches

To switch to another branch, we use:

```bash
git checkout branchName
```

Example:

```bash
git checkout main
```

Or:

```bash
git checkout dev
```

Modern alternative:

```bash
git switch main
git switch dev
```

---

## 13. Merging Branches

Sir explained an important merge rule:

```text
First go to the branch where you want to bring changes.
Then merge the branch whose changes you want.
```

For example, if we want to bring `dev` branch changes into `main`, first go to `main`:

```bash
git checkout main
```

Then merge `dev`:

```bash
git merge dev
```

This means:

```text
dev → main
```

---

## 14. Merge Direction

Merge direction is very important.

If we run:

```bash
git checkout main
git merge dev
```

It means:

```text
Bring dev changes into main.
```

But if we run:

```bash
git checkout dev
git merge main
```

It means:

```text
Bring main changes into dev.
```

So we must always stand on the target branch first.

---

## 15. Fast-Forward Merge

During practice, I saw this output:

```text
Fast-forward
create mode 100644 file2.txt
```

This means Git moved the `main` branch pointer forward to include the latest commit from `dev`.

Before merge:

```text
main → Adding Initial Files
dev  → Adding Initial Files → Adding file 2
```

After merge:

```text
main → Adding Initial Files → Adding file 2
dev  → Adding Initial Files → Adding file 2
```

Now both branches point to the same latest commit.

---

## 16. Branch Pointer Concept

After merging, the log showed something like:

```text
519ee07 (HEAD -> main, dev) Adding file 2
632e88c Adding Initial Files
```

This means:

- `HEAD -> main` shows that I am currently on the main branch
- `dev` also points to the same latest commit
- Both branches are now at the same commit after merge

---

## 17. Practical Workflow Practiced Today

The practical workflow was:

```text
Create initial files
Commit files on main
Create dev branch
Switch to dev branch
Create file2.txt
Stage and commit file2.txt on dev
Check commit history
Switch back to main
Verify file2.txt is not present before merge
Merge dev into main
Verify file2.txt is now present in main
```

---

## 18. Commands Practiced Today

```bash
git branch
git checkout -b dev
git switch -c dev
touch file2.txt
git add file2.txt
git commit -m "Adding file 2"
git log --oneline
git checkout main
git merge dev
ls
git status
```

---

## 19. Common Bugs and Fixes

| Issue | Reason | Fix |
|---|---|---|
| File from dev appears in main before merge | File was not committed successfully and remained uncommitted/staged | Commit successfully before switching branches |
| Commit fails with author identity error | Git user.name and user.email are not configured | Configure Git identity |
| Wrong branch merged | User was standing on wrong target branch | First checkout the target branch, then merge |
| `git log --oneline` shows no commits | No successful commit exists yet | Create a commit first |
| Confusion between repository and branch | Repository is the whole project, branch is a line of development | Use correct terms |

---

## 20. Interview Questions

1. What is a Git branch?
2. Why do we use branches in Git?
3. What does `git checkout -b dev` do?
4. What is the difference between a repository and a branch?
5. If a file is committed in the dev branch, will it automatically appear in main?
6. How do you merge dev into main?
7. Why should you checkout the target branch before merging?
8. What is `git log --oneline` used for?
9. What is the difference between `git status` and `git log --oneline`?
10. What is a fast-forward merge?
11. What does `HEAD -> main` mean?
12. Why can uncommitted changes appear after switching branches?
13. What happens if a commit fails?
14. How do you fix the Git author identity error?

---

## 21. Key Takeaways

- A branch is a separate workspace in Git.
- Branching helps us work safely without disturbing stable code.
- `git checkout -b dev` creates and switches to a new branch.
- A new branch starts from the current state of the branch where it was created.
- Commits belong to the branch where they are created.
- Changes from one branch do not automatically move to another branch.
- To bring changes from one branch to another, we use merge.
- Before merging, always checkout the branch where you want to bring changes.
- `git log --oneline` shows commit history in a short format.
- Uncommitted changes can move between branches, so commit successfully before switching branches.
- Fast-forward merge moves the target branch pointer forward.

---

## One-Line Summary

```text
Git branching allows us to work safely in separate lines of development and merge changes only when they are ready.
```
