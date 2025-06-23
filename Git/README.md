# GIT COMMANDS CHEAT SHEET

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

Here’s a **workflow-based Git tutorial** that outlines common workflows in teams and projects.

---

# GIT WORKFLOW TUTORIAL

## 1️⃣ **Feature Branching Workflow**

This is a popular workflow for developing new features or bug fixes in isolation. It ensures that the `main` (or `develop`) branch remains stable.

### Steps:

1. **Start from the main branch**

   ```bash
   git checkout main
   git pull origin main
   ```

2. **Create a feature branch**

   ```bash
   git checkout -b feature/feature-name
   ```

3. **Work on your feature**

   * Commit as you go:

     ```bash
     git add <file>
     git commit -m "Feature implementation"
     ```

4. **Push the feature branch**
   Share it with teammates.

   ```bash
   git push origin feature/feature-name
   ```

5. **Create a Pull Request (PR)**
   Create a PR on GitHub/GitLab/Bitbucket to merge your feature branch into `main` (or `develop`).

6. **Rebase before merging (Optional, recommended)**
   If there have been updates to `main`, rebase your branch.

   ```bash
   git fetch origin
   git rebase origin/main
   ```

7. **Resolve conflicts (if any)**
   If there are merge conflicts, resolve them in the conflicting files, then:

   ```bash
   git add <resolved_file>
   git rebase --continue
   ```

8. **Final commit and merge**

   * After resolving conflicts, commit the final changes, and merge into the `main` branch through the PR.

---

## 2️⃣ **Git Flow Workflow**

The **Git Flow** model provides a strict branching model for collaboration.

### Branches in Git Flow:

* **main**: Contains production-ready code.
* **develop**: Main branch for development work.
* **feature/**: For new features.
* **release/**: For preparing a new production release.
* **hotfix/**: For fixing bugs in production.

### Steps:

1. **Start from the develop branch**

   ```bash
   git checkout develop
   git pull origin develop
   ```

2. **Create a feature branch**
   For new features:

   ```bash
   git checkout -b feature/feature-name
   ```

3. **Work on the feature and commit**

   * Keep your commits focused.

   ```bash
   git add <file>
   git commit -m "Feature work"
   ```

4. **Finish the feature**
   Once finished, merge the feature into `develop`:

   ```bash
   git checkout develop
   git pull origin develop
   git merge feature/feature-name
   git push origin develop
   ```

5. **Create a release branch (optional)**
   When the development for the next release is complete:

   ```bash
   git checkout develop
   git pull origin develop
   git checkout -b release/v1.0
   ```

6. **Finalize the release**

   * Once testing and bug fixing are done, merge `release/v1.0` into `main` and `develop`.

   ```bash
   git checkout main
   git merge release/v1.0
   git push origin main
   ```

7. **Create a hotfix (if necessary)**
   If a bug is found in production, create a hotfix branch from `main`.

   ```bash
   git checkout main
   git checkout -b hotfix/1.0.1
   ```

8. **Merge the hotfix**
   After the fix is applied, merge into both `main` and `develop`:

   ```bash
   git checkout main
   git merge hotfix/1.0.1
   git push origin main

   git checkout develop
   git merge hotfix/1.0.1
   git push origin develop
   ```

---

## 3️⃣ **Forking Workflow**

This workflow is common in open-source projects where developers work on their own forks of a repository.

### Steps:

1. **Fork the repository**
   Fork the repo on GitHub/GitLab.

2. **Clone the forked repository**
   Clone your fork to your local machine:

   ```bash
   git clone https://github.com/your-username/repo-name.git
   ```

3. **Add the original repository as a remote**
   Set up the original repo as an upstream remote:

   ```bash
   git remote add upstream https://github.com/original-author/repo-name.git
   ```

4. **Sync your fork**
   Before working on a new feature, pull the latest changes from the original repository:

   ```bash
   git fetch upstream
   git checkout main
   git merge upstream/main
   ```

5. **Create a feature branch**

   ```bash
   git checkout -b feature/feature-name
   ```

6. **Work on the feature and commit**

   ```bash
   git add <file>
   git commit -m "Feature work"
   ```

7. **Push the feature to your fork**

   ```bash
   git push origin feature/feature-name
   ```

8. **Create a Pull Request (PR)**
   Go to your fork on GitHub/GitLab and create a PR to the original repository's `main` branch.

---

## 4️⃣ **Release Management Workflow**

This workflow is for handling stable releases and ensures that a release branch only includes changes that are production-ready.

### Steps:

1. **Create a release branch**
   Start from `main` to prepare the next release:

   ```bash
   git checkout main
   git pull origin main
   git checkout -b release/v1.0
   ```

2. **Prepare the release**
   Final bug fixes, testing, and updates should happen in the release branch.

3. **Merge the release into `main`**
   Once the release is ready, merge it into `main`:

   ```bash
   git checkout main
   git merge release/v1.0
   git tag v1.0
   git push origin main --tags
   ```

4. **Merge the release back into `develop`**
   It's important to also merge the release branch back into `develop` to include any changes that were made.

   ```bash
   git checkout develop
   git merge release/v1.0
   git push origin develop
   ```

---

## 5️⃣ **Hotfix Workflow**

This workflow is used to quickly patch critical bugs found in the production environment.

### Steps:

1. **Create a hotfix branch from `main`**

   ```bash
   git checkout main
   git pull origin main
   git checkout -b hotfix/1.0.1
   ```

2. **Apply the hotfix**
   Commit the necessary bug fixes:

   ```bash
   git add <file>
   git commit -m "Critical bug fix"
   ```

3. **Merge the hotfix into `main`**

   ```bash
   git checkout main
   git merge hotfix/1.0.1
   git push origin main
   ```

4. **Merge the hotfix into `develop`**
   Since development may have continued in `develop`, merge the hotfix back:

   ```bash
   git checkout develop
   git merge hotfix/1.0.1
   git push origin develop
   ```

---



