# Global Claude Instructions

## Python Package Management

Always use Python virtual environments instead of installing packages system-wide with pip.

- Before running `pip install`, create or activate a venv:
  ```bash
  python3 -m venv .venv && source .venv/bin/activate
  pip install <package>
  ```
- If a `.venv` already exists in the project directory, activate it before installing.
- Never use `pip install --break-system-packages` or `pip install` at the system level.
- Prefer `uv` if available (`uv venv` / `uv pip install`) as it is faster and safer.

## Git Workflow

Before starting any development work:

1. **Pull latest changes first**: Check if an `upstream` remote exists (`git remote -v`). If yes, pull from upstream main: `git fetch upstream && git merge upstream/main`. If no upstream, pull from origin main: `git pull origin main`.
2. **Create a new branch**: Always create and switch to a new feature branch before making changes. Never develop directly on `main`.

```bash
# Check for upstream
git remote -v

# If upstream exists:
git fetch upstream && git merge upstream/main

# If no upstream:
git pull origin main

# Then create a new branch
git checkout -b <branch-name>
```

### Commit Sign-off

Every commit must be signed off using the GitHub no-reply email — never the personal email:

```
Signed-off-by: Faradawn Yang <73060648+faradawn@users.noreply.github.com>
```

Ensure git is configured correctly before committing:

```bash
git config --global user.name "Faradawn Yang"
git config --global user.email "73060648+faradawn@users.noreply.github.com"
git config --global commit.signoff true
```

With `commit.signoff true` set, every `git commit` will automatically append the correct sign-off line.
