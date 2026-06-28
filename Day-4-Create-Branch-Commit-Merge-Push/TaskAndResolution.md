# TaskAndResolution.md

# Git Task – Branch Creation, Commit, Merge, and Push

## Objective

The development team requested the DevOps team to perform the following Git workflow on an existing repository.

### Requirements

* Create a new branch named `nautilus-t2q3` from `master`.
* Copy the file `/tmp/index-t2q3.html` into the repository.
* Stage and commit the file.
* Merge the feature branch back into `master`.
* Push both `master` and the feature branch to the remote repository.
* Ensure the repository is clean after completion.

---

# Environment

| Component         | Value              |
| ----------------- | ------------------ |
| Operating System  | Linux              |
| Version Control   | Git                |
| Repository Type   | Working Repository |
| Authentication    | SSH                |
| Remote Repository | Bare Repository    |

> **Note:** Server names, usernames, and organization-specific identifiers have been generalized.

---

# Repository Structure Before Task

```text
working-repository/
├── info.txt
└── welcome.txt
```

---

# Step 1 - Connect to Server

```bash
ssh developer@storage-server
```

Purpose

* Connects securely to the storage server using SSH.

---

# Step 2 - Navigate to Repository

```bash
cd /path/to/working-repository
```

Verify location

```bash
pwd
```

Example Output

```text
/path/to/working-repository
```

---

# Step 3 - Verify Current Branch

```bash
git branch
```

Output

```text
* master
```

Explanation

The repository is currently on the **master** branch.

---

# Step 4 - Create New Branch

Command

```bash
git checkout -b nautilus-t2q3
```

Output

```text
Switched to a new branch 'nautilus-t2q3'
```

Explanation

* Creates a new branch.
* Automatically switches to it.
* The branch is created from **master**.

---

# Step 5 - Verify Branch

```bash
git branch
```

Output

```text
master
* nautilus-t2q3
```

Explanation

The asterisk (*) indicates the active branch.

---

# Step 6 - Copy File

Command

```bash
cp /tmp/index-t2q3.html .
```

Verify

```bash
ls -l
```

Output

```text
index-t2q3.html
info.txt
welcome.txt
```

Explanation

The required HTML file has been copied into the repository.

---

# Step 7 - Stage File

Command

```bash
git add index-t2q3.html
```

Purpose

Moves the file from the Working Directory to the Staging Area.

Repository State

```text
Working Directory
        |
        |
    git add
        |
        ▼
Staging Area
```

---

# Step 8 - Commit File

Command

```bash
git commit -m "Add index-t2q3.html"
```

Output

```text
[nautilus-t2q3 xxxxxx] Add index-t2q3.html
1 file changed
create mode 100644 index-t2q3.html
```

Explanation

Creates a permanent snapshot in Git history.

---

# Common Mistake

Incorrect

```bash
git commit "Add index-t2q3.html"
```

Output

```text
error: pathspec did not match any file(s)
```

Reason

Git interprets the text as a filename.

Correct

```bash
git commit -m "Add index-t2q3.html"
```

---

# Step 9 - Switch Back to Master

```bash
git checkout master
```

Output

```text
Switched to branch 'master'
```

---

# Step 10 - Merge Feature Branch

```bash
git merge nautilus-t2q3
```

Output

```text
Updating ...
Fast-forward
```

Explanation

The master branch now contains the changes from the feature branch.

---

# Step 11 - Push Master

```bash
git push origin master
```

Output

```text
master -> master
```

Explanation

Uploads the updated master branch to the remote repository.

---

# Step 12 - Push Feature Branch

```bash
git push origin nautilus-t2q3
```

Output

```text
[new branch] nautilus-t2q3 -> nautilus-t2q3
```

Explanation

Publishes the feature branch to the remote repository.

---

# Step 13 - Verify Remote Branches

```bash
git branch -r
```

Output

```text
origin/master
origin/nautilus-t2q3
```

Explanation

Both branches now exist on the remote repository.

---

# Step 14 - Verify Repository Status

```bash
git status
```

Output

```text
On branch master

nothing to commit, working tree clean
```

Explanation

The repository is fully synchronized.

---

# Git Workflow

```text
                master
                   │
                   │
         git checkout -b
                   │
                   ▼
         nautilus-t2q3
                   │
                   │
             Copy File
                   │
             git add
                   │
             git commit
                   │
                   ▼
        Commit Created
                   │
             git checkout master
                   │
             git merge
                   │
                   ▼
              master Updated
                   │
          git push origin master
                   │
          git push origin feature
```

---

# Internal Git Flow

