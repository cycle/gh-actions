<div align="center">
    <br>
    <a href="https://cycle-orm.dev" target="_blank">
        <picture>
            <source media="(prefers-color-scheme: dark)" srcset="https://github.com/cycle/.github/blob/main/logo/words-vector-dark.svg?raw=true">
            <img width="50%" align="center" src="https://github.com/cycle/.github/blob/main/logo/words-vector-light.svg?raw=true" alt="CycleORM Logo">
        </picture>
    </a>
    <br>
    <br>
</div>

<div align="center">

[![Build Status](https://img.shields.io/endpoint.svg?url=https%3A%2F%2Factions-badge.atrox.dev%2Fcycle%2Fgh-actions%2Fbadge&style=flat-square)](https://github.com/cycle/gh-actions/actions)
[![Software License](https://img.shields.io/github/license/cycle/gh-actions.svg?style=flat-square&color=blue)](LICENSE.md)
[![Commits since latest release](https://img.shields.io/github/commits-since/cycle/gh-actions/latest?style=flat-square)](https://github.com/cycle/gh-actions)
[![Discord](https://img.shields.io/discord/538114875570913290?style=flat-square&logo=discord&labelColor=7289d9&logoColor=white&color=39456d)](https://discord.gg/spiralphp)
[![Twitter](https://img.shields.io/badge/-Follow-black?style=flat-square&logo=X)](https://x.com/intent/follow?screen_name=SpiralPHP)

</div>

# Shared GitHub Actions

This repository is a collection of [reusable workflows](https://docs.github.com/en/actions/using-workflows/reusing-workflows) and [composite actions](https://docs.github.com/en/actions/creating-actions/creating-a-composite-action), specifically designed for use in [cycle](https://github.com/cycle) projects.

These tools encapsulate common and repetitive tasks, allowing for easy integration into multiple projects. This approach not only reduces the need to rewrite code but also ensures standardized operations across all Cycle repositories.

<br>

## 📋 Table of Contents

- [Getting Started](#-getting-started)
- [Composite Actions](#-composite-actions)
- [Workflows](#-workflows)
  - [Auto Label and Release Management](#-auto-label-and-release-management)
  - [Code Architecture](#-code-architecture)
  - [Static Analysis](#-static-analysis)
- [License](#-license)
- [Security Policy](#-security-policy)
- [Contributing](#-want-to-contribute)
- [Social Links](#-social-links)
- [Useful Resources](#-useful-resources)
- [Contributors](#-contributors)

<br>

## 🚀 Getting Started

To use these workflows and actions, reference them directly from your project's workflows. Detailed instructions for each are provided below.

<br>

## 💻 Composite Actions

Composite Actions are a powerful feature of GitHub Actions that allow you to create reusable actions using a combination of other actions, shell commands, or both.

This enables you to encapsulate a sequence of steps into a single action, making your workflows more modular, easier to maintain, and reducing duplication across your projects.

Composite Actions can accept inputs and use outputs, making them highly flexible and adaptable to various use cases.

Check each action's README file for detailed instructions on how to use it.

| **Action**                                                                         | **Description**                                                                                    |
|------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| [`composer/get-cache-directory`](./actions/composer/get-cache-directory/README.md) | Gets the Composer cache directory path and exports it as env variable.                             |
| [`composer/get-root-version`](./actions/composer/get-root-version/README.md)       | determines the Composer root version based on the specified branch and exports it as env variable. |
| [`composer/install`](./actions/composer/install/README.md)                         | Installs dependencies with Composer based on the specified dependency level.                       |
| [`phive/install`](./actions/phive/install/README.md)                               | Install dependencies with [Phive](https://phar.io).                                                |

<br>

## 💻 Workflows

Read more about [reusing workflows](https://docs.github.com/en/actions/using-workflows/reusing-workflows).

### → Auto Label and Release Management

#### `apply-labels.yml:`

Automatically applies labels to pull requests based on modified paths.

This workflow triages pull requests and applies labels based on the paths that are modified in the pull request. This can help to categorize your pull requests and make it easier to identify the type of changes included.

To use this workflow, set up a `.github/labeler.yml` file with your configuration in your project. For more information on how to configure the labeler, see: <https://github.com/actions/labeler/blob/master/README.md>

Here is an example of how to use this workflow:

<details>
<summary><code>.github/workflows/apply-labels.yml</code></summary>

```yaml
---

on:
  pull_request:

name: 🏷️ Add labels

jobs:
  label:
    uses: cycle/gh-actions/.github/workflows/apply-labels.yml@master
    with:
      os: ubuntu-latest
    secrets:
      token: ${{ secrets.GITHUB_TOKEN }}

...
```

</details>
<details>
<summary><code>.github/labeler.yml</code></summary>

```yaml
---

"type: bug":
    - head-branch: ['^bug', '^fix', 'bug', 'fix']

"type: enhancement":
    - head-branch: ['^feature', '^feat', 'feature']

"type: documentation":
    - changed-files:
          - any-glob-to-any-file: ['assets/**/*', '.github/*', './*.md']

"type: maintenance":
    - changed-files:
          - any-glob-to-any-file: ['tests/**/*', '.github/workflows/*']

...
```

</details>

Real-world examples can be found in the [`wayofdev/laravel-package-tpl`](https://github.com/wayofdev/laravel-package-tpl/blob/master/.github/workflows/apply-labels.yml) repository.

<br>

#### `auto-merge-release.yml:`

This workflow automatically merges releases. This workflow utilizes [peter-evans/enable-pull-request-automerge](https://github.com/peter-evans/enable-pull-request-automerge) to auto-merge releases that are created by [googleapis/release-please](https://github.com/googleapis/release-please).

Here is an example of how to use this workflow:

<details>
<summary><code>.github/workflows/auto-merge-release.yml</code></summary>

```yaml
---

on:  # yamllint disable-line rule:truthy
  pull_request:

permissions:
  pull-requests: write
  contents: write

name: 🤞 Auto merge release

jobs:
  auto-merge:
    uses: cycle/gh-actions/.github/workflows/auto-merge-release.yml@master
    with:
      os: ubuntu-latest
      pull-request-number: ${{ github.event.pull_request.number }}
      actor: lotyp
      merge-method: merge
    secrets:
      # to trigger other workflows, pass PAT token instead of GITHUB_TOKEN
      token: ${{ secrets.PERSONAL_GITHUB_TOKEN }}

...
```

</details>

Real-world examples can be found in the [`wayofdev/laravel-package-tpl`](https://github.com/wayofdev/laravel-package-tpl/blob/master/.github/workflows/auto-merge-release.yml) repository.

<br>

### → Code Architecture

#### `create-arch-diagram.yml:`

This workflow leverages the [codesee-io/codesee-action](https://github.com/Codesee-io/codesee-action) action to automatically generate architecture diagrams for your codebase whenever a pull request is made.

CodeSee is an open-source tool that helps visualize your codebase and its dependencies, making it easier for new contributors to understand the project or for maintaining a clear view of your project's architecture over time.

Here is an example of how to use this workflow:

<details>
<summary><code>.github/workflows/create-arch-diagram.yml</code></summary>

```yaml
---

on:  # yamllint disable-line rule:truthy
  push:
    branches:
      - develop
  pull_request_target:
    types:
      - opened
      - synchronize
      - reopened

name: 🤖 CodeSee

permissions: read-all

jobs:
  codesee:
    uses: cycle/gh-actions/.github/workflows/create-arch-diagram.yml@master
    with:
      os: ubuntu-latest
      continue-on-error: true
    secrets:
      codesee-token: ${{ secrets.CODESEE_ARCH_DIAG_API_TOKEN }}

...
```

</details>

Real-world examples can be found in the [`wayofdev/laravel-package-tpl`](https://github.com/wayofdev/laravel-package-tpl/blob/master/.github/workflows/create-arch-diagram.yml) repository.

<br>

### → Static Analysis

#### `shellcheck.yml:`

This workflow uses [redhat-plumbers-in-action/differential-shellcheck](https://github.com/redhat-plumbers-in-action/differential-shellcheck) to run shell script analysis.

Here is an example of how to use this workflow:

<details>
<summary><code>.github/workflows/shellcheck.yml</code></summary>

```yaml
---

on:  # yamllint disable-line rule:truthy
  pull_request:

name: 🐞 Differential shell-check

permissions:
  contents: read

jobs:
  shellcheck:
    uses: cycle/gh-actions/.github/workflows/shellcheck.yml@master
    with:
      os: ubuntu-latest
      severity: warning
    secrets:
      token: ${{ secrets.GITHUB_TOKEN }}

...
```

</details>

Real-world examples can be found in the [`wayofdev/laravel-package-tpl`](https://github.com/wayofdev/laravel-package-tpl/blob/master/.github/workflows/shellcheck.yml) repository.

<br>

## 🔒 Security Policy

This project has a [security policy](.github/SECURITY.md).

<br>

## 🙌 Want to Contribute?

Thank you for considering contributing to the cycle community! We are open to all kinds of contributions. If you want to:

- 🤔 [Suggest a feature](https://github.com/cycle/gh-actions/issues/new?assignees=&labels=type%3A+enhancement&projects=&template=2-feature-request.yml&title=%5BFeature%5D%3A+)
- 🐛 [Report an issue](https://github.com/cycle/gh-actions/issues/new?assignees=&labels=type%3A+documentation%2Ctype%3A+maintenance&projects=&template=1-bug-report.yml&title=%5BBug%5D%3A+)
- 📖 [Improve documentation](https://github.com/cycle/gh-actions/issues/new?assignees=&labels=type%3A+documentation%2Ctype%3A+maintenance&projects=&template=4-docs-bug-report.yml&title=%5BDocs%5D%3A+)
- 👨‍💻 Contribute to the code

You are more than welcome. Before contributing, kindly check our [contribution guidelines](.github/CONTRIBUTING.md).

[![Conventional Commits](https://img.shields.io/badge/Conventional%20Commits-1.0.0-yellow.svg?style=for-the-badge)](https://conventionalcommits.org)

<br>

## 🌐 Social Links

- **Twitter (X):** Follow our organization [@SpiralPHP](https://x.com/intent/follow?screen_name=SpiralPHP).
- **Discord:** Join our community on [Discord](https://discord.gg/spiralphp).

<br>

## 🫡 Contributors

<a href="https://github.com/cycle/gh-actions/graphs/contributors" target="_blank"><img src="https://img.shields.io/github/contributors-anon/cycle/gh-actions?style=for-the-badge" alt="Contributors Badge"/></a>

<br>

## 🧱 Useful Resources

- [Composite Actions vs Reusable Workflows: what is the difference?](https://dev.to/n3wt0n/composite-actions-vs-reusable-workflows-what-is-the-difference-github-actions-11kd)

- [wayofdev/gh-actions](https://github.com/wayofdev/gh-actions) — Upstream repository for the shared GitHub Actions.

- [ergebnis/.github](https://github.com/ergebnis/.github) — Shareable actions of the [@ergebnis](https://github.com/ergebnis) organization.

- [skills/reusable-workflows](https://github.com/skills/reusable-workflows) — Reusable workflow examples

<br>

## 🤝 License

[![Licence](https://img.shields.io/github/license/cycle/gh-actions?style=for-the-badge&color=blue)](./LICENSE.md)

<br>
