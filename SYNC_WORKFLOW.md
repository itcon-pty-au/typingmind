# Syncing Your Fork with the Upstream Repository

This document outlines the steps to keep your forked repository in sync with the original `TypingMind/typingmind` repository, while preserving your custom file structure.

## One-Time Setup: Adding the Upstream Remote

If you haven't already, you need to add the original repository as a remote. This only needs to be done once.

```bash
git remote add upstream https://github.com/TypingMind/typingmind.git
```

You can verify the remote was added correctly with:

```bash
git remote -v
```

You should see `upstream` pointing to the `TypingMind/typingmind` repository.

## Syncing Workflow

Follow these steps each time you want to pull in the latest changes from the original repository.

### 1. Fetch Latest Changes

Download the latest commits from the `upstream` (original) repository. This command doesn't change any of your local files yet.

```bash
git fetch upstream
```

### 2. Rebase Your Changes

Make sure you are on your main branch (e.g., `main` or `master`). This command will take your local commits (including your file structure changes), and re-apply them on top of the latest changes from the `upstream` repository.

```bash
git rebase upstream/main
```

This will create a clean, linear history, making it appear as though your changes were made after the latest upstream updates.

### 3. Push to Your Fork

Because rebasing rewrites your local branch's history, you cannot use a simple `git push`. You must use a force push. Using `--force-with-lease` is recommended as a safer alternative to `--force`.

```bash
git push --force-with-lease
```

This command will update your remote fork on GitHub with the newly rebased history. It's safer because it won't overwrite any work on the remote if someone else has pushed to it since your last fetch.

---

**Important Notes:**

- This process works best if your file structure changes are in a separate, dedicated commit.
- If you encounter merge conflicts during the `rebase` step, Git will pause and allow you to resolve them before continuing.
