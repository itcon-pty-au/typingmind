# GitHub Copilot Instructions

## Repository Overview

This is a fork of [TypingMind/typingmind](https://github.com/TypingMind/typingmind) maintained at [itcon-pty-au/typingmind](https://github.com/itcon-pty-au/typingmind).

**No custom code modifications are made** — this fork simply mirrors the upstream repo with a flattened directory structure.

### Remotes

| Remote | URL | Purpose |
|---|---|---|
| `origin` | https://github.com/itcon-pty-au/typingmind.git | Our fork (itcon) |
| `upstream` | https://github.com/TypingMind/typingmind.git | Original source |

---

## Syncing Upstream Changes to Our Fork

When upstream (`TypingMind/typingmind`) has new commits, follow this process:

### Background

Upstream publishes files inside a `src/` subdirectory, but our fork serves files from the **root** level. We therefore hard-reset to upstream and then flatten `src/` to root before pushing.

### Steps

```powershell
# 1. Fetch latest from upstream
git fetch upstream

# 2. Hard reset our local main to exactly match upstream
git reset --hard upstream/main

# 3. If upstream placed files inside src/, flatten it to root
#    (Check first: if src/ exists, move everything out of it)
if (Test-Path src) {
    Get-ChildItem src | ForEach-Object {
        $dest = Join-Path . $_.Name
        if (Test-Path $dest) { Remove-Item $dest -Recurse -Force }
        Move-Item $_.FullName .
    }
    Remove-Item src -Recurse -Force
}

# 4. Stage all changes
git add -A

# 5. Commit
git commit -m "Sync with upstream: flatten src/ to root"

# 6. Force-push to our fork (force is required due to diverged history)
git push origin main --force
```

### Why force-push?

Because we hard-reset our branch to upstream and then add a flattening commit, our history diverges from `origin/main`. A force-push is intentional and expected here.

### Notes

- Never merge upstream directly — the `src/` restructure causes hundreds of file-location conflicts.
- There are **no custom modifications** to preserve; always use `upstream/main` as the source of truth.
