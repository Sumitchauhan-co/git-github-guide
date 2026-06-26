# Complete Git & GitHub Professional Guide

---

## 1. Git vs GitHub

### Git

- Distributed version control system.
- Tracks code changes locally.
- Enables branching, merging, history tracking.

### GitHub

- Cloud hosting platform for Git repositories.
- Provides collaboration tools like Pull Requests, Issues, Actions.

---

## 2. Basic Setup

> ### Check installation

```
git --version
```

> ### Configure identity

```
 git config --global user.name "YourName"
 git config--global user.email "you@email.com"
```

> ### View configuration

```
git config --list
```

---

## 3. Repository Initialization

### Create new repository

```
git init
```

### Clone existing repository

```
git clone https://github.com/user/project.git
```

---

## 4. Core Workflow

Working Directory → Staging Area → Repository

### Check status

```
git status
```

### Add files

```
git add file.txt
git add .
```

### Commit

```
git commit -m "message"
```

---

## 5. Viewing History

```
git log
git log --oneline
git diff
git diff --staged
```

---

## 6. Branching

### Create branch

```
git branch feature-login
```

### Switch branch

```
git checkout feature-login
git switch feature-login
```

### Create + switch

```
git checkout -b feature-login
```

### List branches

```
git branch
```

### Delete branch

```
git branch -d feature-login
```

---

## 7. Merge Workflow

### Merge branch into main

```
git checkout main
git merge feature-login
```

---

## <b> 8. Remote Operations </b>

### Add remote

```
git remote add origin <url>
```

### Check remotes

```
git remote -v
```

### Push

```
git push origin main
```

### First push

```
git push -u origin main
```

### Pull updates

```
git pull
```

### Fetch only

```
git fetch
```

---

- ## <ins>`Pull vs Rebase`</ins>

### Default

git pull = fetch + merge

`creates merge commit`

```
git pull
```

### Cleaner approach

`No merge commits`

```
git pull --rebase
```

> Make rebase default

```
git config --global pull.rebase true
```

### Manual workflow

```
git fetch origin
git rebase origin/main
```

### Rebase commands

```
git rebase --continue
git rebase --abort
git rebase --skip
```

---

## 9. <ins>`Reset vs Revert`</ins>

- ## Reset (moves HEAD)

### `Soft`

Keep changes staged

```
git reset --soft HEAD\~1
```

### Mixed

Keep changes, but unstage them

```
git reset HEAD\~1
```

### `Hard` 💀

Destroy changes

```
git reset --hard HEAD\~1
```

- ## Revert (safe undo)

creates a new commit that reverses the changes of a previous commit

```
git revert <commit-id>
```

- ## Force remove 💀

removes the commit from remote, but can disrupt team collaboration.

### To remove the recent commit

```
git reset --hard HEAD~1
git push --force
```

```
git reset --hard <commit-hash>
git push --force
```

---

- ## <ins>staged</ins>

### Unstaging Files (Before Commit)

removes a file from the staging area but keeps the changes

```
git reset HEAD <file-name>
```

## 11. Stashing

### Save temporary work

```
git stash
git stash list
git stash pop
```

---

## <b> 12. GitHub Collaboration Terms</b>

### <ins>Fork</ins>

Copy of repository

### <ins>Pull Request (PR)</ins>

Request to merge changes into base branch. Includes code review process

### <ins>Merge strategies</ins>

Merge commit | Squash merge | Rebase merge

### <ins>Issues</ins>

Track bugs/tasks

### <ins>Actions</ins>

Automation/CI/CD

---

## <b>13. Open Source Fork Synchronization (Upstream)</b>

When contributing to open source, origin points to your personal fork, while upstream points to the main project repository.

### Configure Upstream Remotes

```bash
# Add the original project repository as upstream
git remote add upstream https://github.com/original-owner/project.git

# Verify that both origin and upstream are configured
git remote -v
```

### Sync Fork Workflow

Keep your fork up-to-date with the original repository before starting new work.

