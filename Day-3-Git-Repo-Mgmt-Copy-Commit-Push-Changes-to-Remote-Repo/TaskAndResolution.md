````markdown
# TaskAndResolution.md

# Git Repository Management - Copy, Commit, and Push Changes to a Remote Repository

---

# Project Overview

A software engineering team initiated a new web application project and created a centralized Git repository for source code management. A working copy of the repository was already cloned on a dedicated Git server.

A sample HTML page was provided on a build server. The objective was to transfer this file to the Git server, add it to the cloned repository, commit the changes, and push them to the remote repository.

During implementation, multiple real-world Git and Linux permission issues were encountered and resolved.

This exercise demonstrates practical DevOps concepts involving:

- Linux Administration
- Secure File Transfer (SCP)
- SSH Remote Access
- Git Repository Management
- Git Safe Directory
- Linux File Permissions
- Bare Git Repository
- Version Control Best Practices

---

# Objective

Perform the following activities:

1. Verify the sample file exists.
2. Securely copy the file to the Git server.
3. Connect to the Git server.
4. Place the file inside the cloned repository.
5. Stage the changes.
6. Commit the changes.
7. Push the commit to the remote repository.

---

# Environment Details

| Component | Value |
|-----------|-------|
| Build Server | build-node01 |
| Git Server | git-storage01 |
| Repository Name | web-application |
| Bare Repository | /opt/web-application.git |
| Working Repository | /usr/src/projects/web-application |
| Git Branch | master |
| Sample File | sample.html |

---

# Architecture

```text
                 +----------------------+
                 |     Build Server     |
                 |     build-node01     |
                 +----------+-----------+
                            |
                            | SCP
                            |
                            ▼
                 +----------------------+
                 |     Git Server       |
                 |    git-storage01     |
                 +----------+-----------+
                            |
                            |
                  Working Repository
                            |
            git add → git commit → git push
                            |
                            ▼
                 Bare Git Repository
```

---

# Task Workflow

```text
Verify File
      │
      ▼
Copy File Using SCP
      │
      ▼
SSH Login
      │
      ▼
Navigate Repository
      │
      ▼
Copy File
      │
      ▼
git add
      │
      ▼
git commit
      │
      ▼
git push
```

---

# Step 1 — Verify Source File

## Command

```bash
ls -l /tmp/sample.html
```

---

## Sample Output

```text
-rw-r--r-- 1 developer developer 27 Jun 27 sample.html
```

---

## Explanation

Before copying a file, always verify that it exists.

The output displays:

- File permissions
- Owner
- Group
- Size
- Modification date
- Filename

---

# Understanding ls -l Output

```text
-rw-r--r--
```

Meaning

| Symbol | Description |
|----------|------------|
| - | Regular file |
| rw- | Owner has Read & Write |
| r-- | Group has Read |
| r-- | Others have Read |

---

# Step 2 — Copy File to Remote Server

## Command

```bash
scp /tmp/sample.html developer@git-storage01:/tmp/
```

---

## Explanation

SCP means:

```text
Secure Copy Protocol
```

It securely transfers files using SSH.

Syntax:

```bash
scp source destination
```

Example:

```bash
scp file.txt user@server:/tmp/
```

---

# Authentication

The first connection may display:

```text
The authenticity of host cannot be established.

Are you sure you want to continue connecting?
```

Answer:

```text
yes
```

Then provide the user password.

---

# Step 3 — Login to Remote Server

## Command

```bash
ssh developer@git-storage01
```

---

## Explanation

SSH creates an encrypted terminal session.

Benefits:

- Secure communication
- Remote administration
- Encrypted authentication
- Command execution

---

# Step 4 — Navigate to Repository

## Command

```bash
cd /usr/src/projects/web-application
```

Verify location:

```bash
pwd
```

Expected:

```text
/usr/src/projects/web-application
```

---

# Step 5 — Verify Repository

## Command

```bash
ls -ltr
```

Example Output

```text
total 8

welcome.txt

info.txt
```

---

# Step 6 — Check Git Status

## Command

```bash
git status
```

---

# Initial Error

```text
fatal: detected dubious ownership in repository
```

---

# Root Cause

Starting from Git 2.35, Git introduced:

```text
Safe Directory Protection
```

Git refuses to operate inside repositories owned by another user.

This prevents malicious repositories from executing unexpected Git operations.

---

# Resolution

Run:

```bash
git config --global --add safe.directory /usr/src/projects/web-application
```

---

# Verify

```bash
git status
```

Output

