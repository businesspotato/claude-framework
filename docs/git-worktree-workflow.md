# Git Worktree Workflow for Parallel Development

## Overview

Git worktrees enable multiple working directories from a single repository, allowing different Claude Code instances to work on separate branches simultaneously without conflicts. This is ideal for parallel task execution, isolated feature development, and multi-instance coordination.

## Table of Contents

- [What is Git Worktree?](#what-is-git-worktree)
- [When to Use Worktrees](#when-to-use-worktrees)
- [Setup Guide](#setup-guide)
- [Branch Strategy](#branch-strategy)
- [Multi-Instance Coordination](#multi-instance-coordination)
- [Merge Strategies](#merge-strategies)
- [Best Practices](#best-practices)
- [Cleanup Procedures](#cleanup-procedures)
- [Troubleshooting](#troubleshooting)

---

## What is Git Worktree?

Git worktree creates **separate working directories** that share the same Git repository metadata. Each worktree:
- Has its own branch checked out
- Has independent working files
- Shares the same `.git` directory (saving disk space)
- Can be worked on simultaneously without conflicts

**Traditional Git**:
```
one-piece-tcg-manager/  (single branch at a time)
```

**With Worktrees**:
```
one-piece-tcg-manager/     (main development - dangerous_dev)
optcg-cleanup/             (cleanup branch)
optcg-bugfix/              (bugfix branch)
optcg-database/            (database branch)
optcg-features/            (features branch)
```

---

## When to Use Worktrees

### ✅ **Use Worktrees When:**

1. **Multiple Claude Code Instances** need to work on different tasks simultaneously
2. **Isolated Development** required (cleanup vs features vs bugfixes)
3. **Testing Different Approaches** on separate branches without switching
4. **Long-Running Features** that shouldn't block quick fixes
5. **Review Work** while continuing development on another branch

### ❌ **Don't Use Worktrees When:**

1. **Simple Sequential Work** on a single branch is sufficient
2. **Disk Space is Critical** (worktrees duplicate working files)
3. **Tasks are Interdependent** and need constant integration
4. **Only One Developer/Instance** working on the project

---

## Setup Guide

### Basic Worktree Creation

```bash
# Create new branch and worktree in one command
git worktree add -b branch-name ../worktree-directory

# Example: Create cleanup worktree
git worktree add -b cleanup/codebase-organization ../optcg-cleanup
```

### Multi-Branch Setup (Recommended Pattern)

```bash
# From main development directory
cd /path/to/one-piece-tcg-manager

# Create worktrees for different task categories
git worktree add -b cleanup/codebase-organization ../optcg-cleanup
git worktree add -b fix/variant-pricing-mismatch ../optcg-bugfix
git worktree add -b feature/database-optimization ../optcg-database
git worktree add -b feature/cache-and-price-tracking ../optcg-features
```

### Verify Worktrees

```bash
git worktree list

# Expected output:
# /path/to/one-piece-tcg-manager        abc1234 [dangerous_dev]
# /path/to/optcg-cleanup                abc1234 [cleanup/codebase-organization]
# /path/to/optcg-bugfix                 abc1234 [fix/variant-pricing-mismatch]
# /path/to/optcg-database               abc1234 [feature/database-optimization]
# /path/to/optcg-features               abc1234 [feature/cache-and-price-tracking]
```

---

## Branch Strategy

### Branch Naming Conventions

| Category | Prefix | Example |
|----------|--------|---------|
| Cleanup/Organization | `cleanup/` | `cleanup/codebase-organization` |
| Bug Fixes | `fix/` | `fix/variant-pricing-mismatch` |
| Features | `feature/` | `feature/database-optimization` |
| Documentation | `docs/` | `docs/api-reference-update` |
| Performance | `perf/` | `perf/cache-optimization` |
| Refactoring | `refactor/` | `refactor/service-consolidation` |

### Task Categorization

**Category 1: Cleanup & Organization** (Non-code changes)
- File reorganization
- Documentation merging
- Removing obsolete files
- Archive management
- **Risk**: Low
- **Merge Priority**: High (can merge immediately)

**Category 2: Bug Fixes** (Quick wins)
- Critical bug fixes
- Performance issues
- UI/UX corrections
- **Risk**: Low-Medium
- **Merge Priority**: High

**Category 3: Database Changes** (Schema modifications)
- Migration scripts
- Schema refactoring
- Index optimization
- **Risk**: High (requires careful review)
- **Merge Priority**: Medium

**Category 4: Feature Development** (New functionality)
- New services
- UI components
- API endpoints
- **Risk**: Medium
- **Merge Priority**: Low (thorough testing required)

### Recommended Strategy by Project Size

**Small Project (1-3 tasks)**:
```bash
# Single worktree for all tasks
git worktree add -b feature/improvements ../optcg-tasks
```

**Medium Project (4-6 tasks)**:
```bash
# Two worktrees: cleanup+bugfix vs features
git worktree add -b cleanup/organization-and-bugfix ../optcg-quick-wins
git worktree add -b feature/system-improvements ../optcg-features
```

**Large Project (7+ tasks)**:
```bash
# Multiple worktrees by category
git worktree add -b cleanup/codebase-organization ../optcg-cleanup
git worktree add -b fix/critical-bugs ../optcg-bugfix
git worktree add -b feature/database-refactor ../optcg-database
git worktree add -b feature/new-features ../optcg-features
```

---

## Multi-Instance Coordination

### Instance Assignment Strategy

**Primary Instance (dangerous_dev)**:
- Active feature development
- User-driven tasks
- Main development branch

**Secondary Instance (cleanup branch)**:
- File organization
- Documentation updates
- Non-code improvements

**Tertiary Instance (bugfix branch)**:
- Bug fixes
- Performance improvements
- Quick wins

**Quaternary Instance (database branch)**:
- Schema changes
- Migration scripts
- Database optimization

**Quinary Instance (features branch)**:
- New feature development
- Complex integrations
- Long-running tasks

### Coordination Files

**Update status in each worktree**:
```bash
# In cleanup worktree
cd ../optcg-cleanup
echo "STATUS: Cleanup in progress" > .worktree-status

# In features worktree
cd ../optcg-features
echo "STATUS: Implementing cache monitor" > .worktree-status
```

**Centralized coordination** (optional):
```bash
# Create coordination file in main repo
cat > status/worktree-coordination.json << 'EOF'
{
  "worktrees": {
    "cleanup": {
      "branch": "cleanup/codebase-organization",
      "status": "in_progress",
      "instance": "Claude Instance 2",
      "task": "File organization",
      "eta": "1-2 hours"
    },
    "bugfix": {
      "branch": "fix/variant-pricing-mismatch",
      "status": "pending",
      "instance": "Claude Instance 2",
      "task": "Price Scout cache fix",
      "eta": "30-45 minutes"
    }
  }
}
EOF
```

### Terminal Setup for Multi-Instance Work

**Option 1: Multiple Terminal Windows**
```bash
# Terminal 1 (Main development)
cd ~/Projects/OP\ TCG/one-piece-tcg-manager
npm start

# Terminal 2 (Cleanup work)
cd ~/Projects/OP\ TCG/optcg-cleanup
claude-code

# Terminal 3 (Feature development)
cd ~/Projects/OP\ TCG/optcg-features
claude-code
```

**Option 2: tmux/screen Sessions**
```bash
# Create tmux session with multiple panes
tmux new-session -s optcg-dev

# Split into 4 panes (Ctrl+b %)
# Navigate to different worktrees in each pane
```

---

## Merge Strategies

### Sequential Merge (Recommended)

Merge worktrees in order of risk and completion:

```bash
# 1. Merge cleanup first (low risk, immediate benefit)
git checkout dangerous_dev
git merge cleanup/codebase-organization
git push origin cleanup/codebase-organization

# 2. Merge bugfix (quick win)
git merge fix/variant-pricing-mismatch
git push origin fix/variant-pricing-mismatch

# 3. Merge database (review carefully)
git merge feature/database-optimization
git push origin feature/database-optimization

# 4. Merge features (comprehensive testing)
git merge feature/cache-and-price-tracking
git push origin feature/cache-and-price-tracking
```

### Parallel Merge (Advanced)

If branches are completely independent:

```bash
# Merge multiple branches into integration branch
git checkout -b integration/all-improvements
git merge cleanup/codebase-organization
git merge fix/variant-pricing-mismatch
git merge feature/database-optimization
git merge feature/cache-and-price-tracking

# Resolve conflicts
# Run comprehensive tests

# Merge integration branch to main
git checkout dangerous_dev
git merge integration/all-improvements
```

### Conflict Resolution

**Common Conflicts**:
- Same file modified in multiple worktrees
- Package.json dependencies
- Migration file numbers

**Resolution Strategy**:
```bash
# When merge conflict occurs
git status  # See conflicted files

# Manually resolve conflicts in editor
# Look for <<<<<<< HEAD markers

# After resolving
git add <resolved-files>
git commit -m "Resolve merge conflicts between cleanup and database branches"
```

---

## Best Practices

### 1. **Keep Worktrees Focused**
- Each worktree should have a clear, single purpose
- Don't mix unrelated changes in one worktree
- Use separate worktrees for cleanup vs development

### 2. **Regular Status Updates**
```bash
# Check all worktree statuses
for worktree in ../optcg-*; do
  echo "=== $(basename $worktree) ==="
  cd "$worktree"
  git status --short
  cd -
done
```

### 3. **Commit Often in Worktrees**
```bash
# Small, focused commits
cd ../optcg-cleanup
git add scripts/testing/ebay/
git commit -m "organize: move eBay test files to scripts/testing/ebay/"

# Easier to cherry-pick or revert if needed
```

### 4. **Use .gitignore Carefully**
- Worktrees share the same `.gitignore`
- Add worktree-specific ignore patterns to `.git/info/exclude` instead

### 5. **Document Worktree Purpose**
```bash
# Create README in each worktree
cd ../optcg-cleanup
cat > WORKTREE.md << 'EOF'
# Cleanup Worktree

**Branch**: cleanup/codebase-organization
**Purpose**: File organization and documentation cleanup
**Status**: In Progress
**ETA**: 1-2 hours

## Tasks:
- [ ] Remove tmp_instance files
- [ ] Organize JS test files
- [ ] Merge documentation
- [ ] Archive logs
EOF
```

### 6. **Sync Worktrees with Main Branch**
```bash
# Pull latest changes from main to worktree
cd ../optcg-cleanup
git fetch origin
git rebase origin/dangerous_dev

# Or merge if rebase conflicts
git merge origin/dangerous_dev
```

---

## Cleanup Procedures

### Remove Individual Worktree

```bash
# Remove worktree (preserves branch)
git worktree remove ../optcg-cleanup

# Delete branch after merging
git branch -d cleanup/codebase-organization
```

### Remove All Worktrees

```bash
# List all worktrees
git worktree list

# Remove each worktree
git worktree remove ../optcg-cleanup
git worktree remove ../optcg-bugfix
git worktree remove ../optcg-database
git worktree remove ../optcg-features

# Prune stale worktrees
git worktree prune
```

### Cleanup Script

```bash
#!/bin/bash
# cleanup-worktrees.sh

echo "Removing all OPTCG worktrees..."

for worktree in ../optcg-*; do
  if [ -d "$worktree" ]; then
    echo "Removing $(basename $worktree)..."
    git worktree remove "$worktree" --force
  fi
done

echo "Pruning stale worktree references..."
git worktree prune

echo "Worktree cleanup complete!"
```

---

## Troubleshooting

### Issue: "fatal: '<path>' already exists"

**Cause**: Worktree directory already exists

**Solution**:
```bash
# Remove existing directory first
rm -rf ../optcg-cleanup

# Then create worktree
git worktree add -b cleanup/codebase-organization ../optcg-cleanup
```

### Issue: "fatal: '<branch>' is already checked out at '<path>'"

**Cause**: Branch is checked out in another worktree

**Solution**:
```bash
# List worktrees to find where branch is checked out
git worktree list

# Remove old worktree first
git worktree remove <path>

# Or use different branch name
git worktree add -b cleanup/codebase-organization-v2 ../optcg-cleanup
```

### Issue: "Worktree is locked"

**Cause**: Worktree was not removed cleanly

**Solution**:
```bash
# Unlock worktree
git worktree unlock <path>

# Then remove
git worktree remove <path>
```

### Issue: Changes in one worktree not visible in another

**Cause**: Working on different branches (expected behavior)

**Solution**:
```bash
# Commit changes in first worktree
cd ../optcg-cleanup
git add .
git commit -m "cleanup: organize files"

# Fetch and merge in second worktree
cd ../optcg-features
git fetch
git merge cleanup/codebase-organization
```

### Issue: NPM dependencies out of sync

**Cause**: package.json modified in multiple worktrees

**Solution**:
```bash
# Run npm install in each worktree after pulling changes
for worktree in ../optcg-*; do
  cd "$worktree"
  npm install
  cd -
done
```

---

## Example Workflow: One Piece TCG Manager

### Scenario: Parallel Development on 5 Tasks

**Setup**:
```bash
# Create worktrees
git worktree add -b cleanup/codebase-organization ../optcg-cleanup
git worktree add -b fix/variant-pricing-mismatch ../optcg-bugfix
git worktree add -b feature/database-optimization ../optcg-database
git worktree add -b feature/cache-and-price-tracking ../optcg-features
```

**Execution**:
```bash
# Claude Instance 1: Main development (dangerous_dev)
cd ~/Projects/OP\ TCG/one-piece-tcg-manager
npm start

# Claude Instance 2: Cleanup work
cd ~/Projects/OP\ TCG/optcg-cleanup
# Remove tmp files, organize scripts, merge docs

# Claude Instance 2: After cleanup, move to bugfix
cd ~/Projects/OP\ TCG/optcg-bugfix
# Fix Price Scout cache issue

# Claude Instance 2: After bugfix, move to database
cd ~/Projects/OP\ TCG/optcg-database
# Consolidate pricing tables, optimize schema

# Claude Instance 2: Final task, features
cd ~/Projects/OP\ TCG/optcg-features
# Implement cache monitor and price tracking
```

**Merge**:
```bash
# From main directory
cd ~/Projects/OP\ TCG/one-piece-tcg-manager

# Sequential merge (recommended)
git merge cleanup/codebase-organization
git push origin cleanup/codebase-organization

git merge fix/variant-pricing-mismatch
git push origin fix/variant-pricing-mismatch

git merge feature/database-optimization
git push origin feature/database-optimization

git merge feature/cache-and-price-tracking
git push origin feature/cache-and-price-tracking
```

**Cleanup**:
```bash
# Remove worktrees
git worktree remove ../optcg-cleanup
git worktree remove ../optcg-bugfix
git worktree remove ../optcg-database
git worktree remove ../optcg-features

# Delete merged branches
git branch -d cleanup/codebase-organization
git branch -d fix/variant-pricing-mismatch
git branch -d feature/database-optimization
git branch -d feature/cache-and-price-tracking
```

---

## Summary

Git worktrees enable:
- ✅ Parallel development on multiple branches
- ✅ Isolated task execution without conflicts
- ✅ Efficient multi-instance coordination
- ✅ Safe experimentation without switching contexts

**When to use**: Multiple independent tasks, parallel instance work, long-running features

**When not to use**: Simple sequential work, interdependent tasks, disk space constraints

**Best practices**: Focused worktrees, regular commits, clear documentation, sequential merging

---

*Last Updated: October 2025*
*Framework Version: 4.1.0*
