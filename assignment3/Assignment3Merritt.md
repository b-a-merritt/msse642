# Git Activities for the Week

## Configuration

### Commands to configure `user.name` and `user.email`
Git identity is either **global** (for all repositories) or **local** (for a single repository). It is generally recommended to configure these settings **globally** if you use the same name and email for all your projects. If you need a different identity for a specific repository (e.g., a work project vs. a personal project), you can override them locally.

Global configuration
```bash
# Configure globally (applies to all repos)
git config --global user.name "Ben Merritt"
git config --global user.email "me@ben-merritt.com"

# Verify the global settings
git config --global --list
```

Local configuration
```bash
# Configure locally (applies only to the current repo)
git config user.name "Ben Merritt"
git config user.email "me@ben-merritt"

# Verify the local settings
git config --local --list
```

- **Why global vs. local?**  
  - **Global** (`--global`): Use this if you want the same name/email for every repository on your machine (e.g., your personal GitHub account).  
  - **Local** (no `--global` flag): Use this if you need to use a different name/email for a specific repository (e.g., contributing to an organization with a different email).

### How to configure the core editor for Git
By default, Git uses the system’s default editor (often `vi` or `vim`). You can change this to your preferred editor (e.g., `nano`, `code`, `sublime`, etc.):

```bash
# Set globally (applies to all repos)
git config --global core.editor "nano"

# Or set locally (applies only to the current repo)
git config core.editor "nano"
```

```bash
# Verify the editor setting
git config --global core.editor
```

### How to view global and local configuration
**Global configuration:**  
```bash
git config --global --list
```

  _Sample output:_
```text
user.email=ben.a.merritt@gmail.com
user.name=Ben
pack.windowmemory=100m
pack.packsizelimit=100m
pack.threads=1
core.editor=vim
```
**Local (repository-specific) configuration:**  
```bash
git config --local --list
```

  _Sample output (inside a repo):_
```text
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
remote.origin.url=git@personal.github.com:b-a-merritt/msse642.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
```

---

## Working with a Local Repository

### Steps to create a new local repository via the CLI
1. Create a new directory (optional).
2. Initialize Git in that directory.
3. Verify that the `.git` folder exists.

```bash
# Create a new directory
mkdir my-new-repo
cd my-new-repo

# Initialize Git
git init
```

```text
Initialized empty Git repository in /path/to/my-new-repo/.git/
```

```bash
# Verify the .git folder
ls -a
```

```text
.  ..  .git
```

### How to clone a repository (and difference vs. creating a new repo)
- **Cloning** copies an existing remote repository (including all commits, branches, and history) to your local machine.
- **Creating a new repo** starts a fresh, empty Git repository with no commits or history.

```bash
# Clone a public repository (example: Git’s official repo)
git clone https://github.com/git/git.git
```

```text
Cloning into 'git'...
remote: Enumerating objects: 550580, done.
remote: Counting objects: 100% (550580/550580), done.
remote: Compressing objects: 100% (422334/422334), done.
Receiving objects: 100% (550580/550580), 217.45 MiB | 2.14 MiB/s, done.
Resolving deltas: 100% (509839/509839), done.
```

- **Difference**:  
  - **`git clone`**: Copies an existing repository (history, commits, branches).  
  - **`git init`**: Creates a brand-new repository with no history.

### How to look at the status of your repository
The `git status` command shows which branch you’re on, files staged for commit, unstaged changes, and untracked files.

```bash
git status
```

```text
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

- **Information provided by `git status`:**
  - Current branch (e.g., `main`).
  - Relationship to remote (ahead/behind).
  - Files staged for commit.
  - Files with unstaged changes.
  - Untracked files.

### How to stage changes in preparation for a commit
Use `git add` to stage specific files or all changes.

```bash
# Stage a single file
git add file1.txt

# Stage multiple files
git add file2.txt file3.txt

# Stage all modified and new files
git add .
```

```text
# (No output on successful staging)
```

### How to commit changes to your local repository
Use `git commit` with a message describing your changes.

```bash
git commit -m "Add initial project files"
```

```text
[main abc1234] Add initial project files
 3 files changed, 42 insertions(+)
 create mode 100644 file1.txt
 create mode 100644 file2.txt
```

### Example of a `.gitignore` file
A `.gitignore` file specifies intentionally untracked files that Git should ignore. Common items to ignore include:
- Build artifacts (e.g., `bin/`, `obj/`).
- Dependency directories (e.g., `node_modules/`).
- Log files (e.g., `*.log`).
- OS-specific files (e.g., `.DS_Store` on macOS).
- Editor config files (e.g., `.vscode/`, `*.suo`, `*.user`).

```gitignore
# Node.js dependencies
node_modules/

# Logs
*.log

# Build outputs
/dist
/build

# OS-specific files
.DS_Store

# IDE/editor folders
.vscode/
.idea/

