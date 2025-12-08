# Reusable Workflows

This document explains how to use the organization's reusable workflows in your repository.

## Dependabot Auto-merge

Automatically approves and merges Dependabot PRs based on configurable semver update types.

### Basic Usage

Create `.github/workflows/dependabot-automerge.yml` in your repository:

```yaml
name: Dependabot auto-merge

on: pull_request

jobs:
  dependabot-automerge:
    uses: draytechnologies/.github/.github/workflows/dependabot-automerge.yml@main
```

This uses the **default configuration**: automatically merges `patch` and `minor` version updates.

### Advanced Configuration

Customize which update types trigger auto-merge using the `update-types` input:

#### Merge all updates (patch, minor, major)

```yaml
jobs:
  dependabot-automerge:
    uses: draytechnologies/.github/.github/workflows/dependabot-automerge.yml@main
    with:
      update-types: 'patch,minor,major'
```

#### Merge only patch updates

```yaml
jobs:
  dependabot-automerge:
    uses: draytechnologies/.github/.github/workflows/dependabot-automerge.yml@main
    with:
      update-types: 'patch'
```

#### Merge only minor and major updates

```yaml
jobs:
  dependabot-automerge:
    uses: draytechnologies/.github/.github/workflows/dependabot-automerge.yml@main
    with:
      update-types: 'minor,major'
```

#### Exclude specific dependencies

Prevent auto-merge for specific dependencies (e.g., language runtimes, critical packages):

```yaml
jobs:
  dependabot-automerge:
    uses: draytechnologies/.github/.github/workflows/dependabot-automerge.yml@main
    with:
      excluded-dependencies: 'node,python,npm,go,rust'
```

This example excludes base language packages from auto-merge, even if they are patch/minor updates.

#### Allow all dependencies (no exclusions)

To disable the exclusion list entirely and restore pre-v2.0.0 behavior:

```yaml
jobs:
  dependabot-automerge:
    uses: draytechnologies/.github/.github/workflows/dependabot-automerge.yml@main
    with:
      excluded-dependencies: ''
```

**Note**: Dependency names are **case-sensitive** and must match exactly (e.g., `node` ≠ `Node`).

### Features

- ✅ Automatically approves qualifying Dependabot PRs
- ✅ Enables squash auto-merge for qualifying PRs
- ✅ Respects `disable-automerge` label to skip specific PRs
- ✅ Configurable dependency exclusion list (default: node, python, npm)
- ✅ Only acts on PRs from `dependabot[bot]`
- ✅ Configurable semver update types (patch, minor, major)

### Disabling Auto-merge for Specific PRs

Add the `disable-automerge` label to any Dependabot PR to prevent automatic merging, even if it matches your configured update types.

### Using a Specific Version

To pin to a specific release instead of `@main`:

```yaml
uses: draytechnologies/.github/.github/workflows/dependabot-automerge.yml@v1.0.0
```

Check the [releases](https://github.com/draytechnologies/.github/releases) for available versions.

## Conventional Release Labels

Automatically adds labels to pull requests based on conventional commit types. Labels are derived from PR titles or commit messages that follow the conventional commits specification.

### Basic Usage

Create `.github/workflows/conventional-labels.yml` in your repository:

```yaml
name: Conventional Release Labels

on: pull_request

jobs:
  label:
    uses: draytechnologies/.github/.github/workflows/conventional-label.yaml@main
```

### Features

- ✅ Automatically labels PRs based on conventional commit types
- ✅ Skips Dependabot PRs automatically
- ✅ Maps commit types to semantic labels
- ✅ Supports multiple commit types (feat, fix, docs, test, refactor, etc.)

### Label Mapping

The workflow maps conventional commit types to labels:

| Commit Type | Label |
|-------------|-------|
| `breaking` | `breaking-changes` |
| `build` | `dependencies` |
| `chore` | `chore` |
| `ci` | `ci` |
| `docs` | `documentation` |
| `feat` | `feature` |
| `fix` | `bug` |
| `hotfix` | `hotfix` |
| `refactor` | `refactor` |
| `test` | `test` |

Commit types `invalid` and `wontfix` are ignored.

### Using a Specific Version

To pin to a specific release instead of `@main`:

```yaml
uses: draytechnologies/.github/.github/workflows/conventional-label.yaml@v1.0.0
```

## Block Fixup Commits

Blocks merging of pull requests that contain fixup commits, ensuring clean commit history.

### Basic Usage

Create `.github/workflows/block-fixup.yml` in your repository:

```yaml
name: Block Fixup Commits

on: pull_request

jobs:
  block-fixup:
    uses: draytechnologies/.github/.github/workflows/block-fixup.yml@main
```

### Features

- ✅ Prevents merging PRs with fixup commits
- ✅ Enforces clean commit history
- ✅ Runs automatically on all pull requests

### Why Block Fixup Commits?

Fixup commits (created with `git commit --fixup`) are meant to be squashed before merging. This check ensures developers don't accidentally merge PRs with temporary fixup commits, keeping the main branch history clean and meaningful.

### Using a Specific Version

To pin to a specific release instead of `@main`:

```yaml
uses: draytechnologies/.github/.github/workflows/block-fixup.yml@v1.0.0
```

## Update Pre-commit

Automatically updates pre-commit configuration files when Dependabot creates PRs that update pre-commit hooks.

### Basic Usage

Create `.github/workflows/update-pre-commit.yml` in your repository:

```yaml
name: Update Pre-commit

on: pull_request

jobs:
  update-pre-commit:
    uses: draytechnologies/.github/.github/workflows/update-pre-commit.yml@main
```

### Features

- ✅ Only runs on Dependabot PRs
- ✅ Automatically updates `.pre-commit-config.yaml`
- ✅ Commits and pushes changes back to the PR
- ✅ Uses Python script for reliable config updates
- ✅ Requires write permissions for contents and pull requests

### Requirements

Your repository must have:
- A `scripts/update_pre_commit.py` script that handles the pre-commit config update logic

The workflow automatically installs Python 3.12 and PyYAML to run the update script.

### How It Works

1. Detects when Dependabot opens/updates a PR
2. Checks out the PR branch
3. Runs your custom `scripts/update_pre_commit.py` script
4. Commits any changes with message: "build(deps-dev): Update pre commit"
5. Pushes changes back to the PR branch

### Using a Specific Version

To pin to a specific release instead of `@main`:

```yaml
uses: draytechnologies/.github/.github/workflows/update-pre-commit.yml@v1.0.0
```