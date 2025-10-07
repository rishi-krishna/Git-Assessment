# Git Pre-Commit Hook

## Question
How would you write a pre-commit hook that prevents commits if there are any TODO comments in the staged files, but allows them if they are inside .md files?

## Answer
We need to write a pre-commit hook script that checks for TODO comments in staged files while excluding markdown files.

### Implementation Steps:

1. **Get all staged files** that are different from the previous commit
2. **Check if the file has .md extension**
3. **Loop through each staged file** (excluding .md files)
4. **Search for TODO comments** using grep
5. **Prevent commit** if TODO is found (exit with code 1)
6. **Allow commit** if no TODO found (exit with code 0)

### Pre-Commit Hook Script:

```bash
#!/bin/bash

# Get all staged files
stagedfiles=$(git diff --cached --name-only)
# --cached shows changes in staging area compared to last commit

# Loop through each staged file
for file in $stagedfiles
do
    # Check if file is NOT a markdown file
    if [[ $file != *.md ]]; then
        # Search for TODO comment in the file
        if grep -n "TODO" "$file"; then
            echo "Error: TODO comment found in $file"
            echo "Please fix TODO before committing."
            exit 1
        fi
    fi
done

# If no TODO found, allow commit
exit 0
```

### Setup Instructions:

1. **Save the script** as `.git/hooks/pre-commit`
2. **Make it executable**:
```bash
chmod +x .git/hooks/pre-commit
```

### How It Works:

- **`git diff --cached --name-only`**: Gets list of all files staged for commit
- **`for file in $stagedfiles`**: Iterates through each staged file
- **`if [[ $file != *.md ]]`**: Checks if file is NOT a markdown file
- **`grep -n "TODO" "$file"`**: Searches for TODO comment and shows line number
- **`exit 1`**: Prevents commit if TODO found
- **`exit 0`**: Allows commit if no TODO found or TODO only in .md files

### Example Scenarios:

**Scenario 1: TODO in JavaScript file**
```javascript
// TODO: Fix this bug
function example() {
  // code here
}
```
**Result**: Commit blocked ❌

**Scenario 2: TODO in Markdown file**
```markdown
# Project Tasks
- TODO: Update documentation
```
**Result**: Commit allowed ✅

**Scenario 3: No TODO comments**
```javascript
function example() {
  // Implemented properly
}
```
**Result**: Commit allowed ✅
