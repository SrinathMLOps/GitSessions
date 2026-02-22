# Git Examples and Exercises

Practical examples and hands-on exercises to master Git.

## Exercise 1: Basic Git Workflow

### Objective
Learn the basic Git workflow: init, add, commit, log.

### Steps

```bash
# 1. Create a new project
mkdir my-first-repo
cd my-first-repo

# 2. Initialize Git
git init

# 3. Configure user
git config user.name "Your Name"
git config user.email "your@email.com"

# 4. Create files
echo "# My First Repo" > README.md
echo "console.log('Hello Git');" > app.js

# 5. Check status
git status
# Expected: 2 untracked files

# 6. Add files
git add README.md app.js

# 7. Check status again
git status
# Expected: 2 files staged

# 8. Commit
git commit -m "Initial commit: Add README and app.js"

# 9. View history
git log
git log --oneline

# 10. Make changes
echo "More content" >> README.md

# 11. See changes
git diff

# 12. Add and commit
git add README.md
git commit -m "Update README"

# 13. View history
git log --oneline
```

### Expected Result
- 2 commits in history
- Clean working directory

---

## Exercise 2: Branching and Merging

### Objective
Create branches, make changes, and merge them.

### Steps

```bash
# 1. Create repository
mkdir branch-practice
cd branch-practice
git init
git config user.name "Developer"
git config user.email "dev@example.com"

# 2. Create initial commit
echo "Main content" > main.txt
git add main.txt
git commit -m "Add main.txt"

# 3. Create feature branch
git checkout -b feature-login

# 4. Add feature
echo "Login page" > login.html
git add login.html
git commit -m "Add login page"

# 5. Switch to main
git checkout main

# 6. Verify login.html doesn't exist
ls
# Should not show login.html

# 7. Merge feature
git merge feature-login

# 8. Verify merge
ls
# Should now show login.html

# 9. View history
git log --oneline --graph

# 10. Delete feature branch
git branch -d feature-login
```

### Expected Result
- main branch has both main.txt and login.html
- Feature branch deleted
- Clean merge in history

---

## Exercise 3: Handling Merge Conflicts

### Objective
Create and resolve a merge conflict.

### Steps

```bash
# 1. Setup
mkdir conflict-practice
cd conflict-practice
git init
git config user.name "Developer"
git config user.email "dev@example.com"

# 2. Create file on main
echo "Line 1" > file.txt
git add file.txt
git commit -m "Add file.txt"

# 3. Create branch and modify
git checkout -b feature-a
echo "Line 2 from Feature A" >> file.txt
git add file.txt
git commit -m "Update from Feature A"

# 4. Switch to main and modify same file
git checkout main
echo "Line 2 from Main" >> file.txt
git add file.txt
git commit -m "Update from Main"

# 5. Try to merge (will conflict)
git merge feature-a
# CONFLICT!

# 6. View conflict
cat file.txt
# Shows conflict markers

# 7. Resolve conflict
# Edit file.txt to:
# Line 1
# Line 2 from Main
# Line 2 from Feature A

# 8. Complete merge
git add file.txt
git commit -m "Resolve conflict: keep both changes"

# 9. View result
cat file.txt
git log --oneline --graph
```

### Expected Result
- Conflict resolved
- Both changes preserved
- Merge commit in history

---

## Exercise 4: Using Git Stash

### Objective
Save work temporarily and restore it later.

### Steps

```bash
# 1. Setup
mkdir stash-practice
cd stash-practice
git init
git config user.name "Developer"
git config user.email "dev@example.com"

# 2. Create initial commit
echo "Initial" > app.js
git add app.js
git commit -m "Initial commit"

# 3. Start working on feature
echo "Feature code" >> app.js
git add app.js

# 4. Urgent bug fix needed!
# Stash current work
git stash save "Feature in progress"

# 5. Verify clean workspace
git status
cat app.js
# Should only show "Initial"

# 6. Fix bug
echo "Bug fix" > bugfix.js
git add bugfix.js
git commit -m "Fix urgent bug"

# 7. Return to feature work
git stash pop

# 8. Verify feature code is back
cat app.js
# Should show both "Initial" and "Feature code"

# 9. Complete feature
git commit -m "Complete feature"
```

### Expected Result
- Bug fix committed
- Feature work restored and committed
- Stash list empty

---

## Exercise 5: Tagging Releases

### Objective
Create and manage tags for releases.

### Steps

```bash
# 1. Setup
mkdir tag-practice
cd tag-practice
git init
git config user.name "Developer"
git config user.email "dev@example.com"

# 2. Create v0.1
echo "Version 0.1" > app.js
git add app.js
git commit -m "Version 0.1"
git tag v0.1

# 3. Create v0.2
echo "Version 0.2" >> app.js
git add app.js
git commit -m "Version 0.2"
git tag v0.2

# 4. Create v1.0
echo "Version 1.0" >> app.js
git add app.js
git commit -m "Version 1.0"
git tag -a v1.0 -m "First stable release"

# 5. View tags
git tag

# 6. View tag details
git show v1.0

# 7. Checkout old version
git checkout v0.1
cat app.js
# Should only show "Version 0.1"

# 8. Return to latest
git checkout main
cat app.js
# Should show all versions

# 9. View history with tags
git log --oneline
```

### Expected Result
- 3 tags created
- Can checkout and view old versions
- Tags visible in log

---

