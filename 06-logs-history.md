# Git Logs and History

## Basic Log Commands

```bash
# View commit history
git log

# View compact log
git log --oneline

# View last N commits
git log -n <number>

# Combine options
git log --oneline -5
```

## Viewing Commit Details

### Example: Basic Log

```bash
# Full log
git log

# Output:
# commit abc123def456... (HEAD -> main)
# Author: John Doe <john@example.com>
# Date:   Sun Feb 22 10:30:00 2026 +0000
#
#     Add login feature

# Compact log
git log --oneline

# Output:
# abc123d Add login feature
# def456e Update homepage
# ghi789f Initial commit
```

### Show Specific Commit

```bash
# Show details of a commit
git show <commit_id>

# Example
git show abc123d

# Output shows:
# - Commit info (author, date, message)
# - Files changed
# - Diff of changes
```

## Filtering Logs

### By Author

```bash
# View commits by specific author
git log --author="John Doe"

# With limit
git log --author="John Doe" -5

# Compact format
git log --author="John Doe" --oneline

# Example
git log --author="jane@example.com" --oneline -3
# abc123 Fix bug in auth
# def456 Add user model
# ghi789 Update README
```

### By Date

```bash
# Commits since a date
git log --since="2026-01-01"
git log --after="2026-01-01"

# Commits until a date
git log --until="2026-02-22"
git log --before="2026-02-22"

# Date range
git log --since="2026-01-01" --until="2026-02-22"
git log --after="2026-01-01" --before="2026-02-22"
```

### Date with Time

```bash
# Specific time range
git log --after="2026-02-22 09:00" --before="2026-02-22 17:00"

# Relative dates
git log --since="2 weeks ago"
git log --since="yesterday"
git log --since="3 days ago"
```

### Example: Date Filtering

```bash
# Commits from last week
git log --since="1 week ago" --oneline

# Commits from January 2026
git log --since="2026-01-01" --until="2026-01-31" --oneline

# Commits from today
git log --since="today" --oneline

# Commits by author in date range
git log --author="John" --since="2026-02-01" --oneline
```

### By File

```bash
# View commits that modified a specific file
git log <filename>

# Example
git log app.js

# With compact format
git log --oneline app.js

# Output:
# abc123 Update app logic
# def456 Fix app bug
# ghi789 Add app.js
```

### By Commit Message

```bash
# Search commit messages
git log --grep="<pattern>"

# Example: Find all bug fixes
git log --grep="fix"

# Example: Find feature additions
git log --grep="feature" --oneline

# Case-insensitive search
git log --grep="bug" -i
```

## Advanced Log Options

### Graph View

```bash
# Show branch graph
git log --graph --oneline

# Output:
# * abc123 Merge feature-login
# |\
# | * def456 Add login page
# | * ghi789 Add auth module
# |/
# * jkl012 Update homepage
```

### Show File Changes

```bash
# Show files changed in each commit
git log --name-only

# Show files with status
git log --name-status

# Show statistics
git log --stat
```

### Example: Detailed Log

```bash
# Show last 3 commits with files
git log --stat -3

# Output:
# commit abc123...
# Author: John Doe
# Date: Sun Feb 22 2026
#
#     Add login feature
#
#  login.html | 50 ++++++++++++++++++++
#  auth.js    | 30 +++++++++++++
#  2 files changed, 80 insertions(+)
```

## Practical Examples

### Example 1: Team Activity Review

```bash
# See what John worked on this week
git log --author="John" --since="1 week ago" --oneline

# See all commits from yesterday
git log --since="yesterday" --oneline

# See commits by multiple authors
git log --author="John\|Jane" --oneline
```

### Example 2: File History

```bash
# See all changes to config file
git log config.json

# See who last modified the file
git log -1 config.json

# See detailed changes
git log -p config.json
```

### Example 3: Bug Investigation

```bash
# Find commits mentioning "bug"
git log --grep="bug" --oneline

# Find commits in last month
git log --since="1 month ago" --grep="fix" --oneline

# See what changed in a specific commit
git show abc123d
```

## Git Aliases for Logs

```bash
# Create useful aliases
git config --global alias.lg "log --oneline --graph --all"
git config --global alias.last "log -1 HEAD"
git config --global alias.ls "log --oneline -10"

# Use aliases
git lg        # Pretty graph view
git last      # Last commit
git ls        # Last 10 commits
```

## Complete Example: Investigating Changes

```bash
# Scenario: Find when a bug was introduced

# Step 1: View recent commits
git log --oneline -20

# Step 2: Find commits related to the feature
git log --grep="payment" --oneline

# Step 3: Check specific file history
git log payment.js --oneline

# Step 4: See detailed changes in suspicious commit
git show def456e

# Step 5: Check who made the change
git log --author="John" payment.js

# Step 6: See changes in date range
git log --since="2026-02-15" --until="2026-02-20" payment.js
```

## Formatting Log Output

```bash
# Custom format
git log --pretty=format:"%h - %an, %ar : %s"

# Output:
# abc123d - John Doe, 2 days ago : Add login feature
# def456e - Jane Smith, 1 week ago : Update homepage

# Common format placeholders:
# %h  - Short commit hash
# %H  - Full commit hash
# %an - Author name
# %ae - Author email
# %ar - Author date (relative)
# %ad - Author date
# %s  - Commit message
```

## Searching Commit Content

```bash
# Search for code in commits
git log -S"function_name"

# Example: Find when a function was added/removed
git log -S"calculateTotal" --oneline

# Search with regex
git log -G"regex_pattern"
```

## Comparing Commits

```bash
# See changes between commits
git log commit1..commit2

# See changes in current branch not in main
git log main..feature-branch

# Example
git log abc123..def456 --oneline
```

## Best Practices

1. **Use --oneline for quick overview**: `git log --oneline`
2. **Filter by author for team review**: `git log --author="name"`
3. **Use date ranges for reports**: `git log --since="1 week ago"`
4. **Search commit messages**: `git log --grep="keyword"`
5. **Check file history**: `git log filename`
6. **Create aliases for common commands**: Save time with shortcuts

## Quick Reference

```bash
# Basic
git log                          # Full log
git log --oneline               # Compact log
git log -5                      # Last 5 commits
git show abc123                 # Show commit details

# Filtering
git log --author="John"         # By author
git log --since="2026-01-01"   # By date
git log --grep="fix"           # By message
git log app.js                 # By file

# Advanced
git log --graph --oneline      # Graph view
git log --stat                 # With statistics
git log -p                     # With diffs
git log -S"text"              # Search content
```

## Next Steps

Learn about [Handling Conflicts](./07-conflicts.md) when merging branches.