# Environment variables
.env
```

- **Files that should NOT be part of version control:**
  - Credentials and secrets (`.env`, `.key`).
  - Compiled code or binaries (`.class`, `*.exe`).
  - OS-generated metadata (`Thumbs.db`, `.DS_Store`).
  - Dependency directories (`node_modules/`, `vendor/`).

### How to delete files using Git
You cannot simply use OS commands if a file is tracked by Git. Instead, use:

```bash
git rm filename.txt
```

```text
rm 'filename.txt'
```

```bash
# Then commit the deletion
git commit -m "Remove obsolete file"
```

```text
[main def5678] Remove obsolete file
 1 file changed, 0 insertions(+), 1 deletion(-)
 delete mode 100644 filename.txt
```

---

## Working with a Remote Repository

### How to view the remote repository associated with your local repository
Use `git remote -v` to list the remotes and their URLs.

```bash
git remote -v
```

```text
origin  https://github.com/username/my-repo.git (fetch)
origin  https://github.com/username/my-repo.git (push)
```

### Function of the `git fetch` command
`git fetch` downloads commits, files, and refs from a remote repository into your local repository, but it does **not** merge them into your working branch. It updates the remote-tracking branches (e.g., `origin/main`).

```bash
git fetch origin
```

```text
remote: Enumerating objects: 10, done.
remote: Counting objects: 100% (10/10), done.
remote: Compressing objects: 100% (6/6), done.
remote: Total 6 (delta 2), reused 4 (delta 0), pack-reused 0
Unpacking objects: 100% (6/6), 1.23 KiB | 1.23 MiB/s, done.
From https://github.com/username/my-repo
   abc1234..def5678  main       -> origin/main
```

### Difference between `git fetch` and `git pull`
- **`git fetch`**:  
  - Downloads objects and refs from the remote.  
  - Updates remote-tracking branches (e.g., `origin/main`).  
  - Does **not** modify your local working branch.

- **`git pull`**:  
  - Equivalent to `git fetch` followed by `git merge` (or `git rebase` if configured).  
  - Merges the fetched changes into your current branch automatically.

```bash
# Practice using fetch
git fetch origin
```

```text
# (Output as shown above)
```

```bash
# Practice using pull
git pull origin main
```

```text
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 2 (delta 1), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (2/2), 179 bytes | 179.00 KiB/s, done.
From https://github.com/username/my-repo
   def5678..ghi9012  main       -> origin/main
Updating def5678..ghi9012
Fast-forward
 README.md | 2 ++
 1 file changed, 2 insertions(+)
```

### Making changes and syncing with the remote repository
1. Modify or create a file.
2. Stage and commit the changes.
3. Push to the remote.

```bash
# Modify or create a file
echo "Some new content" >> file1.txt

# Stage the changes
git add file1.txt

# Commit locally
git commit -m "Update file1.txt with new content"
```

```text
[main jkl3456] Update file1.txt with new content
 1 file changed, 1 insertion(+)
```

```bash
# Push to the remote repository
git push origin main
```

```text
Counting objects: 3, done.
Delta compression using up to 8 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 234 bytes | 234.00 KiB/s, done.
Total 2 (delta 1), reused 0 (delta 0), pack-reused 0
To https://github.com/username/my-repo.git
   ghi9012..jkl3456  main -> main
```

---

## Branches

### How to view local and remote branches
Use `git branch -a` to list both local and remote-tracking branches.

```bash
git branch -a
```

```text
* main
  feature-old
  remotes/origin/HEAD -> origin/main
  remotes/origin/feature-x
  remotes/origin/main
```

- **Local branches** are listed without a prefix.  
- **Remote-tracking branches** appear as `remotes/origin/<branch-name>`.

### View local branches and create a new branch (before and after)
1. View local branches.
2. Create a new branch.
3. View local branches again.

```bash
# Before creating a new branch
git branch
```

```text
* main
  feature-old
```

```bash
# Create a new branch named "feature-new"
git branch feature-new
```

```bash
# After creating the new branch
git branch
```

```text
  feature-new
* main
  feature-old
```

### Different ways to switch to a new branch
- **Using `checkout`:**
  ```bash
  git checkout feature-new
  ```
  ```text
  Switched to branch 'feature-new'
  ```

- **Using `switch`:**
  ```bash
  git switch feature-new
  ```
  ```text
  Switched to branch 'feature-new'
  ```

- **Create and switch in one command (with `checkout`):**
  ```bash
  git checkout -b experimental-branch
  ```
  ```text
  Switched to a new branch 'experimental-branch'
  ```

- **Create and switch in one command (with `switch`):**
  ```bash
  git switch -c hotfix-branch
  ```
  ```text
  Switched to a new branch 'hotfix-branch'
  ```

### Delete your local branch without pushing to a remote or merging
To delete a local branch that is not merged into the current branch, use `-d` or `-D` (force delete).

```bash
# Ensure you are on a different branch (e.g., main)
git checkout main
```

```bash
# Delete "feature-old" (must be fully merged; otherwise use -D)
git branch -d feature-old
```

```text
Deleted branch feature-old (was abc1234).
```

```bash
# Verify that "feature-old" is gone
git branch
```

```text
* main
  feature-new
```

```bash
# For an unmerged branch, force-delete:
git branch -D unmerged-branch
```

```text
Deleted branch unmerged-branch (was def5678).
```

```bash
# Verify deletion
git branch
```

```text
* main
  feature-new
```