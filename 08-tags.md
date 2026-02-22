# Tags and Releases

## What are Tags?

Tags are markers for specific points in Git history, typically used to mark release points (v1.0, v2.0, etc.).

```
Commits:  A---B---C---D---E---F
Tags:         v1.0    v1.1    v2.0
```

## Why Use Tags?

- Mark release points
- Easy reference to specific versions
- Track production deployments
- Create stable snapshots

## Creating Tags

### Tag Latest Commit

```bash
# Create a tag on latest commit
git tag <tag_name>

# Example
git tag v1.0

# View tags
git tag

# Output:
# v1.0
```

### Tag Specific Commit

```bash
# Create tag on specific commit
git tag <tag_name> <commit_id>

# Example
git log --oneline
# abc123 Add feature
# def456 Fix bug
# ghi789 Initial commit

git tag v0.9 def456
git tag v1.0 abc123
```

## Viewing Tags

```bash
# List all tags
git tag

# Show tag details
git show <tag_name>

# Example
git show v1.0
# Shows commit info, author, date, changes
```

### Example: Creating Multiple Tags

```bash
# View commits
git log --oneline
# aaa111 Add dashboard
# bbb222 Add login
# ccc333 Add homepage
# ddd444 Initial commit

# Create tags for different versions
git tag v0.1 ddd444    # Initial release
git tag v0.5 ccc333    # Homepage release
git tag v0.8 bbb222    # Login release
git tag v1.0 aaa111    # Full release
git tag v1.1           # Latest (if not specified)

# View all tags
git tag
# v0.1
# v0.5
# v0.8
# v1.0
# v1.1

# View tags with commits
git log --oneline
# aaa111 (tag: v1.0, tag: v1.1) Add dashboard
# bbb222 (tag: v0.8) Add login
# ccc333 (tag: v0.5) Add homepage
# ddd444 (tag: v0.1) Initial commit
```

## Checking Out Tags

```bash
# Switch to a specific tag
git checkout <tag_name>

# Example
git checkout v1.0

# You're now in "detached HEAD" state
# You can view files at this version
ls

# Return to branch
git checkout main
```

## Pushing Tags to Remote

### Push Single Tag

```bash
# Push specific tag
git push origin <tag_name>

# Example
git push origin v1.0
```

### Push All Tags

```bash
# Push all tags at once
git push --tags

# Or
git push origin --tags
```

### Example: Complete Tag Workflow

```bash
# Create and push tags
git tag v1.0
git tag v1.1
git tag v1.2

# Push all tags
git push --tags

# Verify on remote (GitHub/GitLab)
# Go to repository → Tags section
# You'll see: v1.0, v1.1, v1.2
```

## Deleting Tags

### Delete Local Tag

```bash
# Delete tag locally
git tag -d <tag_name>

# Example
git tag -d v1.0

# Verify
git tag
# v1.0 is gone
```

### Delete Remote Tag

```bash
# Delete tag on remote
git push origin -d <tag_name>

# Or
git push origin :refs/tags/<tag_name>

# Example
git push origin -d v1.0
```

### Example: Cleanup Tags

```bash
# List tags
git tag
# v0.1
# v0.2
# v1.0-beta
# v1.0

# Delete beta tag locally
git tag -d v1.0-beta

# Delete from remote
git push origin -d v1.0-beta

# Verify
git tag
# v0.1
# v0.2
# v1.0
```

## Annotated Tags

Annotated tags store extra information (tagger name, email, date, message).

```bash
# Create annotated tag
git tag -a <tag_name> -m "message"

# Example
git tag -a v2.0 -m "Version 2.0 - Major release"

# Show annotated tag
git show v2.0
# Shows tag info AND commit info
```

## Practical Examples

### Example 1: Release Workflow

```bash
# Development complete
git log --oneline
# abc123 Final bug fixes
# def456 Add new features
# ghi789 Update documentation

# Create release tag
git tag -a v1.0.0 -m "Release version 1.0.0"

# Push tag
git push origin v1.0.0

# On GitHub: Create release from tag
# 1. Go to Releases
# 2. Click "Create a new release"
# 3. Select tag: v1.0.0
# 4. Add release notes
# 5. Publish release
```

### Example 2: Hotfix Release

```bash
# Production issue found
git checkout v1.0.0

# Create hotfix branch
git checkout -b hotfix-1.0.1

# Fix the issue
echo "fix" > bugfix.txt
git add bugfix.txt
git commit -m "Fix critical bug"

# Merge to main
git checkout main
git merge hotfix-1.0.1

# Create hotfix tag
git tag -a v1.0.1 -m "Hotfix: Critical bug fix"

# Push
git push origin main
git push origin v1.0.1
```

### Example 3: Viewing Release History

```bash
# List all releases
git tag

# See what's in each release
git show v1.0.0
git show v1.1.0
git show v2.0.0

# Compare releases
git diff v1.0.0 v2.0.0

# See commits between releases
git log v1.0.0..v2.0.0 --oneline
```

## Semantic Versioning

Use semantic versioning for tags: `vMAJOR.MINOR.PATCH`

```bash
# v1.0.0 - Initial release
git tag v1.0.0

# v1.0.1 - Patch (bug fix)
git tag v1.0.1

# v1.1.0 - Minor (new feature, backward compatible)
git tag v1.1.0

# v2.0.0 - Major (breaking changes)
git tag v2.0.0
```

## Complete Real-World Example

```bash
# Project development
git init
git add .
git commit -m "Initial commit"

# Alpha release
git tag v0.1.0-alpha
git push origin v0.1.0-alpha

# Beta release
# ... more commits ...
git tag v0.9.0-beta
git push origin v0.9.0-beta

# Release candidate
# ... more commits ...
git tag v1.0.0-rc1
git push origin v1.0.0-rc1

# Final release
# ... final fixes ...
git tag -a v1.0.0 -m "Version 1.0.0 - First stable release"
git push origin v1.0.0
git push --tags

# View all tags
git tag
# v0.1.0-alpha
# v0.9.0-beta
# v1.0.0-rc1
# v1.0.0

# Check out specific version
git checkout v1.0.0
# View files at this version

# Return to development
git checkout main
```

## Tags vs Branches

| Feature | Tags | Branches |
|---------|------|----------|
| Purpose | Mark specific points | Ongoing development |
| Mutable | No (immutable) | Yes (can add commits) |
| Use case | Releases, versions | Features, fixes |
| Can merge | No | Yes |

## Best Practices

1. **Use semantic versioning**: v1.0.0, v1.1.0, v2.0.0
2. **Tag releases only**: Don't tag every commit
3. **Use annotated tags**: Include release notes
4. **Push tags explicitly**: `git push --tags`
5. **Don't modify tags**: Create new tag instead
6. **Document releases**: Add release notes on GitHub/GitLab

## Quick Reference

```bash
# Create tag
git tag v1.0                    # Simple tag
git tag -a v1.0 -m "msg"       # Annotated tag
git tag v1.0 abc123            # Tag specific commit

# View tags
git tag                         # List all tags
git show v1.0                  # Show tag details
git log --oneline              # See tags in log

# Checkout tag
git checkout v1.0              # View files at tag

# Push tags
git push origin v1.0           # Push single tag
git push --tags                # Push all tags

# Delete tags
git tag -d v1.0                # Delete local
git push origin -d v1.0        # Delete remote
```

## Next Steps

Learn about [Git Stash](./09-stash.md) to temporarily save your work.
