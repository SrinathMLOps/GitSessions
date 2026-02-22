# Git Visual Guide

Visual representations to help understand Git concepts.

## The Three Phases

```
┌─────────────────────────────────────────────────────────────┐
│                     GIT WORKFLOW                            │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌──────────────┐      ┌──────────────┐      ┌──────────┐ │
│  │              │      │              │      │          │ │
│  │  WORKSPACE   │      │   STAGING    │      │  LOCAL   │ │
│  │  (Working    │      │   (Index)    │      │   REPO   │ │
│  │  Directory)  │      │              │      │          │ │
│  │              │      │              │      │          │ │
│  └──────────────┘      └──────────────┘      └──────────┘ │
│         │                     │                    │       │
│         │   git add           │   git commit       │       │
│         │─────────────────────>│───────────────────>│       │
│         │                     │                    │       │
│         │<────────────────────│<───────────────────│       │
│         │   git restore       │   git reset        │       │
│         │   --staged          │                    │       │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## Git Reset Types

```
┌─────────────────────────────────────────────────────────────┐
│                    GIT RESET MODES                          │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  WORKSPACE         STAGING          LOCAL REPO              │
│                                                             │
│  ┌─────────┐      ┌─────────┐      ┌─────────┐            │
│  │ file.txt│      │ file.txt│      │ commit  │            │
│  │ (edit)  │      │ (staged)│      │ abc123  │            │
│  └─────────┘      └─────────┘      └─────────┘            │
│                                                             │
│  git reset HEAD <file>                                      │
│  ├──────────────────────────────────────────>               │
│  │ Moves from STAGING to WORKSPACE                          │
│                                                             │
│  git reset --soft <commit>                                  │
│  │                 ├──────────────────────>                 │
│  │                 │ Moves from REPO to STAGING             │
│                                                             │
│  git reset --mixed <commit>                                 │
│  ├──────────────────────────────────────────>               │
│  │ Moves from REPO to WORKSPACE                             │
│                                                             │
│  git reset --hard <commit>                                  │
│  │                                     X                    │
│  │ DELETES commit and all changes                           │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## Branching Visualization

```
┌─────────────────────────────────────────────────────────────┐
│                    BRANCH WORKFLOW                          │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  main:     A───B───C───────────F                           │
│                 \             /                             │
│  feature:        D───────E                                  │
│                                                             │
│  Commands:                                                  │
│  1. git checkout -b feature    (create branch at B)         │
│  2. git commit (D)             (work on feature)            │
│  3. git commit (E)             (more work)                  │
│  4. git checkout main          (switch to main)             │
│  5. git commit (C)             (work on main)               │
│  6. git merge feature          (merge at F)                 │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## Merge vs Rebase

```
┌─────────────────────────────────────────────────────────────┐
│                    MERGE vs REBASE                          │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  BEFORE:                                                    │
│  main:     A───B───C                                        │
│                 \                                           │
│  feature:        D───E                                      │
│                                                             │
│  ─────────────────────────────────────────────────────────  │
│                                                             │
│  AFTER MERGE:                                               │
│  main:     A───B───C───────F                                │
│                 \         /                                 │
│  feature:        D───E                                      │
│                                                             │
│  Command: git merge feature                                 │
│  Result: Creates merge commit F                             │
│                                                             │
│  ─────────────────────────────────────────────────────────  │
│                                                             │
│  AFTER REBASE:                                              │
│  main:     A───B───C───D'───E'                              │
│                                                             │
│  Command: git rebase main                                   │
│  Result: Moves D and E after C (linear history)             │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## Conflict Resolution

