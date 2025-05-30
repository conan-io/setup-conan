# Setup Conan Action Github Action

[![Validate Conan Action](https://github.com/conan-io/setup-conan/actions/workflows/ci.yml/badge.svg)](https://github.com/conan-io/setup-conan/actions/workflows/ci.yml)
[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-Setup%20Conan%20Client-blue.svg?colorA=24292e&colorB=0366d6&style=flat&longCache=true&logo=github)](https://github.com/marketplace/actions/setup-conan-client)


A GitHub Action to install and configure the Conan package manager in your workflow.
This action provides a simple and flexible way to set up Conan with custom configurations and audit settings.

## Features

- 🚀 Install any version of Conan 2.x
- ⚙️ Apply custom Conan configurations
- 🔐 Configure audit tokens for dependencies vulnerabilities scanning
- 🗂️ Cache Conan packages using GitHub cache
- 🔍 Support for custom Conan home
- 💪 Cross-platform support

## Usage

### Basic Usage

Install Conan, authenticate to Audit server, install custom configurations:

```yaml
steps:
  - name: Checkout code
    uses: actions/checkout@v4

  - name: Install Conan
    uses: conan-io/setup-conan@v1
    with:
      audit_token: ${{ secrets.CONAN_AUDIT_TOKEN }}
      config_urls: |
        https://github.com/<org>/conan-config.git
        https://myrepo.com/conan-config.git

  - name: Install Conan dependencies
    run: conan install . --build=missing
```

### Using Lockfiles

In order to ensure repeatability, the use of [lockfiles](https://docs.conan.io/2/tutorial/versioning/lockfiles.html#tutorial-versioning-lockfiles) on the consumer side is greatly encouraged:

```yaml
  - name: Install Conan dependencies with lockfile
    run: conan install . --lockfile=conan.lock --lockfile-partial --lockfile-out=conan.lock --build=missing
```

Lockfiles ensure that Conan will resolve the same graph in a repeatable and consistent manner - thus making sure the same versions are used across multiple systems (CI, developers, etc).

Lockfiles are strict by default, that means that if there is some requires and it cannot find a matching locked reference in the lockfile, it will error and stop. For cases where it is expected that the lockfile will not be complete, as there might be new dependencies, the `--lockfile-partial` argument can be used.

By default, conan install will not generate an output lockfile, but if the `--lockfile-out` argument is provided, pointing to a filename, then a lockfile will be generated from the current dependency graph. The same lockfile can be cached or stored in the repository, so that it can be used in the future.

## Options

This Github Action offers options inputs to execute extra steps just after installing Conan.
This is useful for installing custom configurations or applying any other setup you need.
It's possible to customize the action using the following options:

| Option           | Type    | Description                                                                                      |
|------------------|---------|--------------------------------------------------------------------------------------------------|
| `version`        | string  | Conan client version 2.x to be installed. By default, it's the latest version available.         |
| `home`           | string  | A custom path to be used as Conan cache directory.                                               |
| `audit_token`    | string  | The Conan audit token to authenticate to the Audit server with Conan.                            |
| `config_urls`    | list    | URLs of the Git repositories containing the custom Conan configurations to be installed.         |
| `cache_packages` | boolean | Cache all stored Conan packages, under Conan cache, using Github cache support. false by default |


## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE.md) file for details.
