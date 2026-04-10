# Global Claude Instructions

## Python Package Management

Always prefer virtual environments over system-wide pip installs.

## Git Workflow

Always pull latest changes and create a new feature branch before starting work. Never develop directly on `main`.

### Commit Sign-off

Always sign off commits using the GitHub no-reply email, with git configured as:

```
git config --global user.name "Faradawn Yang"
git config --global user.email "73060648+faradawn@users.noreply.github.com"
git config --global commit.signoff true
```

### Pull Request Style

PR descriptions should be a single short paragraph — plain prose, no bullet points, no headers. Write in a direct, human tone that states what changed and why in one breath. No test plan section.

Good example: "Replace the legacy TRT engine backend workflow with the modern LLM API / PyTorch backend."
Bad example: A multi-section PR with a "## Summary" bullet list and a "## Test plan" checklist.