## Exercise 6: Rebase Practice

### Objective
Use rebase to keep history linear.

### Steps

```bash
# 1. Setup
mkdir rebase-practice
cd rebase-practice
git init
git config user.name "Developer"
git config user.email "dev@example.com"

# 2. Create main commits
echo "Commit 1" > file.txt
git add file.txt
git commit -m "Commit 1"

echo "Commit 2" >> file.txt
git add file.txt
git commit -m "Commit 2"

# 3. Create feature branch
git checkout -b feature

echo "Feature A" > feature.txt
git add feature.txt
git commit -m "Feature A"

# 4. Update main
git checkout main
echo "Commit 3" >> file.txt
git add file.txt
git commit -m "Commit 3"

# 5. Rebase feature on main
git checkout feature
git rebase main

# 6. View history
git log --oneline --graph --all

# 7. Merge to main
git checkout main
git merge feature
```

### Expected Result
- Linear history (no merge commit)
- Feature commits after main commits

---

## Exercise 7: Cherry-Pick

### Objective
Apply specific commits to another branch.

### Steps

```bash
# 1. Setup
mkdir cherry-pick-practice
cd cherry-pick-practice
git init
git config user.name "Developer"
git config user.email "dev@example.com"

# 2. Create commits on main
echo "Feature 1" > f1.txt
git add f1.txt
git commit -m "Feature 1"

echo "Feature 2" > f2.txt
git add f2.txt
git commit -m "Feature 2"

echo "Feature 3" > f3.txt
git add f3.txt
git commit -m "Feature 3"

# 3. Note commit IDs
git log --oneline
# abc123 Feature 3
# def456 Feature 2
# ghi789 Feature 1

# 4. Create release branch
git checkout -b release ghi789

# 5. Cherry-pick only Feature 2
git cherry-pick def456

# 6. Verify
ls
# Should have f1.txt and f2.txt, but not f3.txt

git log --oneline
```

### Expected Result
- Release branch has Feature 1 and Feature 2
- Feature 3 not included

---

## Exercise 8: Complete Project Workflow

### Objective
Simulate a real project workflow.

### Steps

```bash
# 1. Initialize project
mkdir real-project
cd real-project
git init
git config user.name "Developer"
git config user.email "dev@example.com"

# 2. Create initial structure
echo "# My Project" > README.md
echo "node_modules/" > .gitignore
echo "*.log" >> .gitignore
mkdir src
echo "console.log('App');" > src/app.js

git add .
git commit -m "Initial project structure"
git tag v0.1.0

# 3. Feature: Add authentication
git checkout -b feature-auth

echo "function login() {}" > src/auth.js
git add src/auth.js
git commit -m "Add login function"

echo "function logout() {}" >> src/auth.js
git add src/auth.js
git commit -m "Add logout function"

# 4. Meanwhile, update main
git checkout main
echo "## Installation" >> README.md
git add README.md
git commit -m "Update README"

# 5. Rebase feature on main
git checkout feature-auth
git rebase main

# 6. Merge feature
git checkout main
git merge feature-auth

# 7. Tag release
git tag v0.2.0

# 8. Hotfix needed!
git checkout -b hotfix-auth v0.2.0
echo "// Fix bug" >> src/auth.js
git add src/auth.js
git commit -m "Fix auth bug"

# 9. Merge hotfix
git checkout main
git merge hotfix-auth
git tag v0.2.1

# 10. View final history
git log --oneline --graph --all

# 11. Cleanup
git branch -d feature-auth
git branch -d hotfix-auth
```

### Expected Result
- Clean project structure
- Multiple features merged
- Proper versioning with tags
- Linear history

---

## Challenge Exercises

### Challenge 1: Recover from Mistakes

```bash
# Scenario: You committed to wrong branch
# 1. Create commit on main (should be on feature)
# 2. Use cherry-pick to move it to feature branch
# 3. Reset main to previous state
```

### Challenge 2: Interactive Rebase

```bash
# Scenario: Clean up messy commit history
# 1. Make 5 small commits
# 2. Use interactive rebase to squash them into 1
# Hint: git rebase -i HEAD~5
```

### Challenge 3: Bisect Bug Hunt

```bash
# Scenario: Find which commit broke the code
# 1. Create 10 commits
# 2. Introduce a "bug" in commit 6
# 3. Use git bisect to find it
```

---

## Practice Tips

1. **Create a practice repository**: Don't practice on real projects
2. **Use git log frequently**: Understand what's happening
3. **Experiment with branches**: They're cheap and safe
4. **Make mistakes**: Learn by fixing them
5. **Use git status**: Always know where you are
6. **Read error messages**: Git provides helpful hints
7. **Practice daily**: Consistency builds muscle memory

## Common Mistakes to Avoid

1. **Committing to wrong branch**: Always check `git branch` first
2. **Forgetting to pull**: Pull before starting work
3. **Large commits**: Make small, focused commits
4. **Poor commit messages**: Be descriptive
5. **Not using branches**: Always use feature branches
6. **Ignoring conflicts**: Resolve them properly
7. **Force pushing**: Avoid `git push -f` on shared branches

## Next Steps

After completing these exercises:
1. Practice on a personal project
2. Contribute to open source
3. Learn Git workflows (GitFlow, GitHub Flow)
4. Explore advanced features (hooks, submodules)
5. Master your Git GUI tool

Happy coding!
