# Handling Merge Conflicts

## What is a Merge Conflict?

A merge conflict occurs when Git cannot automatically merge changes because the same lines in a file were modified differently in two branches.

```
main:        A---B---C (modified line 5)
                  \
feature:           D (modified line 5 differently)
```

## Creating a Conflict (Example)

### Step 1: Setup

```bash
# Initialize repository
git init
git config user.name "John Doe"
git config user.email "john@example.com"

# Create a file on main
git checkout main
echo "Hello World" > app.txt
git add app.txt
git commit -m "Add app.txt"
```

### Step 2: Create Conflicting Changes

```bash
# Create and switch to feature branch
git checkout -b feature-branch

# Modify the file
echo "Hello from Feature Branch" > app.txt
git add app.txt
git commit -m "Update app.txt in feature"

# Switch back to main
git checkout main

# Modify the same file differently
echo "Hello from Main Branch" > app.txt
git add app.txt
git commit -m "Update app.txt in main"
```

### Step 3: Attempt to Merge

```bash
# Try to merge feature into main
git merge feature-branch

# Output:
# Auto-merging app.txt
# CONFLICT (content): Merge conflict in app.txt
# Automatic merge failed; fix conflicts and then commit the result.

# Check status
git status
# Output shows: both modified: app.txt
# Branch shows: main|MERGING
```

## Understanding Conflict Markers

When you open the conflicted file, you'll see:

```
<<<<<<< HEAD
Hello from Main Branch
=======
Hello from Feature Branch
>>>>>>> feature-branch
```

### Conflict Markers Explained

- `<<<<<<< HEAD` - Start of your current branch changes
- `=======` - Separator between changes
- `>>>>>>> feature-branch` - End of incoming branch changes

## Resolving Conflicts Manually

### Option 1: Keep Current Branch Changes

```bash
# Edit app.txt
# Remove conflict markers and keep main branch content
Hello from Main Branch

# Save the file
# Add resolved file
git add app.txt

# Commit the merge
git commit -m "Resolve merge conflict: keep main version"
```

### Option 2: Keep Incoming Branch Changes

```bash
# Edit app.txt
# Remove conflict markers and keep feature branch content
Hello from Feature Branch

# Save and commit
git add app.txt
git commit -m "Resolve merge conflict: keep feature version"
```

### Option 3: Keep Both Changes

```bash
# Edit app.txt
# Remove conflict markers and combine both
Hello from Main Branch
Hello from Feature Branch

# Save and commit
git add app.txt
git commit -m "Resolve merge conflict: keep both versions"
```

### Option 4: Write New Content

```bash
# Edit app.txt
# Remove conflict markers and write new content
Hello from Merged Branches

# Save and commit
git add app.txt
git commit -m "Resolve merge conflict: create new version"
```

## Aborting a Merge

If you want to cancel the merge:

```bash
# Abort the merge
git merge --abort

# Check status
git status
# Back to normal, no longer in MERGING state
```

## Using Merge Tools

### Configure Merge Tool

```bash
# Set merge tool
git config --global merge.tool tortoisemerge

# Or use other tools
git config --global merge.tool vimdiff
git config --global merge.tool meld
```

### Use Merge Tool

```bash
# When conflict occurs
git mergetool

# Tool will open showing:
# - Local (your changes)
# - Remote (incoming changes)
# - Base (common ancestor)
# - Merged (result)

# After resolving in tool
git add <resolved_files>
git commit -m "Resolve conflicts using merge tool"
```

## Complete Conflict Resolution Example