```
┌─────────────────────────────────────────────────────────────┐
│                  MERGE CONFLICT                             │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  main:     A───B (modified line 5)                          │
│                 \                                           │
│  feature:        C (modified line 5 differently)            │
│                                                             │
│  When merging:                                              │
│  ┌─────────────────────────────────────────────┐           │
│  │ <<<<<<< HEAD                                │           │
│  │ Line 5 from main                            │           │
│  │ =======                                     │           │
│  │ Line 5 from feature                         │           │
│  │ >>>>>>> feature                             │           │
│  └─────────────────────────────────────────────┘           │
│                                                             │
│  Resolution steps:                                          │
│  1. Edit file to resolve conflict                           │
│  2. Remove conflict markers                                 │
│  3. git add <file>                                          │
│  4. git commit                                              │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## Stash Workflow

```
┌─────────────────────────────────────────────────────────────┐
│                    STASH WORKFLOW                           │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  WORKSPACE                STASH                             │
│  ┌─────────────┐         ┌─────────────┐                   │
│  │ file.txt    │         │             │                   │
│  │ (modified)  │         │             │                   │
│  └─────────────┘         └─────────────┘                   │
│         │                                                   │
│         │ git stash save                                    │
│         │─────────────────────────>                         │
│         │                                                   │
│  ┌─────────────┐         ┌─────────────┐                   │
│  │ file.txt    │         │ stash@{0}   │                   │
│  │ (clean)     │         │ file.txt    │                   │
│  └─────────────┘         └─────────────┘                   │
│         │                       │                           │
│         │<──────────────────────│                           │
│         │   git stash pop       │                           │
│         │   (apply & remove)    │                           │
│         │                       │                           │
│         │<──────────────────────│                           │
│         │   git stash apply     │                           │
│         │   (apply & keep)      │                           │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## Remote Workflow

```
┌─────────────────────────────────────────────────────────────┐
│                  REMOTE WORKFLOW                            │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  LOCAL REPO              REMOTE REPO (GitHub)               │
│  ┌─────────────┐         ┌─────────────┐                   │
│  │   main      │         │   main      │                   │
│  │   A───B     │         │   A───B───C │                   │
│  └─────────────┘         └─────────────┘                   │
│         │                       │                           │
│         │   git pull            │                           │
│         │<──────────────────────│                           │
│         │                       │                           │
│  ┌─────────────┐         ┌─────────────┐                   │
│  │   main      │         │   main      │                   │
│  │   A───B───C │         │   A───B───C │                   │
│  └─────────────┘         └─────────────┘                   │
│         │                       │                           │
│         │ (make commit D)       │                           │
│         │                       │                           │
│  ┌─────────────┐         ┌─────────────┐                   │
│  │   main      │         │   main      │                   │
│  │   A───B───C─D         │   A───B───C │                   │
│  └─────────────┘         └─────────────┘                   │
│         │                       │                           │
│         │   git push            │                           │
│         │───────────────────────>                           │
│         │                       │                           │
│  ┌─────────────┐         ┌─────────────┐                   │
│  │   main      │         │   main      │                   │
│  │   A───B───C─D         │   A───B───C─D                   │
│  └─────────────┘         └─────────────┘                   │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## Cherry-Pick

```
┌─────────────────────────────────────────────────────────────┐
│                    CHERRY-PICK                              │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  main:     A───B───C───D───E                                │
│                                                             │
│  release:  A───B                                            │
│                                                             │
│  Want only commit D in release:                             │
│                                                             │
│  git checkout release                                       │
│  git cherry-pick D                                          │
│                                                             │
│  Result:                                                    │
│  main:     A───B───C───D───E                                │
│                                                             │
│  release:  A───B───D'                                       │
│                                                             │
│  Note: D' is a copy of D with new commit ID                 │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## Tag Visualization

