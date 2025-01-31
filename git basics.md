# GIT Cheatsheet

## Repository Setup

```sh
git init    # Initialize a new Git repo
git clone <repo-url>    # Clone a repo from URL
git config --global user.name "Your Name"    # Set username
git config --global user.email "your.email@example.com"    # Set email
```

## Basic Commands
```sh
git status    # Show changes status
git add <file>    # Stage specific file
git add .    # Stage all changes
git commit -m "Message"    # Commit with message
git commit -am "Message"    # Add tracked & commit
git log    # View commit history
```

## Branching
```sh
git branch    # List local branches
git branch -a    # List all branches
git branch <name>    # Create new branch
git checkout <name>    # Switch to branch
git checkout -b <name>    # Create & switch branch
git merge <branch>    # Merge branch into current
git branch -d <name>    # Delete a branch
```

## Remote Operations
```sh
git remote    # List remotes
git push <remote> <branch>    # Push to remote
git pull <remote> <branch>    # Pull from remote
```

