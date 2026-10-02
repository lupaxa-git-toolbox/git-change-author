<p align="center">
    <a href="https://github.com/lupaxa-git-toolbox">
        <img src="https://raw.githubusercontent.com/the-lupaxa-project/brand-assets/master/logos/organisations/git-toolbox/readme-logo.png" alt="Organisation Logo" />
    </a>
</p>

<h1 align="center">Git Change Author</h1>

Rewrite commit author and committer identity for one email address.

## What it Does

`git-change-author` rewrites history so commits that used an old author email use a new name and email. It updates both author and committer fields, then rewrites branches and tags.

Use it when a repository was committed under the wrong address and every matching commit should show the corrected identity.

## Install

With Homebrew:

```bash
brew tap the-lupaxa-project/tap
brew trust the-lupaxa-project/tap
brew install git-change-author
```

Or run the script from a clone: `./src/git-change-author --help`.

## Quick Start

```bash
./src/git-change-author -S \
  -o old@example.com -e new@example.com -N "New Name"   # plan only — no changes
./src/git-change-author -n -L \
  -o old@example.com -e new@example.com -N "New Name"   # dry-run, local only
./src/git-change-author -y \
  -o old@example.com -e new@example.com -N "New Name"   # rewrite, then force-push
```

**This rewrites history.** Prefer `--summary` or `-n` first.

## Default Behaviour

With the required identity flags and no mode flags, the script will:

1. Require a Git working tree (or `--git-dir` together with `--work-tree`)
2. Refuse to run if `--old-email` is not already in the history
3. Print an action plan
4. Ask you to type `CHANGE` before rewriting
5. Run `git filter-branch` so matching author and committer emails are replaced on all branches and tags
6. Force-push every branch and tag to `origin` when that remote exists

`--local-only` skips the push. `--yes` (and `--force`) skips the confirmation. `--summary` prints the plan and exits.

## Common Options

| Flag                    | Purpose                                               |
| :---------------------- | :---------------------------------------------------- |
| `-o, --old-email EMAIL` | Email address to replace (matched case-insensitively) |
| `-e, --new-email EMAIL` | Email address to write in its place                   |
| `-N, --new-name NAME`   | Name to write for the new author and committer        |
| `-r, --remote NAME`     | Remote to force-push (default: `origin`)              |
| `-L, --local-only`      | Rewrite locally and do not touch the remote           |
| `-n, --dry-run`         | Simulate commands without changing anything           |
| `-S, --summary`         | Print a detailed plan based on all options and exit   |
| `-y, --yes`             | Skip the interactive `CHANGE` confirmation            |
| `-f, --force`           | Same as `--yes`                                       |
| `-d, --git-dir PATH`    | Path to the repository `.git` directory               |
| `-w, --work-tree PATH`  | Path to the working tree (use with `--git-dir`)       |
| `-V, --version`         | Print version and exit                                |
| `-h, --help`            | Show help and exit                                    |

```bash
./src/git-change-author --help
```

## Examples

Preview what a rewrite would do:

```bash
./src/git-change-author --summary \
  --old-email emmett.brown@example.com \
  --new-email marty.mcfly@example.com \
  --new-name "Marty McFly"
```

Rewrite the current repository and leave the remote alone:

```bash
./src/git-change-author --local-only --yes \
  --old-email emmett.brown@example.com \
  --new-email marty.mcfly@example.com \
  --new-name "Marty McFly"
```

Rewrite a repository that is not the current directory, then force-push `origin`:

```bash
./src/git-change-author \
  --git-dir /path/to/repo/.git \
  --work-tree /path/to/repo \
  --old-email emmett.brown@example.com \
  --new-email marty.mcfly@example.com \
  --new-name "Marty McFly" \
  --remote origin \
  --yes
```

## Safety Notes

- `--dry-run` prints the action plan and simulates commands without asking for `CHANGE`.
- Real runs require typing `CHANGE` unless you pass `-y` / `--yes` (or `-f` / `--force`).
- `--summary` resolves the repository and prints the plan only — no rewrite is run.
- A missing old email fails before any rewrite. Commits that already use a different email are left unchanged.
- Force-push runs after a successful rewrite, unless you pass `--local-only` or the repository has no remote.
- Collaborators, forks, and clones must fetch the rewritten history and rebase local work onto it.

<a href="https://github.com/the-lupaxa-project">
    <img src="https://raw.githubusercontent.com/the-lupaxa-project/brand-assets/master/logos/components/footer-for-child-orgs.svg" alt="The Lupaxa Project Footer" width="100%" />
</a>
