# Git Pull Troubleshooting Guide

This document provides solutions for common `git pull` errors encountered in this repository.

## Fixed Issue: Restrictive Fetch Configuration

### Problem
The repository was configured with a restrictive fetch refspec that only tracked a single specific branch:
```
+refs/heads/copilot/fix-d98ca428-e5f0-4f41-b7a5-24db28953151:refs/remotes/origin/copilot/fix-d98ca428-e5f0-4f41-b7a5-24db28953151
```

### Solution
Updated the fetch refspec to the standard configuration:
```bash
git config remote.origin.fetch '+refs/heads/*:refs/remotes/origin/*'
```

This allows tracking of all remote branches and prevents errors when switching between branches or pulling updates.

## Common Git Pull Errors and Solutions

### 1. "Your local changes to the following files would be overwritten by merge"

**Error:**
```
error: Your local changes to the following files would be overwritten by merge:
    filename.txt
Please commit your changes or stash them before you merge.
```

**Solutions:**
```bash
# Option 1: Commit your changes
git add .
git commit -m "Save local changes"
git pull

# Option 2: Stash your changes temporarily
git stash
git pull
git stash pop

# Option 3: Discard local changes (use with caution)
git checkout -- filename.txt
git pull
```

### 2. "There is no tracking information for the current branch"

**Error:**
```
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
```

**Solution:**
```bash
# Set up tracking for the current branch
git branch --set-upstream-to=origin/branch-name

# Or pull with explicit remote and branch
git pull origin branch-name
```

### 3. "fatal: refusing to merge unrelated histories"

**Error:**
```
fatal: refusing to merge unrelated histories
```

**Solution:**
```bash
git pull origin main --allow-unrelated-histories
```

### 4. Merge Conflicts

**Error:**
```
Auto-merging filename.txt
CONFLICT (content): Merge conflict in filename.txt
Automatic merge failed; fix conflicts and then commit the result.
```

**Solution:**
```bash
# 1. Edit the conflicted files to resolve conflicts
# 2. Add the resolved files
git add filename.txt
# 3. Complete the merge
git commit
```

### 5. "Permission denied (publickey)" or Authentication Errors

**Solutions:**
```bash
# Check if you're using the correct remote URL
git remote -v

# For HTTPS (recommended for this repo)
git remote set-url origin https://github.com/username/repository.git

# Verify authentication
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

### 6. "fatal: Not a git repository"

**Solution:**
Make sure you're in the correct directory:
```bash
cd /path/to/your/repository
# Or initialize a new repository if needed
git init
```

## Best Practices to Prevent Issues

1. **Always check status before pulling:**
   ```bash
   git status
   ```

2. **Regularly fetch to stay updated:**
   ```bash
   git fetch origin
   ```

3. **Keep your working directory clean:**
   ```bash
   git stash  # before switching branches
   ```

4. **Verify remote configuration:**
   ```bash
   git remote -v
   git config --get remote.origin.fetch
   ```

## Quick Diagnostic Commands

```bash
# Check current branch and status
git status

# Check remote configuration
git remote -v
git remote show origin

# Check branch tracking
git branch -vv

# View recent commits
git log --oneline -10

# Check for uncommitted changes
git diff
git diff --staged
```

## Repository-Specific Notes

This repository now has proper fetch configuration that allows tracking all remote branches:
- `master` - Main branch
- `2019-branch` - Legacy branch
- Various `copilot/*` branches - Feature branches

All branches should now be accessible via standard git commands after the configuration fix.