```bash
# 1. Download the latest changes from the original repository
git fetch upstream

# 2. Merge those changes into your local main branch
git checkout main
git merge upstream/main

# 3. Update your personal GitHub fork (normal push, no force needed)
git push origin main
```

---

## <b>14. Typical Developer Workflow</b>

```bash
git checkout main
git pull --rebase
git checkout -b feature-auth
// (make changes)
git add .
git commit -m "add auth feature"
git push origin feature-auth
```

- ### Then open Pull Request.

---

## 15. Conflict Resolution

### Resolve conflict markers manually

\<\<\<\<\<\< HEAD code ====== other code \>\>\>\>\>\>\>

### Then

```bash
git add .
git commit
```

---

## <b>16. Useful Advanced Commands</b>

### Graph view

```bash
git log --graph --oneline --all
```

### Rename branch

```bash
git branch -m newname
```

### Delete remote branch

```bash
git push origin --delete branchname
```

### <b>Amend</b>

### <ins>Rewriting the Last Commit</ins>

Perfect for immediate fixes like typos or forgotten files.

```bash
git commit --amend
```

- ### Change message only

```bash
git commit --amend -m "New and improved commit message"
```

- ### Add files without changing the message:

```bash
git add <file>
git commit --amend --no-edit
```

###

### <ins>Rewriting Older History</ins>

When you need to fix commits that are 2 or more steps back, use an Interactive Rebase.

> ```bash
> # 1. Set VS Code as your default editor (optional)
> git config --global core.editor "code --wait"
> ```

```bash
# 2. Start rebase (replace N with the number of commits to look back)
git rebase -i HEAD~N
```

- reword (or r) : Use this if you only want to change the commit message.

- squash (or s) : Use this to merge a commit into the one above it.

### Syncing with github

```bash
git push --force-with-lease
```

`It only allow the push to proceed if the remote branch is in the same state as your last local git fetch`

### Clean untracked files

```bash
# Force delete untracked files
git clean -f

# Force delete untracked files AND directories
git clean -fd
```

---

## <b>17. .gitignore</b>

### Add .gitignore file

```bash
npx gitignore node
```

---

## <b>18. SSH vs HTTPS</b>

HTTPS : Username/password authentication.

SSH : Key-based authentication, more convenient.

### <ins>To set ssh key</ins>

> Key_name : `id_ed25519_github_work`

### Generate the SSH Key

```bash
ssh-keygen -t ed25519 -C "<User_email>" -f ~/.ssh/<Key_name>
```

### Start the agent in the background

```bash
eval "$(ssh-agent -s)"
```

### Add your private key to the agent

```bash
ssh-add ~/.ssh/<KEY_NAME>
```

### View the key to copy this to GitHub account settings

```bash
cat ~/.ssh/<KEY_NAME>.pub
```

### Verify connection to github (`optional`)

```bash
ssh-keyscan -t ed25519 github.com >> ~/.ssh/known_hosts
```

### Create SSH host alias (optional)

```bash
cat >> ~/.ssh/config <<EOF
Host <ALIAS_NAME>
HostName <SERVICE_DOMAIN>
User <USERNAME>
IdentityFile ~/.ssh/<KEY_FILENAME>
IdentitiesOnly yes
EOF

# Set required strict permissions

chmod 600 ~/.ssh/config
```

### test & push

```bash
ssh -T git@github-work

git remote set-url origin git@github-work:<Github_id>/<Repo_name>.git

git push -u origin main
```

- ### if dont have alias name on ssh key

```bash
git remote -v

git remote set-url origin git@github:<Github_id>/<Repo_name>
```

- ### if already have alias name

```bash
git remote -v

git remote set-url origin git@github-<alias>:<Github_id>/<Repo_name>.git
```

---

## 19. Best Practices

- Use feature branches.
- Avoid committing directly to main.
- Prefer rebase for clean history.
- Pull before push.
- Use revert instead of reset on shared branches.

---
