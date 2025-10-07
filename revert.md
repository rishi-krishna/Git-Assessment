# Git Revert

## Question
How would you go back to the previous stage?

## Answer
We can use git revert command to go back to the previous stage.

```bash
git revert <commit-id>
```

## Getting the Commit ID
To get the commit id, we can explore `git log` and other commands:

```bash
git log
git log --oneline
git log -n 5  # Shows last 5 commits
```

The git log command will display the commit history with their commit IDs, which can then be used with git revert.

## Git Revert vs Git Reset

### Git Reset --hard
- Goes back to the last commit and removes all changes in the current working directory
- If working on something that hasn't been added to the index, it will remove all those changes
- Applies the last commit to the current working repository
- Does NOT keep track of the commit

### Git Revert
- Actually goes back one step and creates another commit with the reverse changes
- If you added one or two lines in your code, it will remove those lines and commit the changes
- Keeps track of the commit in history

### Example
```bash
git revert HEAD~1
```

This command reverts the changes made in the previous commit while maintaining commit history.
