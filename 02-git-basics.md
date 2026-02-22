# Git Basics - Understanding the Three Phases

## The Three Phases of Git

Git manages your files through three distinct areas:

1. **Workspace** (Working Directory) - Where you create and edit files
2. **Staging/Index** - Where you prepare files for commit
3. **Local Repository** - Where committed files are stored permanently

```
Workspace → Staging → Local Repository
   ↓          ↓            ↓
 (edit)    (git add)  (git commit)
```

## Initializing a Repository

```bash
# Create a new Git repository
git init

# Check repository status
git status
```

## Working with Files

### Creating and Adding Files

```bash
# Create a new file
touch file1.txt

# Check status
git status
# Output: file1.txt is untracked (in workspace)

# Add file to staging area
git add file1.txt

# Check status again
git status
# Output: file1.txt is staged (ready to commit)
```

### Committing Files

```bash
# Commit staged files
git commit -m "Add file1.txt"

# Check status
git status
# Output: nothing to commit, working tree clean

# View commit history
git log
```

## Practical Example

```bash
# Step 1: Initialize repository
mkdir my-project
cd my-project
git init

# Step 2: Create files
touch app.js
touch style.css
touch index.html

# Step 3: Check status
git status
# All files are untracked

# Step 4: Add files to staging
git add app.js style.css index.html
# Or use: git add .
# Or use: git add *
# Or use: git add -A

# Step 5: Commit files
git commit -m "Initial commit: Add project files"

# Step 6: View history
git log
```

## Adding Multiple Files

```bash
# Method 1: Add specific files
git add file1.txt file2.txt file3.txt

# Method 2: Add all files
git add .

# Method 3: Add all files (alternative)
git add *

# Method 4: Add all changes
git add -A
```

## Viewing Commit Information

```bash
# View detailed commit history
git log

# View specific commit details
git show <commit_id>

# View compact log
git log --oneline

# Exit log view
# Press 'q'
```

## Unstaging Files

If you added files to staging but want to move them back to workspace:

```bash
# Method 1: Restore from staging
git restore --staged <file>

# Method 2: Remove from cache
git rm --cached <file>
```

### Example: Unstaging

```bash
# Create and add files
touch feature.js
git add feature.js
git status
# Output: feature.js is staged

# Unstage the file
git restore --staged feature.js
git status
# Output: feature.js is untracked
```

## Removing Files

```bash
# Remove file from workspace
rm filename.txt

# Check status
git status
# Git will show the file as deleted
```

## Complete Workflow Example

```bash
# 1. Initialize repository
git init

# 2. Create files
echo "console.log('Hello');" > app.js
echo "body { margin: 0; }" > style.css

# 3. Check status
git status
# Both files untracked

# 4. Add to staging
git add .

# 5. Check status
git status
# Both files staged

# 6. Commit
git commit -m "Add app.js and style.css"

# 7. View history
git log --oneline

# 8. Create another file
echo "<h1>Title</h1>" > index.html

# 9. Add and commit in one go
git add index.html
git commit -m "Add index.html"

# 10. View all commits
git log
```

## Key Takeaways

- **Workspace**: Where you edit files
- **Staging**: Where you prepare files for commit (use `git add`)
- **Local Repository**: Where commits are stored (use `git commit`)
- Use `git status` frequently to check which phase your files are in
- Use `git log` to view commit history
- Use `git show <commit_id>` to see details of a specific commit

## Next Steps

Learn about [Working with Remote Repositories](./03-remote-repositories.md) to collaborate with others.