```text
On branch master

nothing to commit

working tree clean
```

---

# Understanding Git Safe Directory

Git now verifies:

Repository Owner

↓

Current User

↓

Trusted?

If No

↓

Operation Blocked

---

# Step 7 — Copy File into Repository

Initial attempt

```bash
cp /tmp/sample.html .
```

---

# Error

```text
Permission denied
```

---

# Root Cause

Repository directory ownership belonged to another user.

Linux permissions prevented writing files.

---

# Verify Permissions

```bash
ls -ld .
```

Example

```text
drwxr-xr-x root root
```

Meaning

Owner:

```text
root
```

Current User:

```text
developer
```

Developer cannot write.

---

# Resolution

```bash
sudo cp /tmp/sample.html .
```

---

# Verify

```bash
ls -ltr
```

Output

```text
sample.html

welcome.txt

info.txt
```

---

# Step 8 — Stage Changes

Attempt

```bash
git add sample.html
```

---

# Error

```text
fatal:

Unable to create

.git/index.lock

Permission denied
```

---

# Root Cause

Git creates

```text
.git/index.lock
```

during every modifying operation.

Current user lacked permission.

---

# Resolution

```bash
sudo git add sample.html
```

---

# Verify

```bash
git status
```

Output

```text
Changes to be committed:

new file:

sample.html
```

---

# Understanding Git Staging

```text
Working Directory
        │
 git add
        ▼
Staging Area
        │
 git commit
        ▼
Local Repository
```

The staging area allows reviewing changes before committing.

---

# Step 9 — Commit Changes

## Command

```bash
sudo git commit -m "Add sample HTML page"
```

---

# Output

```text
[master]

Add sample HTML page

1 file changed

create mode 100644 sample.html
```

---

# Commit Message Best Practices

Good

```text
Add login page

Fix authentication bug

Update README

Add Dockerfile
```

Avoid

```text
test

abc

changes

update
```

Meaningful messages improve project history.

---

# Step 10 — Push Changes

Initial attempt

```bash
git push origin master
```

---

# Error

```text
fatal:

detected dubious ownership

Could not read from remote repository
```

---

# Root Cause

The remote Bare Repository was also owned by another user.

Git prevented writing.

---

# Resolution

```bash
sudo git push origin master
```

---

# Successful Output

```text
Enumerating objects...

Counting objects...

Writing objects...

master -> master
```

---

# Verify Current Branch

```bash
git branch
```

Output

```text
* master
```

---

# End-to-End Git Workflow

```text
Developer
      │
      ▼
Working Directory
      │
git add
      ▼
Staging Area
      │
git commit
      ▼
Local Repository
      │
git push
      ▼
Remote Bare Repository
```

---

# Commands Used

```bash
ls -l /tmp/sample.html

scp /tmp/sample.html developer@git-storage01:/tmp/

ssh developer@git-storage01

cd /usr/src/projects/web-application

git config --global --add safe.directory /usr/src/projects/web-application

git status

sudo cp /tmp/sample.html .

sudo git add sample.html

sudo git commit -m "Add sample HTML page"

sudo git push origin master

git branch
```

---

# Linux Concepts Learned

| Command | Purpose |
|----------|----------|
| ssh | Remote login |
| scp | Secure file transfer |
| sudo | Execute command with elevated privileges |
| pwd | Display current directory |
| ls | List files |
| cp | Copy files |
| cd | Change directory |

---

# Git Concepts Learned

| Command | Purpose |
|----------|----------|
| git status | Repository state |
| git add | Stage changes |
| git commit | Save locally |
| git push | Upload commits |
| git branch | Display branch |
| safe.directory | Trust repository |
| index.lock | Prevent concurrent Git operations |

---

# Why Git Creates index.lock

Git modifies its index database during:

- git add
- git commit
- git merge
- git rebase

To prevent corruption:

```text
Create index.lock

↓

Modify Index

↓

Delete index.lock
```

If Git cannot create this file:

Operations fail.

---

# Real Enterprise Scenario

Suppose:

A Jenkins Pipeline clones a repository.

The repository owner becomes:

```text
jenkins
```

A Linux administrator logs in as:

```text
developer
```

Git refuses operations until:

```bash
git config --global --add safe.directory <repository>
```

This situation is common in CI/CD servers.

---

# Best Practices

- Verify repository ownership.
- Avoid unnecessary use of sudo.
- Use meaningful commit messages.
- Stage only intended files.
- Always verify git status before committing.
- Push only reviewed commits.
````
================================================================

