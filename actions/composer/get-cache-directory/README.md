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

# Composer / Get Cache Directory Action

This action determines the Composer cache directory and exports it as `COMPOSER_CACHE_DIR` environment variable. It allows you to specify the working directory for the Composer command to determine the cache directory.

<br>

## 🤔 Example Usage

Create a new workflow file, for example, `.github/workflows/integrate.yml`, and add the following code to it.

```yaml
---

on:
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

      - name: 🎯 Get Composer Cache Directory
        uses: cycle/gh-actions/actions/composer/get-cache-directory@master
        with:
          working-directory: '.'

      # ...

...
```

For details, see [`actions/composer/get-cache-directory/action.yml`](./action.yml) file.

Real-world examples can be found in the [`wayofdev/laravel-package-tpl`](https://github.com/wayofdev/laravel-package-tpl/blob/master/.github/workflows/integrate.yml) repository.

<br>

## 🗂️ Structure

### Inputs

- `working-directory`, optional: The working directory to use. Defaults to `"."`.

### Outputs

none

### Side Effects

- The `COMPOSER_CACHE_DIR` environment variable contains the path to the composer cache directory.

<br>
