# Removing Sensitive Data from Git History

## Question
You accidentally committed a file containing sensitive information. How would you remove it from all commits in the repository history?

## Answer

To remove sensitive data from all commits in the repository history, we need to use `git filter-repo` tool.

### Steps:

#### 1. Add File to .gitignore
First, add the file to `.gitignore` to prevent it from being tracked in the future:

```bash
echo "sensitive-file.txt" >> .gitignore
git add .gitignore
git commit -m "Add sensitive file to gitignore"
```

#### 2. Install git-filter-repo

According to GitHub docs, there is a command called `git filter-repo`. We need to install it first:

**Using Homebrew (macOS/Linux):**
```bash
brew install git-filter-repo
```

**Using Python:**
```bash
# Download the Python script from GitHub
wget https://raw.githubusercontent.com/newren/git-filter-repo/main/git-filter-repo
chmod +x git-filter-repo
# Move to PATH location
sudo mv git-filter-repo /usr/local/bin/
```

#### 3. Remove the File from History

Use `git filter-repo` to remove the file from all commits:

```bash
git filter-repo --invert-paths --path path/to/sensitive-file.txt
```

**Parameters:**
- `--invert-paths`: Keeps everything EXCEPT the specified path
- `--path`: Specifies the file path to remove

**This will:**
- Remove all instances of the file from all branches
- Remove from all tags
- Remove from all refs
- Rewrite the entire commit history

#### 4. Verify Removal

Check if there are any remaining instances using grep:

```bash
git log --all --full-history -- "*sensitive-file.txt*"
# Or search in refs and commit messages
git grep -i "sensitive" $(git rev-list --all)
```

#### 5. Force Push to Remote

Once satisfied with the changes, force push to update the remote repository:

```bash
git push origin --force --all
git push origin --force --tags
```

## Alternative Method: BFG Repo-Cleaner

BFG is a faster alternative to git-filter-repo for removing sensitive data:

```bash
# Install BFG
brew install bfg

# Remove file by name
bfg --delete-files sensitive-file.txt

# Or remove files matching a pattern
bfg --delete-files '*.key'

# Or remove files larger than a certain size
bfg --strip-blobs-bigger-than 10M

# Clean up and push
git reflog expire --expire=now --all
git gc --prune=now --aggressive
git push origin --force --all
```

## Important Warnings

⚠️ **Warning:** Force pushing rewrites history and will affect all collaborators

⚠️ **Action Required:** All team members must:
1. Backup their work
2. Delete their local clones
3. Re-clone the repository

⚠️ **Security Note:** If the sensitive data was already pushed to a public repository, consider it compromised:
- Rotate all credentials immediately
- Change passwords
- Revoke API keys
- Update secrets

## Best Practices to Prevent This

1. **Use .gitignore** from the start
2. **Use environment variables** for sensitive data
3. **Use secret management tools** (AWS Secrets Manager, HashiCorp Vault)
4. **Review commits** before pushing
5. **Use pre-commit hooks** to scan for secrets