## Scenario-Based Interview Questions & Answers

# L1 – Junior DevOps / Git Engineer

---

## Q1. What is Git?

### Answer

Git is a distributed version control system used to track changes in source code. It enables multiple developers to collaborate efficiently while maintaining a complete history of changes.

---

## Q2. What is the difference between Git and GitHub?

### Answer

| Git                    | GitHub                                     |
| ---------------------- | ------------------------------------------ |
| Version Control System | Cloud-based Git hosting platform           |
| Installed locally      | Accessible via browser                     |
| Tracks source code     | Hosts repositories and collaboration tools |

---

## Q3. What is a repository?

### Answer

A repository (repo) is a storage location that contains source code, version history, branches, tags, and other Git metadata.

---

## Q4. What is the difference between a Bare Repository and a Working Repository?

### Answer

**Bare Repository**

* Contains only Git metadata
* No working directory
* Used as a central remote repository

**Working Repository**

* Contains actual project files
* Developers edit files here
* Connected to a remote repository

---

## Q5. Explain the Git workflow.

### Answer

```text
Working Directory
        │
git add
        ▼
Staging Area
        │
git commit
        ▼
Local Repository
        │
git push
        ▼
Remote Repository
```

---

## Q6. What does `git status` do?

### Answer

Displays:

* Current branch
* Staged files
* Modified files
* Untracked files
* Commit status

---

## Q7. Difference between `git add .` and `git add <file>`?

### Answer

`git add .`

* Stages all modified and new files.

`git add index.html`

* Stages only the specified file.

---

## Q8. Why is SCP used?

### Answer

SCP (Secure Copy Protocol) securely transfers files between Linux systems using SSH encryption.

---

## Q9. Why is SSH preferred over Telnet?

### Answer

SSH encrypts all communication and supports secure authentication, while Telnet transmits data in plain text.

---

## Q10. What is a commit?

### Answer

A commit is a snapshot of the staged changes stored permanently in the Git history.

---

# L2 – Mid-Level DevOps Engineer

---

## Q11. Explain Git Safe Directory.

### Answer

Git 2.35 introduced the Safe Directory feature to prevent Git operations inside repositories owned by another user unless they are explicitly trusted.

Example:

```bash
git config --global --add safe.directory /path/to/repository
```

---

## Q12. Why did Git show "detected dubious ownership"?

### Answer

Because:

* Repository owner ≠ Current user

Git blocks operations to protect against repository ownership attacks.

---

## Q13. What is `.git/index.lock`?

### Answer

It is a temporary lock file created by Git to prevent multiple processes from modifying the repository index simultaneously.

---

## Q14. Why did `.git/index.lock` creation fail?

### Answer

Possible reasons:

* Insufficient permissions
* Read-only filesystem
* Another Git process already running

---

## Q15. Difference between `git fetch` and `git pull`.

### Answer

`git fetch`

Downloads changes without merging.

`git pull`

Downloads changes and immediately merges them into the current branch.

---

## Q16. Explain HEAD.

### Answer

HEAD points to the currently checked-out commit.

Usually:

```text
HEAD → master
```

---

## Q17. Difference between clone and fork.

### Answer

Clone:

Creates a local copy.

Fork:

Creates your own copy on a Git hosting platform.

---

## Q18. What happens during `git push`?

### Answer

Git transfers committed objects from the local repository to the configured remote repository.

---

## Q19. What is a merge conflict?

### Answer

A merge conflict occurs when Git cannot automatically reconcile changes made to the same lines of a file.

---

## Q20. Why should commit messages be meaningful?

### Answer

Good commit messages improve:

* debugging
* auditing
* collaboration
* release tracking

---

# L3 – Senior DevOps Engineer

---

## Q21. How would you troubleshoot Git permission issues?

### Answer

Check:

```bash
ls -ld repository
```

Verify ownership:

```bash
ls -l
```

Verify user:

```bash
whoami
```

Check Git configuration:

```bash
git config --list
```

Verify safe directories:

```bash
git config --global --get-all safe.directory
```

---

## Q22. Why should Git operations generally not be run using sudo?

### Answer

Running Git as root may:

* create root-owned files
* break collaboration
* complicate repository permissions

Use `sudo` only when required by ownership or administrative tasks.

---

## Q23. Explain Git internals.

### Answer

Git stores:

* blobs (file contents)
* trees (directory structure)
* commits
* tags

inside:

```text
.git/objects/
```

---

## Q24. How would you recover from an accidental commit?

### Answer

Options include:

```bash
git reset
```

or

