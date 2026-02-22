# Git Cheat Sheet

Quick reference for all Git commands.

## Setup and Configuration

```bash
# Install Git
# Download from git-scm.com

# Check version
git --version

# Configure user
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# View configuration
git config --list

# Remove configuration
git config --global --unset user.name
git config --global --unset user.email
```

## Repository Basics

```bash
# Initialize repository
git init

# Clone repository
git clone <url>

# Check status
git status

# View history
git log
git log --oneline
git log --oneline -5
git log --graph --oneline --all
```

## Three Phases

```bash
# Workspace → Staging
git add <file>
git add .
git add *
git add -A

# Staging → Local Repo
git commit -m "message"

# Unstage files
git restore --staged <file>
git rm --cached <file>
git reset HEAD <file>
```

## Reset Commands

```bash
# Staging → Workspace
git reset HEAD <file>

# Local Repo → Staging
git reset --soft <commit_id>

# Local Repo → Workspace
git reset --mixed <commit_id>
git reset <commit_id>

# Delete commit and files
git reset --hard <commit_id>
```

## Branching

```bash
# List branches
git branch                  # Local
git branch -r              # Remote
git branch -a              # All

# Create branch
git branch <branch_name>

# Switch branch
git checkout <branch_name>

# Create and switch
git checkout -b <branch_name>

# Delete branch
git branch -d <branch_name>    # Safe delete
git branch -D <branch_name>    # Force delete

# Merge branch
git checkout main
git merge <branch_name>

# Abort merge
git merge --abort
```

## Remote Operations

```bash
# Add remote
git remote add origin <url>

# View remotes
git remote -v

# Push to remote
git push
git push origin <branch_name>
git push -u origin <branch_name>
git push --tags

# Pull from remote
git pull
git pull --rebase

# Fetch from remote
git fetch
git fetch --prune
```

## Logs and History

```bash
# View logs
git log
git log --oneline
git log -n 5
git log --graph

# Filter by author
git log --author="name"

# Filter by date
git log --since="2026-01-01"
git log --until="2026-02-22"
git log --since="1 week ago"

# Filter by file
git log <filename>

# Filter by message
git log --grep="keyword"

# Show commit details
git show <commit_id>
```

## Tags

```bash
# Create tag
git tag <tag_name>
git tag <tag_name> <commit_id>
git tag -a <tag_name> -m "message"

# List tags
git tag

# Show tag
git show <tag_name>

# Checkout tag
git checkout <tag_name>

# Push tags
git push origin <tag_name>
git push --tags

# Delete tag
git tag -d <tag_name>              # Local
git push origin -d <tag_name>      # Remote
```

## Stash

```bash
# Save to stash
git stash
git stash save "message"
git stash save -u "message"        # Include untracked

# List stashes
git stash list

# View stash
git stash show stash@{0}
git stash show -p stash@{0}

# Apply stash
git stash pop                      # Apply and remove
git stash pop stash@{1}
git stash apply                    # Apply and keep
git stash apply stash@{1}

# Delete stash
git stash drop stash@{0}
git stash clear                    # Delete all
```

## Advanced Commands

```bash
# Rebase
git rebase <branch_name>
git rebase --continue
git rebase --abort
git pull --rebase

# Cherry-pick
git cherry-pick <commit_id>

# Amend last commit
git commit --amend -m "new message"
git commit --amend --no-edit

# Revert commit
git revert <commit_id>

# Diff
git diff
git diff --staged
git diff <file1> <file2>
git diff <branch1> <branch2>

# Blame
git blame <filename>
git blame -L 1,10 <filename>

# Bisect
git bisect start
git bisect bad
git bisect good <commit_id>
git bisect reset
```

## Aliases

```bash
# Create aliases
git config --global alias.st "status"
git config --global alias.co "checkout"
git config --global alias.br "branch"
git config --global alias.ci "commit"
git config --global alias.lg "log --oneline --graph --all"

# Use aliases
git st
git co main
git br
git ci -m "message"
git lg

# Remove alias
git config --global --unset alias.st
```

## Conflict Resolution

```bash
# When conflict occurs
git status                         # See conflicted files
git diff                          # See conflicts

# After resolving manually
git add <resolved_file>
git commit

# Or abort
git merge --abort

# Use merge tool
git config --global merge.tool <tool_name>
git mergetool
```

## Gitignore

```bash
# Create .gitignore
touch .gitignore

# Common patterns
*.log                             # Ignore all .log files
node_modules/                     # Ignore directory
.env                              # Ignore specific file
build/                            # Ignore build directory
*.tmp                             # Ignore all .tmp files
!important.log                    # Don't ignore this file
```

## Useful Combinations

```bash
# Quick commit all changes
git add . && git commit -m "message" && git push

# Update branch from main
git checkout main && git pull && git checkout feature && git merge main

# Clean up merged branches
git branch --merged | grep -v "\*" | xargs -n 1 git branch -d

# Undo last commit (keep changes)
git reset --soft HEAD~1

# Undo last commit (discard changes)
git reset --hard HEAD~1

# View file at specific commit
git show <commit_id>:<filename>

# List files in commit
git diff-tree --no-commit-id --name-only -r <commit_id>
```

## Common Workflows

### Feature Branch Workflow

```bash
git checkout main
git pull
git checkout -b feature-name
# ... make changes ...
git add .
git commit -m "Add feature"
git push -u origin feature-name
# ... create pull request ...
# ... after merge ...
git checkout main
git pull
git branch -d feature-name
```

### Hotfix Workflow

```bash
git checkout main
git pull
git checkout -b hotfix-issue
# ... fix issue ...
git add .
git commit -m "Fix issue"
git push -u origin hotfix-issue
# ... create pull request ...
# ... after merge ...
git checkout main
git pull
git tag v1.0.1
git push --tags
```

### Sync Fork Workflow

```bash
git remote add upstream <original_repo_url>
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

## Troubleshooting

```bash
# Undo git add
git reset HEAD <file>

# Undo git commit (keep changes)
git reset --soft HEAD~1

# Undo git commit (discard changes)
git reset --hard HEAD~1

# Recover deleted branch
git reflog
git checkout -b <branch_name> <commit_id>

# Fix wrong commit message
git commit --amend -m "correct message"

# Remove file from Git but keep locally
git rm --cached <file>

# Clean untracked files
git clean -n                      # Preview
git clean -f                      # Remove files
git clean -fd                     # Remove files and directories
```

## Best Practices

1. **Commit often**: Small, focused commits
2. **Write clear messages**: Describe what and why
3. **Pull before push**: Avoid conflicts
4. **Use branches**: Don't work on main
5. **Review before commit**: Check `git status` and `git diff`
6. **Keep history clean**: Use rebase for local branches
7. **Tag releases**: Mark important versions
8. **Use .gitignore**: Don't commit unnecessary files

## Quick Tips

- Use `git status` frequently
- Use `git log --oneline` for quick history
- Use `git diff` before committing
- Use `git stash` when switching tasks
- Use `git pull --rebase` to keep history linear
- Use descriptive branch names
- Delete merged branches regularly
- Use tags for releases

## Resources

- Official Git Documentation: https://git-scm.com/doc
- Git Book: https://git-scm.com/book
- GitHub Guides: https://guides.github.com
- GitLab Docs: https://docs.gitlab.com
