---
layout: post
title:  "Basics of Git"
date:   2025-12-06 11:00:00 -0500
author: Ravi Shankar Gurram
categories: ['Linux']
background: ''
---

## Basics of Shell Scripting

Git is a powerful distributed version control system that helps developers track changes in their code and collaborate with others. Here are some essential Git commands to help you use Git effectively:

## Setting Up Your Git Environment
1.	Configure Git: Set your username and email for Git commits.
    - `git config --global user.name "Your Name"`
    - `git config --global user.email "you@example.com"`

## Initialize a repository
1.	Initializing a Repository: Create a new Git repository in your project directory.
    - `git init`
    - This command creates a .git directory in your project, which tracks the version history.

## Tracking and Staging Changes
1.	Check Status: See the status of your working directory and staging area.
    - `git status`
2.	Stage Changes: Add files to the staging area.
    - `git add <file>`
3. To stage all changes, use:
    - `git add .`

## Committing Changes
1.	Commit Changes: Save your staged changes to the repository.
    - `git commit -m "Your commit message"`

## Branching and Merging
1.	Create a Branch: Create a new branch.
    - `git branch <branch_name>`
2.	Switch Branches: Switch to a different branch.
    - `git checkout <branch_name>`
3.	Merge Branches: Merge a branch into your current branch.
    - `git merge <branch_name>`
 
 
## Collaborating with Others
1.	Fetch Changes: Fetch changes from the remote repository without merging.
    - `git fetch <remote_name>`
2.	Pull Changes: Fetch and merge changes from the remote repository. 
    - `git pull <remote_name> <branch_name>`
3.	Push Changes: Push your local changes to the remote repository.
    - `git push <remote_name> <branch_name>`

## Undoing Changes
1.	Reset Changes: Undo changes in your working directory. 
    - `git reset <file>`
2.	Revert a Commit: Create a new commit that undoes a previous commit. 
    - `git revert <commit_hash>`

## Inspecting a Repository
1.	View Commit History: Show the commit history.
    - `git log`
2.	Show Changes: Display changes between commits, branches, or files.
    - `git diff`

## Git Tags
Git tags are used to mark specific points in a repository's history, often for releases or important milestones. Tags are like branches that don't change; once created, they serve as snapshots of your code at a given point. There are two main types of tags in Git: lightweight tags and annotated tags.
### Lightweight Tags
- Lightweight tags are simple pointers to a specific commit.
- They don't store any additional information beyond the commit hash.
- Useful for quick, temporary tags.
To create a lightweight tag:
git tag <tag_name>
### Annotated Tags
- Annotated tags store additional metadata such as the tagger's name, email, date, and a message.
- They are stored as full objects in the Git database and can be signed and verified with GPG.
- Recommended for public releases due to the extra information they provide.
- To create an annotated tag:
    - `git tag -a <tag_name> -m "Tag message"`
### Listing Tags
- To list all tags in your repository:
    - `git tag`
### Viewing Tag Details
- To view details of an annotated tag:
    - `git show <tag_name>`
### Pushing Tags to Remote
- To push a specific tag to a remote repository:
    - `git push <remote_name> <tag_name>`
### To push all tags:
    - `git push <remote_name> --tags`
### Deleting Tags
- To delete a local tag:
    - `git tag -d <tag_name>`
- To delete a remote tag:
    - `git push <remote_name> --delete <tag_name>`

## Working with Repositories
3.	Clone a Repository: Copy an existing Git repository to your local machine.
    - `git clone <repository_url>`
    - This command downloads the repository from the specified URL to your local machine.