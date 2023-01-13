# devbox-install-action

This action downloads the devbox CLI and installs the Nix packages defined in your `devbox.json`.

[![version](https://img.shields.io/github/v/release/jetpack-io/devbox-install-action?color=green&label=version&sort=semver)](https://github.com/jetpack-io/devbox-install-action/releases) [![tests](https://github.com/jetpack-io/devbox-install-action/actions/workflows/test.yaml/badge.svg)](https://github.com/jetpack-io/devbox-install-action/actions/workflows/test.yaml?branch=main)

## Example Workflow

```
name: Testing with devbox

on: push

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Install devbox
        uses: jetpack-io/devbox-install-action@v0.2.0

      - name: Run arbitrary commands
        run: devbox shell -- echo "done!"

      - name: Run a script called test
        run: devbox run test
```

## Configure Action

### Action Inputs

| Input argument | description                                                         | default               |
| -------------- | ------------------------------------------------------------------- | --------------------- |
| project-path   | Path to the folder that contains a valid `devbox.json`              | repo's root directory |
| enable-cache   | Cache the entire Nix store in github based on your `devbox.json`    | false                 |
| devbox-version | Specify devbox CLI version you want to pin to. Only supports >0.2.2 | latest                |

### Example Configuration

Here's an example job with all three inputs:

```
- name: Install devbox
  uses: jetpack-io/devbox-install-action@v0.2.0
  with:
    project-path: 'path-to-folder'
    enable-cache: true
    devbox-version: '0.2.2'
```
