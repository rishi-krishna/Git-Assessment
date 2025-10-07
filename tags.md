# Git Tags

## Question
How would you create a tag, push it to the remote repository, and delete it locally afterward?

## Answer
I have gone through Git SCM documentation to learn about tags.

### Creating a Tag

To create a lightweight tag in the local machine:
```bash
git tag <version>
# Example:
git tag v1.0.0
```

### Pushing Tag to Remote Repository

To push the tag to the remote repository:
```bash
git push origin <tagName>
# Example:
git push origin v1.0.0
```

This will reflect the tag in the remote repository.

### Deleting a Tag Locally

To delete a tag in the local machine:
```bash
git tag -d <tagName>
# Example:
git tag -d v1.0.0
```

Note: This will only delete the tag locally and will not affect the remote repository.

### Deleting a Tag from Remote Repository

If we want to remove the tag from the remote repository as well:
```bash
git push origin --delete <tagName>
# Example:
git push origin --delete v1.0.0
```

## Summary

1. **Create tag locally**: `git tag v1.0.0`
2. **Push to remote**: `git push origin v1.0.0`
3. **Delete locally**: `git tag -d v1.0.0`
4. **Delete from remote**: `git push origin --delete v1.0.0`

## Additional Tag Information

### Annotated Tags
For more detailed tags with metadata:
```bash
git tag -a v1.0.0 -m "Release version 1.0.0"
```

### Viewing Tags
```bash
git tag                    # List all tags
git show v1.0.0           # Show tag details
git tag -l "v1.*"         # List tags matching pattern
```
