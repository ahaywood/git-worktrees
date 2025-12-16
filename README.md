# Get Worktrees

This repository is a template for setting up and managing git worktrees with ease.
It provides scripts and documentation to help you quickly create isolated development environments for different branches, allowing smooth multi-feature workflows.

This is based on [Nick Nisi's setup.](https://nicknisi.com/posts/git-worktrees/)


## Development Setup

This project uses git worktrees to manage multiple feature branches simultaneously. Each worktree is a separate working directory that allows you to work on different branches without switching contexts or stashing changes.

### Prerequisites

- [direnv](https://direnv.net/) - Used for project-specific environment configuration
- [pnpm](https://pnpm.io/) - Package manager
- Node.js

### Quick Start

1. Make the new-worktree.sh script executable:
```bash
chmod +x new-worktree.sh
```

2. Allow direnv to load project configuration:
```bash
direnv allow
```

3. Clone the repository as a bare repo:
```bash
bare <REPO>
```

This clones the repository as a bare repository into `.bare/`, which is required for the worktree workflow.

4. Create a new worktree for your feature branch:
```bash
worktree <branch-name>
```

This command creates a new worktree with the branch `amydutton/<branch-name>` based off `dev`, installs dependencies, and builds the project.

### Worktree Workflow

The `worktree` command is a convenience wrapper around `./new-worktree.sh` that:
- Creates a new worktree directory
- Creates and pushes a new branch (`amydutton/<branch-name>`)
- Installs dependencies with pnpm
- Runs an initial build

This script is based on [Nick Nisi's helper script](https://gist.github.com/nicknisi/a26f148611517e3d998eb456ac57efff), detailed [here](https://nicknisi.com/posts/git-worktrees/).

**Example:**
```bash
worktree feature-flags
```

This creates:
- Directory: `./feature-flags`
- Branch: `amydutton/feature-flags`
- Based on: `dev`

**Cleanup:**
```bash
worktree-remove <branch-name>
```

This removes the worktree directory, deletes the local branch `amydutton/<branch-name>`, and deletes the remote branch.

**Example:**
```bash
worktree-remove feature-flags
```

### Manual Worktree Management

If you need more control, use the script directly:

```bash
./new-worktree.sh -b <branch-name> -B <base-branch> <directory-name>
```

**Options:**
- `-b, --branch` - Branch name to create
- `-B, --base` - Base branch (default: `dev`)
- `-N, --no-create-upstream` - Skip creating remote branch
- `-h, --help` - Show help

**List all worktrees:**
```bash
git worktree list
```

## Project Structure

```
my-project/
├── feature-flags/          # Monorepo workspace
│   ├── apps/
│   │   └── frontend/      # Next.js application
│   ├── packages/          # Shared packages
│   └── tools/             # Build and lint configuration
├── bin/                   # Project scripts
├── new-worktree.sh       # Worktree creation script
└── .envrc                # direnv configuration
```
