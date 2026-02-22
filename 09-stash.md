# Git Stash

## What is Git Stash?

Git stash temporarily saves your uncommitted changes (staged and modified files) so you can work on something else, then come back to them later.

```
Workspace → Stash (temporary storage)
         ↓
    Clean workspace
         ↓
    Do other work
         ↓
Stash → Workspace (restore changes)
```

## Important Notes

- Stash is LOCAL only (not pushed to GitHub)
- Can stash: staged files and modified files
- Cannot stash: committed files or untracked new files
- Stashes are stored in `.git/refs/stash`

## Basic Stash Commands

```bash
# Save changes to stash
git stash
# Or with a message
git stash save "work in progress"

# List all stashes
git stash list

# View stash contents
git stash show stash@{0}

# View detailed stash contents
git stash show -p stash@{0}
```

## Creating a Stash

### Example 1: Basic Stash

```bash
# Make changes to a file
echo "new feature" >> app.js

# Check status
git status
# modified: app.js

# Stash the changes
git stash save "app.js feature work"

# Check status
git status
# nothing to commit, working tree clean

# List stashes
git stash list
# stash@{0}: On main: app.js feature work
```

### Example 2: Multiple Stashes

```bash
# Modify file 1
echo "change 1" >> file1.txt
git add file1.txt
git stash save "file1 changes"

# Modify file 2
echo "change 2" >> file2.txt
git add file2.txt
git stash save "file2 changes"

# Modify file 3
echo "change 3" >> file3.txt
git add file3.txt
git stash save "file3 changes"

# List all stashes
git stash list
# stash@{0}: On main: file3 changes
# stash@{1}: On main: file2 changes
# stash@{2}: On main: file1 changes
```

## Stash Indexing

Stashes are indexed with `stash@{n}`:
- `stash@{0}` - Most recent stash
- `stash@{1}` - Second most recent
- `stash@{2}` - Third most recent
- And so on...

## Viewing Stash Contents

```bash
# Show files in stash
git stash show stash@{0}

# Output:
# file1.txt | 2 +-
# 1 file changed, 1 insertion(+), 1 deletion(-)

# Show detailed changes
git stash show -p stash@{0}

# Output shows full diff:
# diff --git a/file1.txt b/file1.txt
# +new line added
```

## Retrieving Stashed Changes

### 1. Git Stash Pop (Cut)

Applies stash and removes it from stash list.

```bash
# Apply most recent stash and remove it
git stash pop

# Apply specific stash and remove it
git stash pop stash@{2}
```

#### Example: Pop

```bash
# Create stash
echo "feature" >> app.js
git stash save "app feature"

# List stashes
git stash list
# stash@{0}: On main: app feature

# Pop the stash
git stash pop

# Check status
git status
# modified: app.js

# List stashes
git stash list
# (empty - stash was removed)
```

### 2. Git Stash Apply (Copy)

Applies stash but keeps it in stash list.

```bash
# Apply most recent stash (keep in list)
git stash apply

# Apply specific stash (keep in list)
git stash apply stash@{1}
```

#### Example: Apply

```bash
# Create stash
echo "feature" >> app.js
git stash save "app feature"

# Apply the stash
git stash apply

# Check status
git status
# modified: app.js

# List stashes
git stash list
# stash@{0}: On main: app feature
# (still there!)
```

### 3. Git Stash Drop (Delete)

Removes a stash from the list.

```bash
# Delete most recent stash
git stash drop

# Delete specific stash
git stash drop stash@{1}
```

#### Example: Drop

```bash
# List stashes
git stash list
# stash@{0}: On main: feature 1
# stash@{1}: On main: feature 2

# Drop specific stash
git stash drop stash@{1}

# List stashes
git stash list
# stash@{0}: On main: feature 1
```

## Complete Workflow Examples

### Example 1: Switching Tasks

```bash
# Working on feature A
echo "feature A code" >> featureA.js
git add featureA.js

# Urgent bug fix needed!
# Stash current work
git stash save "Feature A in progress"

# Fix the bug
echo "bug fix" >> bugfix.js
git add bugfix.js
git commit -m "Fix urgent bug"
git push

# Return to feature A
git stash pop
# Continue working on featureA.js
```

### Example 2: Experimenting

```bash
# Make experimental changes
echo "experimental code" >> app.js
git add app.js

# Stash experiment
git stash save "Experimental approach"

# Try different approach
echo "different approach" >> app.js
git add app.js
git commit -m "Implement solution"

# Later, compare with experiment
git stash apply stash@{0}
# Review both approaches
# Keep the better one
git stash drop stash@{0}
```

### Example 3: Multiple Features

```bash
# Feature 1
echo "feature 1" >> f1.js
git add f1.js
git stash save "Feature 1 WIP"

# Feature 2
echo "feature 2" >> f2.js
git add f2.js
git stash save "Feature 2 WIP"

# Feature 3
echo "feature 3" >> f3.js
git add f3.js
git stash save "Feature 3 WIP"

# List all
git stash list
# stash@{0}: Feature 3 WIP
# stash@{1}: Feature 2 WIP
# stash@{2}: Feature 1 WIP

# Work on Feature 2
git stash apply stash@{1}
# Complete and commit
git commit -m "Complete Feature 2"
git stash drop stash@{1}

# Work on Feature 1
git stash pop stash@{1}  # Now it's {1} because we dropped one
# Complete and commit
git commit -m "Complete Feature 1"
```

## Stash with Untracked Files

By default, stash doesn't include untracked files.

```bash
# Include untracked files
git stash save -u "with untracked files"

# Or
git stash save --include-untracked "with untracked"
```

## Clearing All Stashes

```bash
# Delete all stashes
git stash clear

# Warning: This is permanent!
```

## Practical Scenarios

### Scenario 1: Pull Before Push

```bash
# You have local changes
echo "changes" >> app.js
git add app.js

# Try to pull
git pull
# Error: Your local changes would be overwritten

# Stash changes
git stash save "local changes"

# Pull successfully
git pull

# Reapply changes
git stash pop

# Resolve any conflicts if needed
# Then commit
git commit -m "Add changes"
git push
```

### Scenario 2: Wrong Branch

```bash
# Oops! Working on wrong branch
git branch
# * main  (should be on feature branch!)

echo "feature code" >> feature.js
git add feature.js

# Stash the work
git stash save "feature work"

# Switch to correct branch
git checkout feature-branch

# Apply stashed work
git stash pop

# Now commit on correct branch
git commit -m "Add feature"
```

## Stash Best Practices

1. **Use descriptive messages**: `git stash save "descriptive message"`
2. **Don't stash too long**: Apply or commit within a day or two
3. **Clean up old stashes**: Use `git stash drop` for unused stashes
4. **Check stash list regularly**: `git stash list`
5. **Use apply for safety**: Test with `apply` before `pop`

## Quick Reference

```bash
# Create stash
git stash                       # Stash changes
git stash save "message"       # Stash with message
git stash save -u "message"    # Include untracked files

# View stashes
git stash list                 # List all stashes
git stash show stash@{0}       # Show files changed
git stash show -p stash@{0}    # Show detailed changes

# Apply stashes
git stash pop                  # Apply and remove latest
git stash pop stash@{1}        # Apply and remove specific
git stash apply                # Apply and keep latest
git stash apply stash@{1}      # Apply and keep specific

# Delete stashes
git stash drop                 # Delete latest
git stash drop stash@{1}       # Delete specific
git stash clear                # Delete all stashes
```

## Next Steps

Learn about [Advanced Git Commands](./10-advanced-commands.md) including cherry-pick, rebase, amend, and more.
