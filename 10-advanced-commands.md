# Advanced Git Commands

## Git Rebase

Rebase rewrites commit history by moving commits to a new base.

### Rebase vs Merge

```
Merge:                  Rebase:
A---B---C main          A---B---C---D'---E' main
     \                       
      D---E feature     
```

### Basic Rebase

```bash
# Update feature branch with main
git checkout feature-branch
git rebase main

# If conflicts occur
# 1. Resolve conflicts
# 2. git add <resolved_files>
# 3. git rebase --continue

# Or abort
git rebase --abort
```

### Example: Rebase Workflow

```bash
# Create feature branch
git checkout -b feature-payment
echo "payment code" > payment.js
git add payment.js
git commit -m "Add payment"

# Meanwhile, main branch updated
git checkout main
echo "main update" > main.js
git add main.js
git commit -m "Update main"

# Rebase feature on top of main
git checkout feature-payment
git rebase main

# Result: payment commit is now after main update
git log --oneline
# abc123 Add payment (moved)
# def456 Update main
```

### Pull with Rebase

```bash
# Instead of merge, use rebase when pulling
git pull --rebase

# This keeps history linear
```

## Git Cherry-Pick

Cherry-pick applies specific commits from one branch to another.

```bash
# Apply specific commit to current branch
git cherry-pick <commit_id>
```

### Example: Cherry-Pick

```bash
# On main branch
git checkout main
echo "feature 1" > f1.txt
git add f1.txt
git commit -m "Feature 1"

echo "feature 2" > f2.txt
git add f2.txt
git commit -m "Feature 2"

echo "feature 3" > f3.txt
git add f3.txt
git commit -m "Feature 3"

# Create release branch
git checkout -b release

# Cherry-pick only Feature 2
git log main --oneline
# abc123 Feature 3
# def456 Feature 2
# ghi789 Feature 1

git cherry-pick def456

# Now release has only Feature 2
git log --oneline
# def456 Feature 2
```

### Multiple Cherry-Picks

```bash
# Cherry-pick multiple commits
git cherry-pick commit1 commit2 commit3

# Cherry-pick range
git cherry-pick commit1^..commit3
```

## Git Commit --amend

Modify the last commit.

### Change Commit Message

```bash
# Change last commit message
git commit --amend -m "new message"
```

### Add Files to Last Commit

```bash
# Forgot to add a file
touch forgotten.txt
git add forgotten.txt

# Add to last commit (no new commit created)
git commit --amend --no-edit

# Or with new message
git commit --amend -m "Add files including forgotten"
```

### Example: Amend Workflow

```bash
# Make commit
echo "code" > app.js
git add app.js
git commit -m "Add app"

# Oops! Forgot style.css
echo "styles" > style.css
git add style.css

# Add to previous commit
git commit --amend --no-edit

# Check log
git log --oneline
# Only one commit with both files
```

## Git Revert

Undo a commit by creating a new commit that reverses changes.

```bash
# Revert a commit
git revert <commit_id>

# Revert without committing
git revert -n <commit_id>
```

### Example: Revert

```bash
# Make commits
echo "feature" > feature.js
git add feature.js
git commit -m "Add feature"

git push

# Feature breaks production!
git log --oneline
# abc123 Add feature
# def456 Previous commit

# Revert the feature
git revert abc123

# This creates new commit
git log --oneline
# ghi789 Revert "Add feature"
# abc123 Add feature
# def456 Previous commit

git push
```

### Revert a Revert

```bash
# Revert the revert to bring back changes
git revert ghi789

git log --oneline
# jkl012 Revert "Revert 'Add feature'"
# ghi789 Revert "Add feature"
# abc123 Add feature
```

## Git Diff

Compare changes between files, commits, or branches.

```bash
# Compare working directory with staging
git diff

# Compare staging with last commit
git diff --staged

# Compare two files
git diff file1.txt file2.txt

# Compare two branches
git diff branch1 branch2

# Compare two commits
git diff commit1 commit2
```

### Example: Diff Usage

```bash
# Modify file
echo "line 1" > app.js
git add app.js
git commit -m "Add line 1"

echo "line 2" >> app.js

# See changes
git diff
# +line 2

# Stage changes
git add app.js

# See staged changes
git diff --staged
# +line 2
```

## Git Blame

See who last modified each line of a file.

```bash
# Show blame for entire file
git blame <filename>

# Show blame for specific lines
git blame -L 1,10 <filename>

# Show blame for specific commit
git blame <commit_id> <filename>
```

### Example: Blame

