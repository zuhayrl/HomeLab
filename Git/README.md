Here’s a **comprehensive Git tutorial** that covers everything from everyday commands to advanced and less commonly used tools like `blame`, interactive rebasing, commit history manipulation, reflog, and more.

---

# 🧠 GIT COMMANDS CHEAT SHEET (No-Fluff Full Tutorial)

---

## 📁 BASIC SETUP

```bash
git init               # Initialize repo
git clone <url>        # Clone repo
git config --global user.name "Zuhayr"
git config --global user.email "you@example.com"
```

---

## 📄 STAGING & COMMITTING

```bash
git add <file>         # Stage file
git add .              # Stage all
git commit -m "msg"    # Commit
git commit -am "msg"   # Add & commit tracked files
```

---

## 🔍 STATUS, DIFF & LOG

```bash
git status             # Show staged & unstaged
git diff               # Show unstaged changes
git diff --cached      # Show staged changes
git log                # Show commit history
git log --oneline      # Compact log
git log -p             # Show changes in each commit
git log --stat         # File-level stats per commit
git show <commit>      # Show commit + diff
```

---

## 🔙 UNDOING CHANGES

```bash
git restore <file>             # Undo uncommitted changes
git restore --staged <file>    # Unstage file
git reset --hard               # Discard all changes
git checkout -- <file>         # Old method to discard file changes

# Revert a specific commit (preserves history)
git revert <commit_hash>

# Reset to previous commit
git reset --soft HEAD~1        # Keep changes staged
git reset --mixed HEAD~1       # Keep changes in working dir
git reset --hard HEAD~1        # Discard everything
```

---

## 🧭 NAVIGATION & HISTORY

```bash
git checkout <commit>          # Checkout older commit (detached HEAD)
git checkout main              # Return to main branch

# Explore file from past commit
git checkout <commit> -- <file>

# List all branches
git branch
git branch -r                  # Remote only

# List all tags
git tag
```

---

## 🧾 SEE WHO EDITED WHAT

```bash
git blame <file>               # Line-by-line author/date
git blame -L 5,10 <file>       # Blame specific lines
git log -L :function_name:<file>  # Track changes to a function
```

---

## 🔎 SEARCHING & INVESTIGATION

```bash
git grep "search_term"             # Search in codebase
git log -S'snippet' <file>         # Search commits where snippet added/removed
git log -G'regex' <file>           # Regex search in diffs
```

---

## 🛠️ BRANCHING & MERGING

```bash
git branch new-feature       # Create branch
git checkout new-feature     # Switch to branch
git switch -c new-feature    # Shortcut for above
git merge <branch>           # Merge into current
git merge --no-ff <branch>   # Force merge commit
git branch -d <branch>       # Delete branch (safe)
git branch -D <branch>       # Force delete
```

---

## 🧹 CLEANUP

```bash
git clean -n                 # Show what would be deleted
git clean -f                 # Delete untracked files
git clean -fd                # Delete untracked files + dirs
```

---

## 🔀 STASHING

```bash
git stash                    # Save work-in-progress
git stash list               # Show stashes
git stash pop                # Apply and delete top stash
git stash apply              # Apply without deleting
git stash drop               # Delete specific stash
```

---

## ⏪ REWRITING HISTORY

> Use with caution. Only for local/unshared branches.

```bash
git commit --amend           # Edit last commit
git rebase -i HEAD~3         # Interactive rebase last 3 commits
git rebase --continue        # After conflict resolution
git rebase --abort           # Cancel rebase
```

---

## 🌐 REMOTES

```bash
git remote add origin <url>     # Add remote
git remote -v                   # List remotes
git push origin main            # Push to remote
git pull origin main            # Pull from remote
git fetch origin                # Download latest refs

# Set upstream branch
git push -u origin main
```

---

## 🧭 REFS & REVISIONS

```bash
git reflog                     # Show history of HEAD changes
git checkout HEAD@{3}          # Go to state from 3 moves ago
```

---

## 🔧 TAGGING

```bash
git tag v1.0                   # Lightweight tag
git tag -a v1.0 -m "version"   # Annotated tag
git push origin v1.0           # Push tag
git push origin --tags         # Push all tags
```

---

## 🔐 SUBMODULES

```bash
git submodule add <url> <path>
git submodule update --init --recursive
```

---

## 💥 CONFLICT RESOLUTION

```bash
# After merge/rebase conflict
git status                    # See conflicting files
# Edit files to resolve
git add <resolved_file>       # Mark as resolved
git merge --continue          # or `git rebase --continue`
```

---

## 🧪 ALIASES (Optional)

```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.cm 'commit -m'
```

