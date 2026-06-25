# TaskAndResolution.md

# Git Lab - Install Git, Create Bare Repository & Clone Repository

**Author:** Cloud & DevOps Engineer
**Platform:** Linux (RHEL/CentOS Based)
**Category:** Git Administration
**Difficulty:** Beginner → Intermediate

---

# Objective

This lab consists of two independent Git administration tasks:

1. Install Git and create a bare repository.
2. Clone an existing bare repository into a working directory using a non-root user.

The objective is to understand the difference between a bare repository and a working repository while learning the standard Git workflow used in enterprise environments.

---

# Scenario 1

## Requirement

The development team requested the DevOps team to install Git on a Linux server and create a bare repository named:

```
/opt/games.git
```

This repository will act as the central remote repository.

---

# Solution

## Step 1 : Install Git

Command

```bash
sudo yum install -y git
```

Sample Output

```text
Dependencies resolved.
Installed:
git
git-core
git-core-doc
Complete!
```

Explanation

* Installs Git package.
* Also installs required dependencies.
* Git becomes available system-wide.

---

## Step 2 : Verify Installation

Command

```bash
git --version
```

Output

```text
git version 2.52.0
```

Explanation

Confirms Git installation.

---

## Step 3 : Create Bare Repository

Command

```bash
sudo git init --bare /opt/games.git
```

Output

```text
Initialized empty Git repository in /opt/games.git/
```

Explanation

Creates a bare Git repository.

Unlike a normal repository, a bare repository contains only Git metadata.

It has

* HEAD
* refs
* objects
* hooks
* config

No project files exist here.

---

## Step 4 : Verify Repository

Command

```bash
ls -l /opt
```

Output

```text
games.git
```

Command

```bash
cd /opt/games.git
ls
```

Output

```text
HEAD
config
description
hooks
info
objects
refs
```

Explanation

These are the internal components of Git.

---

# Repository Structure

```
/opt/games.git

HEAD
config
description
hooks/
info/
objects/
refs/
```

This repository has

* No source code
* No README
* No application files

It is intended only for Git Push/Pull operations.

---

# Scenario 2

## Requirement

Clone an existing repository

```
/opt/cluster.git
```

into

```
/usr/src/kodekloudrepos
```

using a standard developer account.

No permissions or ownership should be modified.

---

# Solution

## Step 1

Verify repository

```bash
ls -ld /opt/cluster.git
```

Output

```text
drwxr-xr-x cluster.git
```

Explanation

Ensures repository exists.

---

## Step 2

Verify destination directory

```bash
ls -ld /usr/src/kodekloudrepos
```

Output

```text
drwxr-xr-x kodekloudrepos
```

Explanation

Confirms destination exists.

---

## Step 3

Move into destination

```bash
cd /usr/src/kodekloudrepos
```

---

## Step 4

Clone repository

Command

```bash
git clone /opt/cluster.git
```

Output

```text
Cloning into 'cluster'...
warning: You appear to have cloned an empty repository.
done.
```

Explanation

Git creates a working copy named

```
cluster
```

Since the remote repository contains no commits, Git reports it as an empty repository.

This is expected behavior.

---

## Step 5

Verify

```bash
ls -l
```

Output

```text
cluster
```

---

## Step 6

Move into cloned repository

```bash
cd cluster
```

---

## Step 7

Check repository status

Command

```bash
git status
```

Output

```text
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

Explanation

Repository has

* no commits
* no tracked files
* working directory is clean

---

# Bare Repository vs Working Repository

| Bare Repository           | Working Repository          |
| ------------------------- | --------------------------- |
| Stores Git database only  | Contains project files      |
| No working directory      | Working directory available |
| Used as remote repository | Used by developers          |
| Cannot edit project files | Files can be modified       |
| Used for Push/Pull        | Used for Development        |

Example

```
Remote

/opt/cluster.git
```

```
Developer Clone

/usr/src/kodekloudrepos/cluster
```

---

# Internal Flow

```
                Developer

                     |
                     |

          git clone / git push

                     |

                     ▼

+----------------------------------+
|      Bare Repository             |
|                                  |
|        /opt/cluster.git          |
+----------------------------------+

                     ▲

                     |

                     |