```bash
git revert
```

depending on whether the commit has been pushed.

---

## Q25. How do you secure enterprise Git repositories?

### Answer

Implement:

* SSH keys
* Branch protection
* Signed commits
* Pull request reviews
* Secret scanning
* RBAC

---

## Q26. Explain Git hooks.

### Answer

Git hooks are scripts that run automatically during Git events.

Examples:

* pre-commit
* commit-msg
* post-merge
* pre-push

---

## Q27. What is Git LFS?

### Answer

Git Large File Storage manages large binary files without bloating repository history.

---

## Q28. Explain branching strategy.

### Answer

Typical branches:

* main
* develop
* feature/*
* release/*
* hotfix/*

---

## Q29. Difference between merge and rebase.

### Answer

Merge preserves history.

Rebase rewrites history into a linear sequence.

---

## Q30. How would you troubleshoot a failed push?

### Answer

Check:

* authentication
* remote URL
* branch permissions
* repository ownership
* network connectivity

---

# L4 – DevOps Architect

---

## Q31. Design an enterprise Git architecture.

### Answer

Components include:

* Central Git server
* Backup repositories
* CI/CD integration
* Branch protection
* Code review workflow
* RBAC
* Audit logging
* Disaster recovery

---

## Q32. How would Git integrate with CI/CD?

### Answer

Workflow:

```text
Developer
    │
Push
    ▼
Git Server
    │
Webhook
    ▼
CI/CD Pipeline
    │
Build
    │
Test
    │
Security Scan
    │
Deploy
```

---

## Q33. What security controls should enterprise Git use?

### Answer

* SSH authentication
* MFA
* Signed commits
* Protected branches
* Repository scanning
* Least privilege

---

## Q34. How do you manage multiple environments?

### Answer

Separate:

* development
* testing
* staging
* production

Each environment should have its own deployment pipeline.

---

## Q35. Explain GitOps.

### Answer

GitOps stores infrastructure and application configuration inside Git.

Git becomes the single source of truth.

---

## Q36. How would you implement disaster recovery for Git?

### Answer

* Mirror repositories
* Scheduled backups
* Multi-region replication
* Snapshot retention
* Repository integrity checks

---

## Q37. What metrics would you monitor?

### Answer

* Commit frequency
* Build success rate
* Deployment frequency
* Lead time
* Mean time to recovery (MTTR)

---

## Q38. How would you secure the software supply chain?

### Answer

Implement:

* SBOM generation
* Dependency scanning
* Signed commits
* Artifact signing
* Immutable builds

---

## Q39. Explain Infrastructure as Code integration.

### Answer

Git stores Terraform, Ansible, Kubernetes manifests, and deployment pipelines, enabling version-controlled infrastructure changes.

---

## Q40. Why is Git considered the single source of truth?

### Answer

Because it stores:

* source code
* infrastructure
* deployment configuration
* pipeline definitions
* documentation

allowing reproducible deployments and complete auditability.

---

# Common Mistakes

| Mistake                           | Impact                   | Prevention                |
| --------------------------------- | ------------------------ | ------------------------- |
| Using `git add .` carelessly      | Stages unwanted files    | Stage only required files |
| Poor commit messages              | Difficult history        | Use descriptive messages  |
| Pushing directly to production    | Risky deployments        | Use pull requests         |
| Running Git as root unnecessarily | Ownership issues         | Use proper permissions    |
| Ignoring `.gitignore`             | Unwanted files committed | Maintain `.gitignore`     |

---

# Real-World DevOps Use Cases

* CI/CD pipelines
* Infrastructure as Code repositories
* Application deployments
* Kubernetes manifests
* Docker image builds
* Configuration management
* Release management
* Automated testing

---

# Key Takeaways

* Understand the Git workflow from working directory to remote repository.
* Use `scp` and `ssh` for secure file transfer and remote administration.
* Recognize and resolve Git Safe Directory issues.
* Troubleshoot Linux permission problems affecting Git.
* Use meaningful commit messages and follow Git best practices.
* Integrate Git with CI/CD and GitOps workflows.
* Apply enterprise security controls for repository management.

---

# Final Outcome

The project was successfully updated by:

* Transferring the required file to the Git server.
* Adding the file to the working repository.
* Creating a commit with a meaningful message.
* Pushing the changes to the `master` branch.
* Resolving Git ownership and Linux permission issues encountered during the process.

This exercise demonstrates practical skills in Linux administration, Git repository management, troubleshooting, and DevOps workflows commonly encountered in enterprise environments.

