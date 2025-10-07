# Git Rebase

## Question
How would you combine the last 5 commits into a single commit using rebase, but only include the first 3 in the commit message?

## Answer
If we want to have all the last five commits combined, we have to use the `git rebase` command with HEAD as 5. We can use interactive mode `-i` next to git rebase so it will open a text editor like VI where we can edit the commits.

### Steps:

1. Open interactive rebase for the last 5 commits:
```bash
git rebase -i HEAD~5
```

2. This opens a text editor with the last five commits with their commit ids and `pick` before them.

3. Modify the commands as follows:
   - Keep the **first commit** as `pick`
   - Change the **second and third commits** to `squash` (this will add their messages)
   - Change the **fourth and fifth commits** to `fixup` (this will discard their commit messages)

### Example:
```bash
pick c8a888d 2 Lion Commit
squash 0cb14d5 3 Lion Commit
squash 4f4aae1 4 Lion Commit
fixup a2216f6 5 Lion Commit
fixup 0cb8f1d 6 Lion Commit
```

### Result:
- The commits will be combined into a single commit
- Only the first three commit messages will be included in the final commit message
- The last two commit messages will be discarded

## Handling Merge Conflicts During Rebase

When pushing from a feature branch to a main branch, if there are conflicting changes, it will not directly push. It will give us a merge conflict and show which files have the conflict.

### Steps to Resolve:

1. Git will show which files have merge conflicts
2. Edit each conflicting file:
   - Look for conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`)
   - Decide which changes to keep
   - Remove the conflict markers
3. Add the resolved files:
```bash
git add <resolved-files>
```
4. Continue the rebase:
```bash
git rebase --continue
```

This process continues until all conflicts are resolved and commits are merged successfully.
