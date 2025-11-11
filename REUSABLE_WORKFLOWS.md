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

### Features

- ✅ Automatically approves qualifying Dependabot PRs
- ✅ Enables squash auto-merge for qualifying PRs
- ✅ Respects `disable-automerge` label to skip specific PRs
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