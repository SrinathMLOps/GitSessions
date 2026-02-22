# Branching and Merging

## What is a Branch?

A branch is an independent line of development. It allows you to work on features without affecting the main codebase.

```
main:     A---B---C---F
               \     /
feature:        D---E
```

## Basic Branch Commands

```bash
# List all local branches
git branch

# List remote branches
git branch -r

# List all branches (local + remote)
git branch -a

# Create a new branch
git branch <branch_name>

# Switch to a branch
git checkout <branch_name>

# Create and switch to a new branch
git checkout -b <branch_name>
```

## Working with Branches

### Example 1: Creating and Switching Branches

```bash
# Check current branch
git branch
# * main

# Create a new branch
git branch feature-login

# List branches
git branch
#   feature-login
# * main

# Switch to the new branch
git checkout feature-login

# Check current branch
git branch
# * feature-login
#   main
```

### Example 2: Working on a Branch

```bash
# Switch to main
git checkout main

# Create a file
touch homepage.html
git add homepage.html
git commit -m "Add homepage"

# Switch to feature branch
git checkout feature-login

# List files
ls
# homepage.html is NOT here

# Create feature-specific files
touch login.html
git add login.html
git commit -m "Add login page"

# List files
ls
# Only login.html is here
```

## Merging Branches

### Basic Merge

```bash
# Switch to the branch you want to merge INTO
git checkout main

# Merge another branch into current branch
git merge <branch_name>
```

### Example: Merging Feature to Main

```bash
# On feature-login branch
git checkout feature-login
touch auth.js
git add auth.js
git commit -m "Add authentication"

# Switch to main
git checkout main

# Merge feature-login into main
git merge feature-login
# Output: 1 file changed, X insertions(+)

# List files
ls
# Now you have auth.js in main
```

## Deleting Branches

```bash
# Delete a branch (safe delete)
git branch -d <branch_name>

# Force delete a branch
git branch -D <branch_name>
```

### Important Rules

1. You cannot delete the branch you're currently on
2. Use `-d` for merged branches
3. Use `-D` for unmerged branches (forces deletion)

### Example: Deleting Branches

```bash
# Try to delete current branch
git checkout feature-login
git branch -d feature-login
# Error: Cannot delete branch you're on

# Switch to main first
git checkout main
git branch -d feature-login
# Success!

# If branch has unmerged changes
git branch feature-test
git checkout feature-test
touch test.js
git add test.js
git commit -m "Add test"

git checkout main
git branch -d feature-test
# Error: Branch not fully merged

# Force delete
git branch -D feature-test
# Success!
```

## Pushing Branches to Remote

### Push a Branch

```bash
# Push branch to remote
git push origin <branch_name>

# Or specify full path
git push <remote_url> <branch_name>
```

### Example: Complete Branch Workflow

```bash
# Create and switch to new branch
git checkout -b feature-payment

# Make changes
touch payment.js
git add payment.js
git commit -m "Add payment module"

# Push to remote
git push origin feature-payment

# Check remote branches
git branch -r
# origin/main
# origin/feature-payment
```

## Setting Upstream Branch

When you push a branch for the first time, you may need to set upstream:

```bash
# First push will fail
git push
# Error: no upstream branch

# Set upstream and push
git push --set-upstream origin feature-payment

# Or shorter version
git push -u origin feature-payment

# Now git push will work directly
git push
```

### Alternative: Configure in .git/config

```bash
# Edit .git/config
[branch "feature-payment"]
    remote = origin
    merge = refs/heads/feature-payment

# Now git push works without specifying branch
git push
```

## Pull Requests (Merging on Remote)

Instead of merging locally, you can merge on GitHub/GitLab:

### Steps for Pull Request

1. Push your branch to remote
```bash
git push origin feature-payment
```

2. Go to GitHub/GitLab
3. Click "Pull Request" or "Merge Request"
4. Select base branch (e.g., main) and compare branch (e.g., feature-payment)
5. Add title and description
6. Add reviewers (optional)
7. Click "Create Pull Request"
8. After review, click "Merge Pull Request"
9. Confirm merge

### Example: Complete PR Workflow

```bash
# Local: Create feature branch
git checkout -b feature-dashboard
touch dashboard.js
git add dashboard.js
git commit -m "Add dashboard"

# Push to remote
git push origin feature-dashboard

# On GitHub:
# 1. Click "Compare & pull request"
# 2. Base: main ← Compare: feature-dashboard
# 3. Title: "Add dashboard feature"
# 4. Description: "This PR adds the dashboard module"
# 5. Reviewers: Add team members
# 6. Click "Create pull request"
# 7. Wait for review
# 8. Click "Merge pull request"
# 9. Click "Confirm merge"

# Local: Update main branch
git checkout main
git pull
# Now main has dashboard.js
```

## Deleting Remote Branches

```bash
# Delete branch on remote
git push origin -d <branch_name>

# Example
git push origin -d feature-payment
```

## Git Aliases for Branches

```bash
# Create aliases
git config --global alias.br "branch"
git config --global alias.co "checkout"
git config --global alias.cob "checkout -b"

# Use aliases
git br                    # List branches
git co main              # Switch to main
git cob feature-new      # Create and switch to feature-new
```

## Complete Real-World Example

```bash
# Start: Clone repository
git clone https://github.com/team/project.git
cd project

# Create feature branch
git checkout -b feature-user-profile

# Work on feature
touch profile.js profile.css
git add .
git commit -m "Add user profile page"

# Push to remote
git push -u origin feature-user-profile

# Continue working
echo "more code" >> profile.js
git add profile.js
git commit -m "Update profile styling"
git push

# Create pull request on GitHub
# Wait for review and approval

# After merge, update local main
git checkout main
git pull

# Delete local feature branch
git branch -d feature-user-profile

# Delete remote feature branch
git push origin -d feature-user-profile
```

## Best Practices

1. **Use descriptive branch names**: `feature-login`, `bugfix-header`, `hotfix-security`
2. **Keep branches short-lived**: Merge frequently
3. **Always pull before creating a branch**: `git pull` then `git checkout -b`
4. **Delete merged branches**: Keep repository clean
5. **Use pull requests**: Enable code review
6. **Don't work directly on main**: Always use feature branches

## Common Branch Patterns

```bash
# Feature branch
git checkout -b feature/user-authentication

# Bug fix branch
git checkout -b bugfix/login-error

# Hotfix branch
git checkout -b hotfix/security-patch

# Release branch
git checkout -b release/v1.0.0
```

## Next Steps

Learn about [Git Logs and History](./06-logs-history.md) to track and analyze your commits.