```bash
# Create file with multiple commits
echo "line 1" > code.js
git add code.js
git commit -m "Add line 1" --author="John <john@example.com>"

echo "line 2" >> code.js
git add code.js
git commit -m "Add line 2" --author="Jane <jane@example.com>"

# See who wrote what
git blame code.js
# abc123 (John 2026-02-22) line 1
# def456 (Jane 2026-02-22) line 2

# Blame specific lines
git blame -L 1,1 code.js
# abc123 (John 2026-02-22) line 1
```

## Git Bisect

Binary search to find which commit introduced a bug.

```bash
# Start bisect
git bisect start

# Mark current commit as bad
git bisect bad

# Mark a known good commit
git bisect good <commit_id>

# Git checks out middle commit
# Test it, then mark as good or bad
git bisect good   # or git bisect bad

# Repeat until bug is found
# Git will tell you the first bad commit

# End bisect
git bisect reset
```

### Example: Bisect

```bash
# 10 commits, bug introduced somewhere
git log --oneline
# aaa (HEAD) Commit 10
# bbb Commit 9
# ccc Commit 8
# ...
# jjj Commit 1

# Start bisect
git bisect start
git bisect bad                # Current is bad
git bisect good jjj          # Commit 1 was good

# Git checks out commit 5
# Test: Bug exists
git bisect bad

# Git checks out commit 3
# Test: No bug
git bisect good

# Git checks out commit 4
# Test: Bug exists
git bisect bad

# Git says: "ccc is the first bad commit"
git bisect reset
```

## Git .gitignore

Specify files Git should ignore.

```bash
# Create .gitignore
cat > .gitignore << EOF
# Dependencies
node_modules/
*.log

# Environment
.env
.env.local

# Build
dist/
build/

# IDE
.vscode/
.idea/
EOF

# Add and commit
git add .gitignore
git commit -m "Add gitignore"
```

### Example: Gitignore

```bash
# Create files
touch app.js
touch debug.log
mkdir node_modules
touch node_modules/package.js

# Create .gitignore
echo "*.log" > .gitignore
echo "node_modules/" >> .gitignore

# Check status
git status
# Only shows: app.js and .gitignore
# debug.log and node_modules/ are ignored
```

## Git Remote Update --prune

Remove references to deleted remote branches.

```bash
# Update and clean remote references
git remote update --prune

# Or
git fetch --prune
```

## Git Merge vs Rebase

### When to Use Merge

- Working on public/shared branches
- Want to preserve complete history
- Collaborating with team

```bash
git checkout main
git merge feature-branch
```

### When to Use Rebase

- Cleaning up local commits before pushing
- Want linear history
- Working on personal feature branch

```bash
git checkout feature-branch
git rebase main
```

## Complete Advanced Example

```bash
# Start project
git init
git config user.name "Developer"
git config user.email "dev@example.com"

# Create commits
echo "v1" > app.js
git add app.js
git commit -m "Version 1"

echo "v2" > app.js
git add app.js
git commit -m "Version 2"

echo "v3" > app.js
git add app.js
git commit -m "Version 3"

# Amend last commit
echo "v3 updated" > app.js
git add app.js
git commit --amend -m "Version 3 (updated)"

# Create feature branch
git checkout -b feature
echo "feature code" > feature.js
git add feature.js
git commit -m "Add feature"

# Update main
git checkout main
echo "main update" > main.js
git add main.js
git commit -m "Update main"

# Rebase feature on main
git checkout feature
git rebase main

# Cherry-pick specific commit to new branch
git checkout -b release
git cherry-pick <feature_commit_id>

# View history
git log --oneline --graph --all
```

## Best Practices

1. **Use rebase for local branches**: Keep history clean
2. **Use merge for shared branches**: Preserve collaboration history
3. **Amend only unpushed commits**: Don't rewrite public history
4. **Cherry-pick sparingly**: Can create duplicate commits
5. **Use revert for public commits**: Safe way to undo
6. **Bisect for bug hunting**: Efficient way to find issues

## Quick Reference

```bash
# Rebase
git rebase main
git rebase --continue
git rebase --abort

# Cherry-pick
git cherry-pick <commit_id>

# Amend
git commit --amend -m "new message"
git commit --amend --no-edit

# Revert
git revert <commit_id>

# Diff
git diff
git diff --staged
git diff branch1 branch2

# Blame
git blame <file>
git blame -L 1,10 <file>

# Bisect
git bisect start
git bisect bad
git bisect good <commit_id>
git bisect reset
```

## Next Steps

Check out the [Git Cheat Sheet](./git-cheatsheet.md) for a quick reference of all commands.
