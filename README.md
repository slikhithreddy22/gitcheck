# gitcheck

A simple terminal tool that scans a folder full of projects and tells you which ones are in sync with their git remote — and which ones aren't.

Built for developers who keep all their projects in one place and want a quick health check before reinstalling their OS, switching machines, or just making sure nothing important is left unpushed.

## The problem it solves

You have a `projects/` folder that looks something like this:

```
projects/
├── go/
│   ├── cli-tool/
│   └── api-server/
├── dsa/
│   ├── arrays/
│   └── graphs/
├── web/
│   └── portfolio/
└── learning/
    └── rust-basics/
```

Checking each repo one by one is tedious. `gitcheck` scans all of them at once and gives you a single table showing what needs attention.

## Demo output

```
Checking repos inside: /home/user/Documents/projects

Project                                  Branch          Status
──────────────────────────────────────────────────────────────────────
go/cli-tool                              main            ✓ up to date
go/api-server                            main            ↑ 2 to push
dsa/arrays                               main            ✓ up to date
dsa/graphs                               main            ↓ 1 to pull
web/portfolio                            main            ~ uncommitted changes
learning/rust-basics                     main            no remote configured
```

## What each status means

| Status | Meaning |
|---|---|
| `✓ up to date` | Local and remote are identical — safe to delete and restore |
| `↑ N to push` | You have commits not uploaded to GitHub yet |
| `↓ N to pull` | Remote has newer commits you don't have locally |
| `~ uncommitted changes` | Files modified or added but not even committed yet |
| `no remote configured` | Repo exists locally but was never linked to any remote |

Multiple statuses can appear together, e.g. `↑ 1 to push  ~ uncommitted changes`.

## Installation

**1. Download the script**

```bash
curl -O https://raw.githubusercontent.com/slikhithreddy22/gitcheck/main/gitcheck
```

Or clone the repo:

```bash
git clone https://github.com/slikhithreddy22/gitcheck.git
cd gitcheck
```

**2. Move it to your PATH and make it executable**

```bash
sudo mv gitcheck /usr/local/bin/gitcheck
sudo chmod +x /usr/local/bin/gitcheck
```

That's it — no dependencies, no install scripts.

## Usage

```bash
# point it at any folder
gitcheck ~/Documents/projects

# or cd there first
cd ~/Documents
gitcheck projects

# defaults to current directory if no argument given
cd ~/Documents/projects
gitcheck
```

## How it works

For each subdirectory containing a `.git` folder (at any depth):

1. Runs `git fetch` to get the latest info from the remote
2. Compares local commits vs remote commits using `git rev-list`
3. Checks for uncommitted changes using `git status --porcelain`
4. Prints a color-coded row with the result

No files are modified. It only reads — the fetch is silent and only updates remote-tracking refs locally.

## Requirements

- Bash (comes with Linux and macOS)
- Git

Works on Linux and macOS. On Windows, use WSL.

## License

MIT
