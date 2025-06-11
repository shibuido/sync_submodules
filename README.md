# sync_submodules

<!-- README started https://claude.ai/public/artifacts/1877c118-283c-4c9e-a1e5-abd90e6e7004 ; however Aider with OpenAI o3 also were helping later, so it's joined effort of people and many AIs :) -->

A robust bash script for synchronizing git repositories in a team environment using a "superrepo" pattern with submodules.

## 🎯 Purpose

This script automates the complex task of keeping multiple git repositories synchronized across a team. It's designed for teams that use a "superrepo" containing all team repositories as submodules, ensuring everyone stays in sync without needing deep git submodule expertise.

## 🚀 Quick Start

```bash
# Make the script executable (one-time setup)
chmod +x sync_submodules

# Run from your superrepo root
./sync_submodules
```

## 📋 Prerequisites

- Git 2.13 or later
- Bash 4.0 or later
- Unix-like environment (Linux, macOS, WSL on Windows)
- SSH keys or credentials configured for all repositories

## 💡 Why This Script Exists

Working with git submodules can be challenging:
- Manual submodule management is error-prone
- Team members often forget to update submodule references
- Conflicts between team members' changes can be confusing
- Keeping multiple repositories in sync requires many commands

This script solves these problems by:
- Automating the pull/push workflow
- Detecting conflicts early and providing clear guidance
- Updating superrepo references automatically
- Providing team-friendly error messages and resolution steps

## 🔄 How It Works

The script follows a three-step process:

### Step 1: Sync Superrepo
- Fetches latest changes from remote
- Checks for uncommitted changes
- Pulls updates if behind remote
- Pushes local commits if ahead

### Step 2: Sync All Submodules
- Recursively initializes and updates all submodules
- Performs the same sync process for each submodule
- Provides clear error messages if any submodule fails

### Step 3: Update References
- Detects if any submodule has new commits
- Creates a descriptive commit in the superrepo
- Pushes the reference updates to remote

## 📖 Common Usage Scenarios

### Daily Workflow
```bash
# Start your day by syncing everything
./sync_submodules

# Work on your changes in submodules
cd path/to/submodule
# ... make changes ...
git add .
git commit -m "feat: add new feature"

# Sync everything at the end of the day
cd /path/to/superrepo
./sync_submodules
```

### After Making Changes
When you've made commits in one or more submodules:

```bash
# The script will:
# 1. Push your submodule changes
# 2. Update superrepo references
# 3. Push the updated references
./sync_submodules
```

### Resolving Conflicts
If the script detects conflicts, it will stop and provide guidance:

```
ERROR: submodule:frontend has diverged from remote!
WARN: Your branch and remote have different commits.
WARN: Options:
  1. Merge: git pull --no-rebase
  2. Rebase: git pull --rebase
  3. Force push (DANGEROUS): git push --force-with-lease
WARN: Recommended: Use rebase if your commits are not shared yet
```

## 🛠️ Features

### Safety First
- Never performs destructive operations automatically
- Checks for uncommitted changes before any operations
- Uses `--ff-only` for pulls by default
- Provides warnings before any potentially dangerous operations

### Clear Communication
- Color-coded output:
  - 🟢 **INFO** (green): Normal operations
  - 🟡 **WARN** (yellow): Important notices
  - 🔴 **ERROR** (red): Problems requiring action
- Descriptive messages explaining what's happening
- Specific resolution steps when problems occur

### Team-Friendly
- No git submodule expertise required
- Automatic reference updates with descriptive commits
- Clear guidance for escalating to tech leads
- Handles recursive submodules automatically

## ⚠️ Troubleshooting

### "Not in a git repository!"
**Solution**: Run the script from your superrepo root directory.

### "Uncommitted changes detected"
**Solution**: Commit or stash your changes before syncing:
```bash
# Option 1: Commit changes
git add .
git commit -m "wip: current work"

# Option 2: Stash changes
git stash push -m "before sync"
```

### "No upstream branch set"
**Solution**: Set the upstream branch:
```bash
git branch --set-upstream-to=origin/main main
```

### "Failed to push changes"
**Possible causes**:
- No push permissions → Contact repository admin
- Protected branch rules → Follow team's PR process
- Network issues → Check connection and retry

### Submodule Authentication Issues
If you see authentication errors for submodules:
```bash
# Use SSH URLs for all submodules
git config --global url."git@github.com:".insteadOf "https://github.com/"

# Or set up credential caching
git config --global credential.helper cache
```

## 🔧 Configuration

The script uses these git configurations:
- `git fetch --all --prune`: Fetches all remotes and prunes deleted branches
- `git pull --ff-only`: Only fast-forward pulls by default (safe)
- `git submodule update --init --recursive`: Handles nested submodules

## 🏗️ Repository Structure

Expected structure:
```
superrepo/
├── sync_submodules         # This script
├── sync_submodules.README.md
├── .gitmodules            # Submodule definitions
├── project-a/             # Submodule
├── project-b/             # Submodule
└── shared-libs/           # Submodule (may contain more submodules)
```

## 👥 Team Workflow Best Practices

1. **Run sync at the start of your work session**
   - Ensures you have the latest changes from teammates

2. **Commit frequently in submodules**
   - Smaller commits are easier to sync and resolve

3. **Run sync before ending your work session**
   - Shares your changes with the team

4. **Communicate when making breaking changes**
   - Let teammates know before pushing major changes

5. **Use descriptive commit messages**
   - Helps teammates understand what changed

## 🤝 Contributing

To improve this script:

1. Test your changes thoroughly
2. Ensure error messages are clear and actionable
3. Maintain backward compatibility
4. Update this README if adding features

## 📝 Script Internals

Key functions:
- `sync_repository()`: Core sync logic for any git repository
- `sync_all_submodules()`: Iterates through all submodules
- `update_superrepo_references()`: Updates and commits submodule references
- `has_uncommitted_changes()`: Safety check before operations
- `is_ahead_of_remote()` / `is_behind_remote()`: Determines sync direction

## 🐛 Reporting Issues

When reporting issues, please include:
1. The full error output
2. Output of `git status` in the affected repository
3. Your git version: `git --version`
4. Operating system

## 📜 License

This script is provided as-is for team use. Modify as needed for your workflow.

---

*Remember: This script is a tool to help with git workflows, but understanding basic git concepts will help you resolve issues more effectively. When in doubt, ask your tech lead!*