+----------------------------------+
| Working Repository               |
|                                  |
| /usr/src/kodekloudrepos/cluster  |
+----------------------------------+
```

---

# Why Clone Repository?

Instead of editing files directly inside the bare repository,

developers create a working copy.

Example

```
git clone /opt/cluster.git
```

Developers then

* edit code
* commit changes
* push changes

The central repository remains protected.

---

# Important Git Commands

Install Git

```bash
yum install -y git
```

Check Version

```bash
git --version
```

Create Bare Repository

```bash
git init --bare /opt/games.git
```

Clone Repository

```bash
git clone /opt/cluster.git
```

Check Status

```bash
git status
```

List Remote

```bash
git remote -v
```

Check Branch

```bash
git branch
```

View Hidden Files

```bash
ls -la
```

---

# Common Errors

## Error

```
ll: command not found
```

Reason

Alias not configured.

Solution

```bash
ls -l
```

---

## Error

```
warning: You appear to have cloned an empty repository.
```

Reason

Repository has no commits.

Solution

No action required.

---

## Error

```
fatal: this operation must be run in a work tree
```

Reason

Attempted to perform working-tree operations inside a bare repository.

Solution

Clone the repository first and work in the cloned directory.

---

# Best Practices

* Never develop directly inside a bare repository.
* Keep the central repository protected.
* Clone repositories using standard user accounts.
* Avoid changing ownership or permissions unless required.
* Verify repository status before pushing.
* Keep repositories clean and organized.

---

# Scenario Based Interview Questions & Answers

# L1 - Junior Engineer

### Q1. What is Git?

Git is a distributed version control system that tracks changes in source code and enables multiple developers to collaborate efficiently.

---

### Q2. What is a repository?

A repository is a storage location containing project files along with their complete version history.

---

### Q3. What is the difference between `git init` and `git clone`?

`git init` creates a new repository from scratch.

`git clone` creates a local copy of an existing repository while preserving its history.

---

### Q4. What is a bare repository?

A bare repository contains only Git metadata and no working directory. It is primarily used as a central remote repository for collaboration.

---

### Q5. Why was the warning "You appear to have cloned an empty repository" displayed?

Because the remote repository had no commits or tracked files. This is expected behavior when cloning a newly created repository.

---

# L2 - Mid-Level Engineer

### Q1. Why are developers expected to work in the cloned repository rather than the bare repository?

The cloned repository provides a working directory where files can be created, modified, committed, and tested. A bare repository is designed only to store Git history and accept push/pull operations.

---

### Q2. Explain what happens internally during `git clone`.

* A new working directory is created.
* The entire Git history is copied.
* A `.git` directory is created locally.
* The remote repository is automatically configured as `origin`.
* The default branch is checked out (if commits exist).

---

### Q3. Why should cloning typically be done as a non-root user?

Using a standard user ensures proper ownership of the working copy, reduces security risks, and follows the principle of least privilege.

---

### Q4. What command shows the configured remote repository?

```bash
git remote -v
```

---

# L3 - Senior Engineer

### Q1. Your team accidentally modified files inside the bare repository. What is the impact?

Direct modification of files in a bare repository can corrupt repository metadata, disrupt collaboration, and prevent clients from pushing or pulling reliably. Developers should never edit a bare repository directly.

---

### Q2. How would you host a central Git server on Linux?

* Create a bare repository.
* Configure SSH access.
* Assign appropriate ownership and permissions.
* Enable secure authentication.
* Allow developers to clone, fetch, and push over SSH.

---

### Q3. Why does a bare repository not contain a `.git` directory?

Because the repository itself represents the Git database. All Git metadata is stored directly at the top level of the repository.

---

### Q4. How do you verify that a repository is bare?

```bash
git --git-dir=/opt/games.git rev-parse --is-bare-repository
```

Expected Output

```text
true
```

---

# L4 - Architect

### Q1. Design a Git architecture for a large enterprise.

A common architecture includes:

* Central bare repositories hosted on dedicated Git servers.
* Feature branching strategy.
* Protected main branches.
* Pull request workflow.
* Automated CI/CD pipelines.
* Code review and approval process.
* Backup and disaster recovery.
* Repository mirroring for high availability.

---

### Q2. Why are bare repositories preferred for centralized collaboration?

Because they:

* Eliminate working-tree conflicts.
* Safely support concurrent pushes.
* Store only repository metadata.
* Improve security and maintainability.
* Integrate cleanly with CI/CD systems.

---

### Q3. How would you secure enterprise Git repositories?

* SSH key authentication
* Role-based access control
* Protected branches
* Signed commits
* Mandatory code reviews
* Repository backups
* Audit logging
* Automated vulnerability scanning
* Secret scanning before merge

---

# Key Takeaways

* Install Git before creating repositories.
* Use `git init --bare` to create a central remote repository.
* Clone repositories into a working directory for development.
* Never modify a bare repository directly.
* Understand the distinction between a remote (bare) repository and a working repository.
* Follow secure Git workflows using non-root users and centralized collaboration practices.

