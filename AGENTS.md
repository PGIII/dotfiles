# dotfiles

**Public repo.** Everything committed here is world-readable, permanently, including git history. Treat every change as a publication.

Managed with GNU `stow` from the repo root into `~`: each file mirrors its `$HOME` path, and `~/.zshrc` etc. are symlinks back into this repo. Editing a live config edits the repo file.

## Vulnerability scan

Before every commit, scan the staged diff (`git diff --cached`) for anything that should not be public, and stop on any hit:

- Secrets: API keys, tokens, passwords, private keys, `.env` contents, auth headers.
- Identifying infrastructure: private hostnames, tailnet names, internal domains, IPs, usernames in absolute paths, service ports.
- Personal data: email addresses, health or financial data, session and history files.
- Agent config that describes private projects or environments, such as `autoMode` blocks and hooks in `.claude/settings.json`.

Also check dependencies: submodule URLs must be public HTTPS or an owned repo, and any new plugin, script, or `curl | sh` line gets read before it is added.

Secret already pushed: rotating it is the fix, because history is permanent. Tell the user first.

## Stow

- Repo-only files (`AGENTS.md`, `README.md`, `LICENSE`) are listed in `.stow-local-ignore` so they are never linked into `~`.
- `stow` aborts entirely on any conflict. Resolve conflicts by inspecting the live file first; `--adopt` overwrites repo files with the live versions, so run `git status` beforehand and review `git diff` afterward.
- Machine-specific or private config stays untracked: keep it out of stow with `.stow-local-ignore` and out of git with `.gitignore`.
