# Git & GitHub Practice Lab

A small, safe project for practising the Git commands you will use most often.

## Important: Git vs GitHub

- **Git** is the version-control software. It can track changes locally without an internet connection or a GitHub account.
- **GitHub** is an online hosting and collaboration service for Git repositories.
- You can use Git without GitHub. If you do, you still get local commits, branches, diffs, history, and the ability to recover many changes.
- Without a remote host such as GitHub, you miss easy off-device backup, a shareable repository URL, pull requests and code reviews, hosted issue tracking, and convenient collaboration. You can still collaborate using other remote Git hosts or self-hosted servers.

## 1. Set up

Open a terminal in this folder.

```bash
git --version
git init
git status
git add README.md src/app.txt notes/ideas.txt .gitignore
git commit -m "Initial practice lab"
```

If Git asks who you are, configure your identity:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Use an email you are comfortable associating with your commits. You can omit `--global` to configure only this repository.

## 2. Daily workflow

Make a small edit to `src/app.txt`, then inspect and save it:

```bash
git status
git diff
git add src/app.txt
git diff --staged
git commit -m "Update app notes"
git log --oneline
```

Remember:
- `git diff` shows unstaged changes.
- `git diff --staged` shows staged changes.
- `git add` stages a snapshot of a file's current contents.
- `git commit` saves a snapshot in local history.

## 3. Practise branches

```bash
git switch -c feature/greeting
```

Edit `src/app.txt`, then:

```bash
git status
git diff
git add src/app.txt
git commit -m "Add greeting feature"
git switch main
```

If your default branch is named `master`, use `git branch` to see your branch names and substitute `master` for `main` in this practice.

Merge the feature branch:

```bash
git merge feature/greeting
git branch
```

After you are done with a merged practice branch:

```bash
git branch -d feature/greeting
```

## 4. Practise a merge conflict (intentionally)

1. Make sure you have a clean working tree: `git status`.
2. Create a branch: `git switch -c feature/conflict-demo`.
3. Edit the same line in `src/conflict.txt` and commit it.
4. Switch back to your main branch (`git switch main` or `git switch master`).
5. Edit that exact same line differently and commit it.
6. Merge the feature branch into your main branch.

```bash
git merge feature/conflict-demo
git status
```

Git may report a conflict and add markers like:

```text
<<<<<<< HEAD
Your current branch's text
=======
Incoming branch's text
>>>>>>> feature/conflict-demo
```

Edit the file into the correct final version and remove all conflict markers. Then:

```bash
git add src/conflict.txt
git commit
git status
```

To cancel an in-progress merge before resolving it, use `git merge --abort` when Git supports it and the working tree is in a state where it can safely abort.

## 5. Undo commands — practise carefully

First create a disposable commit:

```bash
# Edit src/app.txt
git add src/app.txt
git commit -m "Temporary practice commit"
```

Inspect history:

```bash
git log --oneline
```

### Unstage a file but keep edits

```bash
git restore --staged src/app.txt
```

Older equivalent for many cases:

```bash
git reset HEAD -- src/app.txt
```

### Discard unstaged edits to a tracked file

```bash
git restore src/app.txt
```

**Warning:** this discards the file's unstaged edits.

### Undo the latest local commit but keep changes staged

```bash
git reset --soft HEAD~1
```

### Undo the latest local commit and keep edits unstaged

```bash
git reset --mixed HEAD~1
```

### Remove the latest commit and discard tracked-file changes

```bash
git reset --hard HEAD~1
```

**Danger:** `--hard` can permanently discard uncommitted work. Practise only in this lab and only after checking `git status`.

### Reverse a commit without rewriting shared history

```bash
git revert HEAD
```

This creates a new commit that reverses the selected commit. Prefer this for published/shared history in ordinary team workflows.

## 6. .gitignore practice

The `.gitignore` file in this lab demonstrates ignoring local files such as `.env`, logs, and dependency folders.

```bash
git status
git check-ignore -v .env
```

Create a test `.env` file if you want, but never put real secrets in it. `.gitignore` does not untrack files already committed.

## 7. Connect to GitHub (optional)

Create an **empty repository** on GitHub. If this lab already has commits, do not add a README or license on GitHub during creation, to avoid an unrelated initial-history merge.

Then add the repository URL shown by GitHub:

```bash
git remote add origin https://github.com/YOUR-USERNAME/YOUR-REPO.git
git remote -v
git branch -M main
git push -u origin main
```

Replace the placeholder URL with your actual repository URL. GitHub may ask you to authenticate through a browser or credential manager; your GitHub password is not normally used directly for Git HTTPS authentication.

After a future commit:

```bash
git push
```

To download a remote repository onto another computer, use `git clone REPOSITORY-URL`. To fetch and integrate remote updates, use `git pull` (which fetches and then integrates according to your Git configuration).

## 8. Essential command reference

| Goal | Command |
|---|---|
| Check Git version | `git --version` |
| Current repository state | `git status` |
| Start a repository | `git init` |
| Stage a file | `git add FILE` |
| Commit staged changes | `git commit -m "message"` |
| Show unstaged changes | `git diff` |
| Show staged changes | `git diff --staged` |
| List commits | `git log --oneline` |
| List branches | `git branch` |
| Create and switch branch | `git switch -c NAME` |
| Switch branch | `git switch NAME` |
| Merge branch into current branch | `git merge NAME` |
| Show remotes | `git remote -v` |
| Send commits to remote | `git push` |
| Get and integrate remote changes | `git pull` |
| Copy remote repo locally | `git clone URL` |
| Unstage a file | `git restore --staged FILE` |
| Discard unstaged tracked-file edits | `git restore FILE` |
| Safely reverse a commit with a new commit | `git revert COMMIT` |
| Inspect a commit | `git show COMMIT` |
| Find who changed each line | `git blame FILE` |

## 9. Checkpoint

Try to do these without copying commands from the reference:

- [ ] Make a commit and explain what was saved.
- [ ] Create a feature branch and merge it.
- [ ] Cause and resolve a merge conflict.
- [ ] Explain `git diff` vs `git diff --staged`.
- [ ] Unstage a file without deleting edits.
- [ ] Explain why `git revert` is often safer than resetting shared history.
- [ ] Add a GitHub remote and push the repository.
- [ ] Explain what still works if you remove the GitHub remote.

**Rule:** Before using a destructive command, run `git status` and make sure you understand what it will discard.