```text
Working Directory
       │
       │ git add
       ▼
Staging Area
       │
       │ git commit
       ▼
Local Repository
       │
       │ git push
       ▼
Remote Repository
```

---

# Git Concepts Used

## Branch

A lightweight pointer to a commit.

---

## Checkout

Switches between branches.

---

## Staging Area

Temporary location where selected changes are prepared before committing.

---

## Commit

Creates a permanent snapshot of changes.

---

## Merge

Combines changes from one branch into another.

---

## Origin

Default name of the remote repository.

---

## Fast-Forward Merge

Occurs when master has no new commits after branching.

Git simply moves the master pointer.

---

# Common Errors

## Error

```text
pathspec did not match any file
```

Reason

Incorrect commit syntax.

Solution

```bash
git commit -m "message"
```

---

## Error

```text
Author identity unknown
```

Solution

```bash
git config --global user.name "Developer"
git config --global user.email "developer@example.com"
```

---

## Error

```text
Everything up-to-date
```

Reason

No new commits to push.

---

## Error

```text
Merge conflict
```

Reason

Same file modified in multiple branches.

Solution

Resolve conflicts manually, stage the file, then commit.

---

# Best Practices

* Create feature branches for every task.
* Keep commits small and meaningful.
* Push branches regularly.
* Review before merging.
* Merge into master only after testing.
* Verify repository status before ending work.
* Never commit directly to protected production branches.

---

# Scenario-Based Interview Questions & Answers

# L1 – Junior Engineer

### Q1. What is a Git branch?

A branch is an independent line of development that allows developers to work on new features or fixes without affecting the main codebase.

---

### Q2. Why was a new branch created instead of working directly on master?

Working on a feature branch isolates changes, reduces risk, and supports code review before merging into the main branch.

---

### Q3. What does `git add` do?

It stages changes by moving selected files from the working directory to the staging area.

---

### Q4. What is the purpose of `git commit`?

A commit records a snapshot of the staged changes in the repository history.

---

### Q5. What does `git push` do?

It uploads local commits to the remote repository so other team members can access them.

---

# L2 – Mid-Level Engineer

### Q1. Explain the complete Git workflow used in this task.

1. Checkout `master`.
2. Create a feature branch.
3. Copy the required file.
4. Stage the file.
5. Commit the change.
6. Merge into `master`.
7. Push `master`.
8. Push the feature branch.

---

### Q2. Why push both `master` and the feature branch?

The task explicitly required both branches to be available on the remote repository. The feature branch may be used later for code review, auditing, or future development.

---

### Q3. What is a fast-forward merge?

A fast-forward merge occurs when the target branch has no new commits. Git simply advances the branch pointer instead of creating a merge commit.

---

### Q4. How do you verify that branches exist on the remote?

```bash
git branch -r
```

---

# L3 – Senior Engineer

### Q1. How would you prevent direct commits to the main branch?

By configuring branch protection rules on the Git server to require pull requests, approvals, and successful CI/CD checks before merging.

---

### Q2. What if another developer modifies the same file before you merge?

Git may detect conflicting changes and require manual conflict resolution before completing the merge.

---

### Q3. How would you recover from an incorrect commit?

Depending on the situation, use `git revert`, `git reset`, or `git restore` to safely undo or adjust changes while preserving repository integrity.

---

### Q4. How do you inspect commit history?

```bash
git log --oneline --graph --decorate
```

---

# L4 – Architect

### Q1. Describe a scalable Git branching strategy.

A common enterprise strategy includes:

* `main` for production-ready code.
* `develop` for integration.
* Feature branches for new work.
* Release branches for stabilization.
* Hotfix branches for urgent production fixes.

This approach supports parallel development, controlled releases, and clear separation of responsibilities.

---

### Q2. How would you integrate Git with CI/CD?

* Trigger builds on push events.
* Run automated tests.
* Perform static code analysis.
* Scan dependencies for vulnerabilities.
* Build and publish artifacts.
* Deploy to staging automatically.
* Promote to production after approval.

---

### Q3. How do you secure an enterprise Git environment?

* Enforce SSH key authentication.
* Protect critical branches.
* Require signed commits.
* Use role-based access control.
* Enable audit logging.
* Perform regular backups.
* Integrate secret scanning and code quality tools.

---

# Key Takeaways

* Create feature branches instead of modifying `master` directly.
* Stage changes before committing.
* Write meaningful commit messages.
* Merge feature branches only after validation.
* Push both local and feature branches when required.
* Verify the repository is clean using `git status`.
* Follow branch protection and collaboration best practices in enterprise environments.

