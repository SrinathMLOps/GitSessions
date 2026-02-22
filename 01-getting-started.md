# Getting Started with Git

## What is Git?

Git is a distributed Source Code Management (SCM) tool that helps you track changes in your code and collaborate with others.

## Installation

Download Git according to your operating system from [git-scm.com](https://git-scm.com) and install it.

Verify installation:
```bash
git --version
```

## Initial Configuration

### Setting Up User Information

```bash
# Set your name
git config --global user.name "Your Name"

# Set your email
git config --global user.email "your.email@example.com"
```

### Verify Configuration

```bash
# List all configurations
git config --list
```

### Editing Configuration

You can edit existing user information by running the same commands again with new values.

**Note:** Git is a local machine tool, so you cannot create multiple users on the same machine. The configuration is global.

## Removing Configuration

```bash
# Remove username
git config --global --unset user.name

# Remove email
git config --global --unset user.email
```

## Example Workflow

```bash
# 1. Check Git version
git --version

# 2. Configure user
git config --global user.name "John Doe"
git config --global user.email "john.doe@example.com"

# 3. Verify settings
git config --list

# Expected output:
# user.name=John Doe
# user.email=john.doe@example.com
```

## Common Issues

### Issue: Configuration not saving
**Solution:** Make sure you're using the `--global` flag for system-wide configuration.

### Issue: Wrong email configured
**Solution:** Simply run the config command again with the correct email.

## Next Steps

Once you've configured Git, you're ready to learn about [Git Basics and the Three Phases](./02-git-basics.md).
