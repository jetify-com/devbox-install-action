# devbox-install-action

This action downloads the devbox CLI and installs the Nix packages defined in your `devbox.json`.

[![version](https://img.shields.io/github/v/release/jetify-com/devbox-install-action?color=green&label=version&sort=semver)](https://github.com/jetify-com/devbox-install-action/releases) [![tests](https://github.com/jetify-com/devbox-install-action/actions/workflows/test.yaml/badge.svg)](https://github.com/jetify-com/devbox-install-action/actions/workflows/test.yaml?branch=main)

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
        uses: jetify-com/devbox-install-action@v0.12.0

      - name: Run arbitrary commands
        run: devbox run -- echo "done!"

      - name: Run a script called test
        run: devbox run test
```

## Configure Action

### Action Inputs

| Input argument           | description                                                                           | default               |
| ------------------------ | ------------------------------------------------------------------------------------- | --------------------- |
| project-path             | Path to the folder that contains a valid `devbox.json`                                | repo's root directory |
| enable-cache             | Cache the entire Nix store in github based on your `devbox.json`                      | false                 |
| refresh-cli              | Specify whether the CLI should be redownloaded                                        | false                 |
| devbox-version           | Specify devbox CLI version you want to pin to. Only supports >0.2.2                   | latest                |
| sha256-checksum          | Specify an explicit checksum for the devbox binary                                    |                       |
| disable-nix-access-token | Disable configuration of nix access-tokens with the GitHub token used in the workflow | false                 |
| skip-nix-installation    | Skip the installation of nix                                                          | false                 |
| extra-nix-config         | Gets appended to `nix.conf` if passed                                                 |                       |

### Example Configurations

Here's an example job with most inputs:

```
- name: Install devbox
  uses: jetify-com/devbox-install-action@v0.12.0
  with:
    project-path: 'path-to-folder'
    enable-cache: 'true'
    refresh-cli: 'false'
    devbox-version: 0.13.4
    disable-nix-access-token: 'false'
    sha256-checksum: <checksum>
```

#### Usage on a GitHub Enterprise Server

On a privately hosted GitHub Enterprise Server, the `github.token` available in the context is not valid for accessing `api.github.com`, 
which can lead to failures due to the rate-limit for unauthenticated requests. To work around this, you can provide a personal access token
for `api.github.com` in the `extra-nix-config` input.
Additionally, it might be necessary to provide a token for your GitHub Enterprise Server, if you are using Nix packages from there.

```
- name: Install devbox
  uses: jetify-com/devbox-install-action@v0.13.0
  with:
    extra-nix-config: access-tokens = my-github-enterprise-server.example.com=${{ github.token }} github.com=${{ secrets.MY_GITHUB_COM_TOKEN }}
```