# Git Log Commands

## Question
How would you list the last 2 commits along with the names of files that were changed?

## Answer

To view the last 2 commits along with the names of files that were changed, we can use:

```bash
git log -n 2 --name-only
```

### Breakdown:

- **`git log`**: Shows commit history
- **`-n 2`**: Limits output to the last 2 commits
- **`--name-only`**: Shows only the names of changed files

### Example Output:

```
commit abc123def456...
Author: John Doe <john@example.com>
Date:   Mon Oct 6 21:30:00 2025 -0500

    Update documentation

README.md
docs/guide.md

commit 789ghi012jkl...
Author: John Doe <john@example.com>
Date:   Mon Oct 6 20:15:00 2025 -0500

    Fix bug in authentication

src/auth.js
tests/auth.test.js
```

## Alternative Commands

### Show file names with status (added, modified, deleted):
```bash
git log -n 2 --name-status
```

Output includes:
- `A` = Added
- `M` = Modified
- `D` = Deleted

### Show commit summary with statistics:
```bash
git log -n 2 --stat
```

Shows:
- File names
- Number of insertions and deletions
- Visual bar graph of changes

### One-line format with file names:
```bash
git log -n 2 --oneline --name-only
```

More compact output showing:
- Short commit hash
- Commit message
- Changed file names

## Additional Useful Git Log Options

### Filter by author:
```bash
git log -n 2 --author="John Doe" --name-only
```

### Filter by date:
```bash
git log --since="2 days ago" --name-only
```

### Show full diff for each file:
```bash
git log -n 2 -p
```

### Pretty format:
```bash
git log -n 2 --pretty=format:"%h - %an, %ar : %s" --name-only
```
