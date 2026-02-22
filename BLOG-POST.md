# Git Mastery: From Zero to Hero

A comprehensive guide to mastering Git version control for developers.

## Introduction

Git is the most popular version control system used by developers worldwide. Whether you're working solo or collaborating with a team, understanding Git is essential for modern software development. This guide will take you from complete beginner to confident Git user.

## What You'll Learn

- Git fundamentals and core concepts
- Essential commands for daily use
- Branching strategies and workflows
- Conflict resolution techniques
- Advanced Git features
- Best practices and tips

## Prerequisites

- Basic command line knowledge
- Git installed on your system ([download here](https://git-scm.com))

---

## Part 1: Getting Started

### Installing and Configuring Git

First, verify Git is installed:

```bash
git --version
```

Configure your identity (required for commits):

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

Verify your configuration:

```bash
git config --list
```

---

## Part 2: Understanding the Three Phases

Git manages files through three distinct areas:

```
┌─────────────┐    git add    ┌─────────────┐   git commit   ┌──────────────┐
│  Workspace  │ ────────────> │   Staging   │ ─────────────> │  Local Repo  │
│  (Working)  │               │   (Index)   │                │  (Commits)   │
└─────────────┘               └─────────────┘                └──────────────┘
```

### Workspace
Where you create and edit files.

### Staging Area
Where you prepare files for commit.

### Local Repository
Where committed files are stored permanently.

### Example Workflow

```bash
# Initialize repository
git init

# Create a file (Workspace)
echo "Hello Git" > hello.txt

# Add to staging
git add hello.txt

# Commit to repository
git commit -m "Add hello.txt"

# Check status at any time
git status
```

---

## Part 3: Essential Daily Commands

### Checking Status

```bash
git status  # See what's changed
```

### Adding Files

```bash
git add file.txt        # Add specific file
git add .               # Add all files
git add *.js            # Add all JavaScript files
```

### Committing Changes

```bash
git commit -m "Descriptive message"
```

### Viewing History

```bash
git log                 # Full history
git log --oneline       # Compact view
git log --graph         # Visual graph
```

### Undoing Changes

```bash
# Unstage file
git restore --staged file.txt

# Discard changes in workspace
git restore file.txt

# Undo last commit (keep changes)
git reset --soft HEAD~1

# Undo last commit (discard changes)
git reset --hard HEAD~1
```

---

## Part 4: Branching Like a Pro

Branches allow you to work on features independently without affecting the main codebase.

```
main:     A───B───C───F
               \     /
feature:        D───E
```

### Creating and Switching Branches

```bash
# Create branch
git branch feature-login

# Switch to branch
git checkout feature-login

# Create and switch (shortcut)
git checkout -b feature-login
```

### Working with Branches

```bash
# List branches
git branch              # Local branches
git branch -r           # Remote branches
git branch -a           # All branches

# Delete branch
git branch -d feature-login
```

### Merging Branches

```bash
# Switch to target branch
git checkout main

# Merge feature branch
git merge feature-login
```

---

## Part 5: Working with Remote Repositories

### Cloning a Repository

```bash
git clone https://github.com/username/repo.git
cd repo
```

### Pushing Changes

```bash
# Push to remote
git push

# Push new branch
git push -u origin feature-branch
```

### Pulling Changes

```bash
# Pull and merge
git pull

# Pull and rebase (cleaner history)
git pull --rebase
```

### Syncing Your Work

```bash
# Before starting work
git pull --rebase

# After completing work
git push
```

---

## Part 6: Handling Merge Conflicts

Conflicts occur when the same lines are modified in different branches.

### Conflict Markers

```
<<<<<<< HEAD
Your changes
=======
Their changes
>>>>>>> feature-branch
```

### Resolving Conflicts

1. Open the conflicted file
2. Choose which changes to keep
3. Remove conflict markers
4. Save the file
5. Add and commit

```bash
# After resolving
git add resolved-file.txt
git commit -m "Resolve merge conflict"
```

### Aborting a Merge

```bash
git merge --abort
```

---

## Part 7: Git Stash - Your Safety Net

Stash temporarily saves your work when you need to switch contexts.

```bash
# Save work
git stash save "Work in progress"

# List stashes
git stash list

# Apply and remove stash
git stash pop

# Apply and keep stash
git stash apply

# Delete stash
git stash drop
```

### Real-World Example

```bash
# Working on feature
echo "feature code" >> app.js
git add app.js

# Urgent bug fix needed!
git stash save "Feature in progress"

# Fix bug
git checkout main
echo "bug fix" >> bugfix.js
git add bugfix.js
git commit -m "Fix urgent bug"
git push

# Return to feature
git checkout feature-branch
git stash pop
# Continue working
```

---

## Part 8: Tags for Releases

Tags mark important points in history, typically releases.

```bash
# Create tag
git tag v1.0.0

# Create annotated tag
git tag -a v1.0.0 -m "First stable release"

# Push tags
git push --tags

# List tags
git tag

# Checkout tag
git checkout v1.0.0
```

### Semantic Versioning

```
v1.0.0
│ │ │
│ │ └─ PATCH (bug fixes)
│ └─── MINOR (new features, backward compatible)
└───── MAJOR (breaking changes)
```

---

## Part 9: Advanced Techniques

### Git Rebase

Rebase rewrites history for a cleaner commit log.

```bash
# Update feature branch with main
git checkout feature-branch
git rebase main
```

### Git Cherry-Pick

Apply specific commits to another branch.

```bash
# Pick specific commit
git cherry-pick abc123
```

### Git Amend

Modify the last commit.

```bash
# Change commit message
git commit --amend -m "Better message"

# Add forgotten file
git add forgotten.txt
git commit --amend --no-edit
```

### Git Revert

Safely undo a commit by creating a new commit.

```bash
git revert abc123
```

---

## Part 10: Best Practices

### Commit Messages

Good commit messages are crucial:

```bash
# Bad
git commit -m "fix"

# Good
git commit -m "Fix login validation bug"

# Better
git commit -m "Fix: Validate email format before login attempt

- Add regex pattern for email validation
- Show error message for invalid emails
- Add unit tests for validation logic"
```

### Branching Strategy

```bash
main          # Production-ready code
develop       # Integration branch
feature/*     # New features
bugfix/*      # Bug fixes
hotfix/*      # Urgent production fixes
release/*     # Release preparation
```

### Workflow Tips

1. **Commit often**: Small, focused commits
2. **Pull before push**: Avoid conflicts
3. **Use branches**: Never work directly on main
4. **Write clear messages**: Explain what and why
5. **Review before commit**: Use `git diff`
6. **Keep history clean**: Use rebase for local branches
7. **Tag releases**: Mark important versions

---

## Part 11: Common Scenarios

### Scenario 1: Undo Last Commit

```bash
# Keep changes
git reset --soft HEAD~1

# Discard changes
git reset --hard HEAD~1
```

### Scenario 2: Change Last Commit Message

```bash
git commit --amend -m "Correct message"
```

### Scenario 3: Move Commit to Different Branch

```bash
# On wrong branch
git log --oneline  # Note commit ID

# Switch to correct branch
git checkout correct-branch
git cherry-pick abc123

# Remove from wrong branch
git checkout wrong-branch
git reset --hard HEAD~1
```

### Scenario 4: Recover Deleted Branch

```bash
# Find commit
git reflog

# Recreate branch
git checkout -b recovered-branch abc123
```

---

## Part 12: Troubleshooting

### Problem: Merge Conflict

```bash
# Solution
git status              # See conflicted files
# Edit files to resolve
git add resolved-files
git commit
```

### Problem: Pushed Wrong Commit

```bash
# Solution: Revert (safe for shared branches)
git revert abc123
git push
```

### Problem: Detached HEAD

```bash
# Solution: Create branch
git checkout -b new-branch
```

### Problem: Large File Committed

```bash
# Solution: Remove from history
git rm --cached large-file
echo "large-file" >> .gitignore
git commit --amend
```

---

## Part 13: Git Aliases

Save time with aliases:

```bash
# Create aliases
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.lg "log --oneline --graph --all"

# Use them
git st
git co main
git br
git ci -m "message"
git lg
```

---

## Part 14: .gitignore

Specify files Git should ignore:

```bash
# Create .gitignore
cat > .gitignore << EOF
# Dependencies
node_modules/
vendor/

# Environment
.env
.env.local

# Build
dist/
build/
*.log

# IDE
.vscode/
.idea/
*.swp

# OS
.DS_Store
Thumbs.db
EOF

git add .gitignore
git commit -m "Add gitignore"
```

---

## Conclusion

Git is a powerful tool that becomes intuitive with practice. Start with the basics, experiment with branches, and gradually incorporate advanced features into your workflow.

### Key Takeaways

- Understand the three phases: Workspace, Staging, Repository
- Use branches for all new work
- Commit often with clear messages
- Pull before push to avoid conflicts
- Use stash when switching contexts
- Tag your releases
- Practice regularly

### Next Steps

1. Practice these commands on a test repository
2. Contribute to an open-source project
3. Learn about Git hooks and automation
4. Explore Git GUIs (GitKraken, SourceTree, GitHub Desktop)
5. Master your team's Git workflow

### Resources

- [Official Git Documentation](https://git-scm.com/doc)
- [Pro Git Book](https://git-scm.com/book) (free)
- [GitHub Learning Lab](https://lab.github.com)
- [Atlassian Git Tutorials](https://www.atlassian.com/git/tutorials)

---

## Quick Reference Card

```bash
# Setup
git config --global user.name "Name"
git config --global user.email "email"

# Basics
git init                    # Initialize repo
git clone <url>            # Clone repo
git status                 # Check status
git add <file>             # Stage file
git commit -m "msg"        # Commit
git log --oneline          # View history

# Branching
git branch <name>          # Create branch
git checkout <name>        # Switch branch
git checkout -b <name>     # Create and switch
git merge <branch>         # Merge branch
git branch -d <name>       # Delete branch

# Remote
git push                   # Push changes
git pull                   # Pull changes
git pull --rebase          # Pull with rebase

# Undo
git restore --staged <file>  # Unstage
git reset --soft HEAD~1      # Undo commit
git reset --hard HEAD~1      # Undo and discard

# Stash
git stash save "msg"       # Save work
git stash pop              # Apply and remove
git stash list             # List stashes

# Tags
git tag v1.0.0             # Create tag
git push --tags            # Push tags
```

---

Happy coding! May your commits be clean and your merges conflict-free! 🚀
