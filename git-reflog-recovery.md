# Git Reflog and Recovery

## Question
If a teammate accidentally force-pushed a shared branch, how would you recover the previous state without affecting new changes?

## Answer

When a teammate accidentally force-pushes a shared branch, we can use `git reflog` to recover the previous state.

### What is Git Reflog?

`git reflog` is a mechanism that records all major operations in your local repository, including:
- Commits
- Checkouts
- Resets
- Merges
- Rebases
- Pull/Push operations
- Force pushes
- Deleted branches/commits

These are usually not shown in normal `git log`, but reflog keeps track of them all.

### Recovery Steps:

#### 1. View the Reflog

First, check the reflog to see all recent operations:

```bash
git reflog
# or for more detailed view
git reflog show --all
```

**Example Output:**
```
abc123d HEAD@{0}: pull: Fast-forward
789xyz4 HEAD@{1}: commit: Fix bug in authentication
def456e HEAD@{2}: reset: moving to HEAD~1
321ghi7 HEAD@{3}: commit: Add new feature
```

#### 2. Find the Previous Commit

Locate the commit that existed before the accidental force push:

```bash
git reflog show origin/feature-branch
```

Identify the commit hash from before the force push.

#### 3. Recover the Previous State

**Option A: Create a new branch from the old commit**
```bash
# Create a recovery branch
git branch recovery-branch <commit-hash-before-push>
git checkout recovery-branch
```

**Option B: Reset to the previous commit (if no new changes)**
```bash
# Hard reset to previous state
git reset --hard <commit-hash-before-push>
```

**Option C: Cherry-pick specific commits**
```bash
# Pick specific commits you want to keep
git cherry-pick <commit-hash>
```

#### 4. Preserve New Changes

If there are new valuable changes after the force push:

```bash
# 1. Checkout the recovery branch
git checkout recovery-branch

# 2. Cherry-pick the new commits
git cherry-pick <new-commit-1> <new-commit-2>

# Or merge the new changes
git merge --no-ff <branch-with-new-changes>
```

#### 5. Push the Recovered State

```bash
# Force push the recovered state (coordinate with team first!)
git push origin recovery-branch --force

# Or push to a new branch for review
git push origin recovery-branch:recovered-feature
```

## Advanced Reflog Commands

### View Reflog for Specific Branch
```bash
git reflog show branch-name
```

### View Reflog with Dates
```bash
git reflog show --date=iso
```

### Find Commits from Specific Time
```bash
# Show reflog from 2 hours ago
git reflog show 'HEAD@{2 hours ago}'

# Show reflog from yesterday
git reflog show 'HEAD@{yesterday}'
```

### Recover Deleted Branch
```bash
# Find the deleted branch in reflog
git reflog | grep 'branch-name'

# Recreate the branch
git branch recovered-branch <commit-hash>
```

## Important Notes

⚠️ **Reflog is local**: Each team member has their own reflog. It doesn't sync with remote.

⚠️ **Reflog expires**: By default, reflog entries expire after 90 days (30 days for unreachable commits).

⚠️ **Communication**: Always coordinate with your team before force-pushing recovery.

## Prevention Best Practices

1. **Never force push shared branches** without team coordination
2. **Use `--force-with-lease`** instead of `--force` for safer force pushes
3. **Create backup branches** before risky operations
4. **Enable branch protection** rules on important branches
5. **Use pull request workflows** instead of direct pushes

## Example Scenario

**Situation**: Teammate force-pushed feature-branch, losing 5 commits

**Solution:**
```bash
# 1. Check reflog
git reflog show origin/feature-branch

# 2. Found commit before force push: abc1234
# Create recovery branch
git branch feature-branch-recovery abc1234

# 3. Get new commits from current branch
git log feature-branch --oneline -n 3

# 4. Cherry-pick valuable new commits
git checkout feature-branch-recovery
git cherry-pick def5678 ghi9012

# 5. Replace broken branch
git branch -D feature-branch
git checkout -b feature-branch
git push origin feature-branch --force-with-lease
```

## Conclusion

`git reflog` is a powerful recovery tool that keeps track of all repository operations. It's your safety net when things go wrong, allowing you to recover seemingly lost commits and branches.
