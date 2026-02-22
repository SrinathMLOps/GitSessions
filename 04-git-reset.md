# Git Reset Commands

## Understanding Git Reset

Git reset allows you to move files between the three phases: Workspace, Staging, and Local Repository.

```
Local Repo → Staging → Workspace
     ↓          ↓         ↓
  (--soft)  (--mixed)  (--hard)
```

## Types of Reset

1. **git reset HEAD** - Move from Staging to Workspace
2. **git reset --soft** - Move from Local Repo to Staging
3. **git reset --mixed** - Move from Local Repo to Workspace
4. **git reset --hard** - Delete commits and files

## 1. Moving from Staging to Workspace

### Using git reset HEAD

```bash
# Add files to staging
git add file1.txt file2.txt
git status
# Files are staged

# Move back to workspace
git reset HEAD file1.txt

# Or move all files
git reset HEAD .
# Or
git reset HEAD *
```

### Using git restore

```bash
# Alternative method
git restore --staged <file>

# Example
git add app.js
git restore --staged app.js
```

### Example

```bash
# Create and stage files
touch feature.js style.css
git add feature.js style.css
git status
# Both files staged

# Unstage one file
git reset HEAD feature.js
git status
# feature.js: untracked
# style.css: staged

# Unstage all
git reset HEAD .
git status
# Both files untracked
```

## 2. Git Reset --soft (Local Repo → Staging)

Moves committed files back to staging area without losing changes.

```bash
# View commits
git log --oneline

# Reset to previous commit (moves to staging)
git reset --soft <commit_id>

# Check status
git status
# Files are now staged
```

### Example

```bash
# Create and commit files
touch file1.txt file2.txt
git add .
git commit -m "Add two files"

# View log
git log --oneline
# abc123 Add two files
# def456 Previous commit

# Reset to previous commit
git reset --soft def456

# Check status
git status
# file1.txt and file2.txt are staged
# Commit is undone but files remain
```

## 3. Git Reset --mixed (Local Repo → Workspace)

Moves committed files back to workspace. This is the default reset mode.

```bash
# Reset to previous commit (moves to workspace)
git reset --mixed <commit_id>

# Or simply
git reset <commit_id>

# Check status
git status
# Files are now untracked
```

### Example

```bash
# Commit files
touch app.js
git add app.js
git commit -m "Add app.js"

# View log
git log --oneline
# xyz789 Add app.js
# abc123 Previous commit

# Reset to previous commit
git reset --mixed abc123
# Or: git reset abc123

# Check status
git status
# app.js is untracked (in workspace)
```

### Important Note

`--mixed` brings files from both Local Repo AND Staging to Workspace.

## 4. Git Reset --hard (Delete Everything)

**WARNING:** This permanently deletes commits and changes. Use with caution!

```bash
# Delete commit and all changes
git reset --hard <commit_id>

# Check status
git status
# Files are gone
```

### Example

```bash
# Create and commit files
touch file1.txt file2.txt
git add .
git commit -m "Add two files"

# View log
git log --oneline
# aaa111 Add two files
# bbb222 Previous commit

# Delete the commit and files
git reset --hard bbb222

# Check status
git status
# Clean working tree

# Check files
ls
# file1.txt and file2.txt are deleted
```

## Complete Example: All Reset Types

```bash
# Initialize repository
git init
git config user.name "John Doe"
git config user.email "john@example.com"

# Scenario 1: Unstage files
echo "content" > file1.txt
git add file1.txt
git status
# file1.txt staged

git reset HEAD file1.txt
git status
# file1.txt untracked

# Scenario 2: Reset --soft
git add file1.txt
git commit -m "Add file1"

echo "content" > file2.txt
git add file2.txt
git commit -m "Add file2"

git log --oneline
# ccc333 Add file2
# ddd444 Add file1

git reset --soft ddd444
git status
# file2.txt is staged

# Scenario 3: Reset --mixed
git commit -m "Add file2 again"
git log --oneline
# eee555 Add file2 again
# ddd444 Add file1

git reset --mixed ddd444
git status
# file2.txt is untracked

# Scenario 4: Reset --hard
git add file2.txt
git commit -m "Add file2 final"

git log --oneline
# fff666 Add file2 final
# ddd444 Add file1

git reset --hard ddd444
git status
# Clean working tree
ls
# Only file1.txt exists
```

## Visual Summary

```
WORKSPACE          STAGING           LOCAL REPO
---------          -------           ----------
file.txt    →     file.txt    →     [commit]
         git add         git commit

[commit]    ←     file.txt    ←     [commit]
         git reset HEAD    git reset --soft

file.txt    ←     -------     ←     [commit]
              git reset --mixed

---------   ←     -------     ←     [deleted]
              git reset --hard
```

## When to Use Each Reset

| Command | Use Case |
|---------|----------|
| `git reset HEAD` | Unstage files, keep changes |
| `git reset --soft` | Undo commit, keep files staged |
| `git reset --mixed` | Undo commit, keep files in workspace |
| `git reset --hard` | Completely remove commit and changes |

## Best Practices

1. **Use --soft** when you want to recommit with a different message
2. **Use --mixed** when you want to modify files before recommitting
3. **Use --hard** only when you're absolutely sure you want to delete changes
4. **Always check** `git status` after reset
5. **Be careful** with --hard on shared branches

## Next Steps

Learn about [Branching and Merging](./05-branching-merging.md) to work on features independently.