```
┌─────────────────────────────────────────────────────────────┐
│                      TAGS                                   │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Commits:  A───────B───────C───────D───────E───────F        │
│            │       │       │       │       │       │        │
│          v0.1    v0.5    v1.0    v1.1    v1.2    v2.0      │
│                                                             │
│  Tags mark specific points in history                       │
│                                                             │
│  git tag v1.0 C          (tag commit C)                     │
│  git checkout v1.0       (view code at v1.0)                │
│  git push --tags         (push all tags)                    │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## Complete Workflow Example

```
┌─────────────────────────────────────────────────────────────┐
│              COMPLETE DEVELOPMENT WORKFLOW                  │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  1. Clone repository                                        │
│     git clone <url>                                         │
│                                                             │
│  2. Create feature branch                                   │
│     main:  A───B                                            │
│                 \                                           │
│     feature:     (branch created)                           │
│                                                             │
│  3. Work on feature                                         │
│     main:  A───B                                            │
│                 \                                           │
│     feature:     C───D                                      │
│                                                             │
│  4. Main branch updated                                     │
│     main:  A───B───E                                        │
│                 \                                           │
│     feature:     C───D                                      │
│                                                             │
│  5. Rebase feature on main                                  │
│     main:  A───B───E                                        │
│                     \                                       │
│     feature:         C'───D'                                │
│                                                             │
│  6. Merge to main                                           │
│     main:  A───B───E───C'───D'                              │
│                                                             │
│  7. Tag release                                             │
│     main:  A───B───E───C'───D'                              │
│                             │                               │
│                           v1.0                              │
│                                                             │
│  8. Push to remote                                          │
│     Local and Remote are now in sync                        │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## Git Command Flow

```
┌─────────────────────────────────────────────────────────────┐
│                  COMMAND FLOW CHART                         │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Start                                                      │
│    │                                                        │
│    ├─> git init / git clone                                │
│    │                                                        │
│    ├─> Edit files                                          │
│    │                                                        │
│    ├─> git status (check changes)                          │
│    │                                                        │
│    ├─> git diff (review changes)                           │
│    │                                                        │
│    ├─> git add (stage files)                               │
│    │                                                        │
│    ├─> git status (verify staging)                         │
│    │                                                        │
│    ├─> git commit -m "message"                             │
│    │                                                        │
│    ├─> git log (view history)                              │
│    │                                                        │
│    ├─> git push (upload to remote)                         │
│    │                                                        │
│    └─> Repeat                                              │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## State Transitions

```
┌─────────────────────────────────────────────────────────────┐
│                  FILE STATE TRANSITIONS                     │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Untracked ──────────────> Staged ──────────> Committed     │
│     │         git add         │    git commit      │        │
│     │                         │                    │        │
│     │<────────────────────────│                    │        │
│     │    git rm --cached      │                    │        │
│     │                         │                    │        │
│     │                         │<───────────────────│        │
│     │                         │   git reset --soft │        │
│     │                         │                    │        │
│     │<────────────────────────────────────────────│        │
│     │              git reset --mixed               │        │
│                                                             │
│  Modified ───────────────> Staged ──────────> Committed     │
│     │         git add         │    git commit      │        │
│     │                         │                    │        │
│     │<────────────────────────│                    │        │
│     │   git restore --staged  │                    │        │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## Collaboration Flow

```
┌─────────────────────────────────────────────────────────────┐
│                  TEAM COLLABORATION                         │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Developer 1:                                               │
│  ┌─────────────────────────────────────┐                   │
│  │ git clone                           │                   │
│  │ git checkout -b feature-a           │                   │
│  │ (work on feature)                   │                   │
│  │ git commit                          │                   │
│  │ git push origin feature-a           │                   │
│  │ (create pull request)               │                   │
│  └─────────────────────────────────────┘                   │
│                                                             │
│  Developer 2:                                               │
│  ┌─────────────────────────────────────┐                   │
│  │ git clone                           │                   │
│  │ git checkout -b feature-b           │                   │
│  │ (work on feature)                   │                   │
│  │ git commit                          │                   │
│  │ git push origin feature-b           │                   │
│  │ (create pull request)               │                   │
│  └─────────────────────────────────────┘                   │
│                                                             │
│  Maintainer:                                                │
│  ┌─────────────────────────────────────┐                   │
│  │ (review pull requests)              │                   │
│  │ git checkout main                   │                   │
│  │ git merge feature-a                 │                   │
│  │ git merge feature-b                 │                   │
│  │ git push                            │                   │
│  └─────────────────────────────────────┘                   │
│                                                             │
│  Result:                                                    │
│  main: A───B───C───D                                        │
│             │   │                                           │
│         feature-a feature-b                                 │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

These visual guides should help you understand Git concepts more intuitively!
