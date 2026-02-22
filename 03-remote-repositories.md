# Working with Remote Repositories

## What is a Remote Repository?

A remote repository (like GitHub, GitLab, or Bitbucket) is a central location where your code is stored online, allowing collaboration with others.

## Cloning a Repository

When you clone a repository, the entire repo including the `.git` folder is copied to your local machine.

```bash
# Clone a repository
git clone <repository_url>

# Navigate to the cloned repository
cd <repository_name>

# Check the repository structure
ls -la
# You'll see the .git folder
```

## Pushing Changes to Remote

```bash
# 1. Make changes
touch newfile.txt
echo "Hello World" > newfile.txt

# 2. Add to staging
git add newfile.txt

# 3. Commit locally
git commit -m "Add newfile.txt"

# 4. Push to remote repository
git push
```

### First Time Push

The first time you push, Git will ask for credentials:
- Username/Password
- Personal Access Token (recommended)
- Sign in with browser

## Practical Example: Complete Workflow

```bash
# Clone repository
git clone https://github.com/username/my-project.git
cd my-project

# Create multiple files
touch file1.txt file2.txt file3.txt

# Add only specific file
git add file2.txt

# Check status
git status
# file2.txt is staged
# file1.txt and file3.txt are untracked

# Commit staged file
git commit -m "Add file2.txt"

# Push to remote
git push
# Only file2.txt will be pushed
```

## Important Notes

### Selective Staging vs Pushing

```bash
# You CAN stage specific files
git add file1.txt file2.txt

# But when you push, ALL committed files are pushed
git push
# All commits in local repo go to remote
```

**Key Point:** You cannot push only some commits from local repo to remote. All commits are pushed together.

## Handling Push Conflicts

### Scenario: Someone else pushed changes

```bash
# You try to push
git push
# Error: Updates were rejected

# Solution: Pull changes first
git pull --rebase

# Then push your changes
git push
```

### Example with Conflict

```bash
# Your teammate pushed changes
# You made local commits

# Try to push
git push
# Error: ! [rejected] main -> main (fetch first)

# Pull with rebase
git pull --rebase
# Your commits will be applied on top of remote changes

# If there are conflicts, resolve them
# Then continue
git rebase --continue

# Finally push
git push
```

## Collaboration and Permissions

### Public Repository Access

Even if a repository is public, you need collaboration access to push changes.

### Granting Access (Repository Owner)

1. Go to repository **Settings**
2. Click **Collaborators**
3. Click **Add people**
4. Enter username or email
5. Send invitation

### Example: Adding Collaborator

```
Repository: https://github.com/john/awesome-project
Settings → Collaborators → Add people
Enter: jane@example.com
Jane receives invitation email
Jane accepts invitation
Jane can now push to the repository
```

## Unstaging Files

```bash
# After git add, if you want to unstage
git restore --staged <filename>

# Example
git add feature.js
git status
# feature.js is staged

git restore --staged feature.js
git status
# feature.js is untracked
```

## Complete Collaboration Example

```bash
# Developer 1: Clone and make changes
git clone https://github.com/team/project.git
cd project
echo "Feature A" > featureA.txt
git add featureA.txt
git commit -m "Add Feature A"
git push

# Developer 2: Clone and make changes
git clone https://github.com/team/project.git
cd project
echo "Feature B" > featureB.txt
git add featureB.txt
git commit -m "Add Feature B"

# Developer 2: Try to push (will fail)
git push
# Error: Updates were rejected

# Developer 2: Pull and rebase
git pull --rebase
# Now local repo has both Feature A and Feature B

# Developer 2: Push successfully
git push
```

## Configuring Merge Tool

```bash
# Set merge tool (for resolving conflicts)
git config --global merge.tool tortoisemerge

# When conflicts occur, use merge tool
git mergetool

# After resolving conflicts
git add <resolved_files>
git commit -m "Resolve merge conflicts"
```

## Best Practices

1. **Always pull before push**: `git pull --rebase` before `git push`
2. **Commit frequently**: Small, focused commits are better
3. **Write clear commit messages**: Describe what and why
4. **Check status often**: Use `git status` to know what's happening
5. **Use branches**: Don't work directly on main (covered in next section)

## Common Commands Summary

```bash
# Clone repository
git clone <url>

# Check status
git status

# Add files
git add <file>

# Commit changes
git commit -m "message"

# Pull changes (with rebase)
git pull --rebase

# Push changes
git push

# View commit history
git log

# Unstage files
git restore --staged <file>
```

## Next Steps

Learn about [Git Reset Commands](./04-git-reset.md) to understand how to move files between different phases.
