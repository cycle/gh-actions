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
[![Software License](https://img.shields.io/github/license/cycle/gh-actions.svg?style=flat-square&color=blue)](/LICENSE.md)
[![Commits since latest release](https://img.shields.io/github/commits-since/cycle/gh-actions/latest?style=flat-square)](https://github.com/cycle/gh-actions)
[![Discord](https://img.shields.io/discord/538114875570913290?style=flat-square&logo=discord&labelColor=7289d9&logoColor=white&color=39456d)](https://discord.gg/spiralphp)
[![Twitter](https://img.shields.io/badge/-Follow-black?style=flat-square&logo=X)](https://x.com/intent/follow?screen_name=SpiralPHP)

</div>

# Phive / Install

This action installs dependencies with [Phive](https://github.com/phar-io/phive), the [Phar Installer](https://phar.io), based on the specified `PHIVE_HOME` directory and a list of trusted `GPG keys`. It's designed to be flexible, allowing you to specify the `PHIVE_HOME` directory and the `GPG keys` to trust for the installation process.

<br>

## 🤔 Example Usage

Create a new workflow file, for example, `.github/workflows/integrate.yml`, and add the following code to it.

```yaml
---

on:  # yamllint disable-line rule:truthy
  push:
    branches:
      - master
  pull_request:

name: 🔍 Continuous integration

jobs:
  integrate:
    runs-on: ubuntu-latest

    steps:
      - name: 📦 Check out the codebase
        uses: actions/checkout@v4

      # ...

      - name: 📥 Install dependencies with Phive
        uses: cycle/gh-actions/actions/phive/install@master
        with:
          phive-home: '.build/phive'
          trust-gpg-keys: '0x033E5F8D801A2F8D'

      # ...

...
```

For details, see [`actions/phive/install/action.yml`](./action.yml).

Real-world examples can be found in the [`wayofdev/laravel-package-tpl`](https://github.com/wayofdev/laravel-package-tpl/blob/master/.github/workflows/integrate.yml) repository.

<br>

## 🗂️ Structure

### Inputs

- `dependencies`, optional: Which dependencies to install, one of `"lowest"`, `"locked"`, `"highest"`
- `working-directory`, optional: The working directory to use, defaults to `"."`.

### Outputs

none

### Side Effects

- When `dependencies` is set to `"lowest"`, dependencies are installed in the directory specified by `working-directory` with

  ```bash
  composer update --ansi --no-interaction --no-progress --prefer-lowest
  ````

- ```bash
  composer install --ansi --no-interaction --no-progress
  ```

- When `dependencies` is set to `"highest"`, dependencies are installed in the directory specified by `working-directory` with

  ```bash
  composer update --ansi --no-interaction --no-progress
  ````

<br>