```bash
# Setup: Create repository
git init
git config user.name "Developer"
git config user.email "dev@example.com"

# Main branch: Create file
echo "Line 1: Original" > code.js
echo "Line 2: Original" >> code.js
git add code.js
git commit -m "Initial commit"

# Feature branch: Modify line 2
git checkout -b feature-update
sed -i 's/Line 2: Original/Line 2: Feature Update/' code.js
git add code.js
git commit -m "Update line 2 in feature"

# Main branch: Modify line 2 differently
git checkout main
sed -i 's/Line 2: Original/Line 2: Main Update/' code.js
git add code.js
git commit -m "Update line 2 in main"

# Attempt merge
git merge feature-update
# CONFLICT!

# View conflict
cat code.js
# Line 1: Original
# <<<<<<< HEAD
# Line 2: Main Update
# =======
# Line 2: Feature Update
# >>>>>>> feature-update

# Resolve: Keep both
cat > code.js << EOF
Line 1: Original
Line 2: Main Update
Line 2: Feature Update (additional)
EOF

# Complete merge
git add code.js
git commit -m "Merge feature-update: combine both updates"

# Verify
git log --oneline --graph
```

## Real-World Scenario

### Scenario: Team Collaboration Conflict

```bash
# Developer 1: Working on main
git checkout main
echo "function login() {" > auth.js
echo "  // Main implementation" >> auth.js
echo "}" >> auth.js
git add auth.js
git commit -m "Add login function"
git push

# Developer 2: Working on feature (before pulling)
git checkout -b feature-auth
echo "function login() {" > auth.js
echo "  // Feature implementation" >> auth.js
echo "}" >> auth.js
git add auth.js
git commit -m "Add login function with new approach"

# Developer 2: Try to merge main
git checkout main
git pull
git checkout feature-auth
git merge main
# CONFLICT in auth.js!

# Developer 2: Resolve conflict
# Open auth.js
# See both implementations
# Decide to combine best of both

cat > auth.js << EOF
function login() {
  // Combined implementation
  // Using approach from main with enhancements from feature
  // Main logic here
  // Feature enhancements here
}
EOF

# Complete merge
git add auth.js
git commit -m "Merge main: combine login implementations"
git push origin feature-auth

# Create pull request for review
```

## Preventing Conflicts

### Best Practices

1. **Pull frequently**: `git pull --rebase` before starting work
2. **Commit often**: Small, focused commits
3. **Communicate**: Let team know what you're working on
4. **Use feature branches**: Isolate changes
5. **Merge main regularly**: Keep feature branches up to date

### Example: Staying Updated

```bash
# Before starting work
git checkout main
git pull

# Create feature branch
git checkout -b feature-new

# Work on feature
# ... make changes ...

# Before finishing, update from main
git checkout main
git pull
git checkout feature-new
git merge main
# Resolve any conflicts now

# Then push feature
git push origin feature-new
```

## Checking for Conflicts Before Merge

```bash
# See what would be merged
git diff main..feature-branch

# Check if merge would conflict
git merge --no-commit --no-ff feature-branch

# If conflicts, resolve them
# If no conflicts, abort
git merge --abort
```

## Complex Conflict Example

```bash
# Multiple files with conflicts
git merge feature-branch

# Output:
# CONFLICT (content): Merge conflict in app.js
# CONFLICT (content): Merge conflict in style.css
# CONFLICT (content): Merge conflict in index.html

# Check status
git status
# Shows all conflicted files

# Resolve each file
vim app.js      # Resolve conflicts
git add app.js

vim style.css   # Resolve conflicts
git add style.css

vim index.html  # Resolve conflicts
git add index.html

# Commit merge
git commit -m "Resolve conflicts in app.js, style.css, and index.html"
```

## Quick Reference

```bash
# When conflict occurs
git status                    # See conflicted files
git diff                      # See conflict details
cat <file>                    # View conflict markers

# Resolve conflict
# Edit file manually
git add <file>               # Mark as resolved
git commit                   # Complete merge

# Or abort
git merge --abort            # Cancel merge

# Using merge tool
git mergetool                # Open merge tool
git add <file>               # After resolving
git commit                   # Complete merge
```

## Next Steps

Learn about [Tags and Releases](./08-tags.md) to mark important points in your project history.
