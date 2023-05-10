<img align="right" width="15%" src="docs/ansible-logo.svg" alt="ansible logo"/>

# setup-lint

[![Test](https://github.com/bec-galaxy/setup-lint/actions/workflows/test.yml/badge.svg)](https://github.com/bec-galaxy/setup-lint/actions/workflows/test.yml)

This action provides the following functionality for GitHub Actions users:

- Installing Python.
- Installing the latest version of Ansible-Lint with yamllint.

> This action is a rolling release, it will install the latest dependencies on each run. Use [the official Ansible action](https://github.com/marketplace/actions/ansible-lint) for more stability. See [Ansible Creator Execution Environment](https://github.com/ansible/creator-ee) for additional information.

## Basic usage

See [action.yml](action.yml)

**Lint**
```yaml
steps:
  - name: Checkout the codebase
    uses: actions/checkout@v3

  - name: Setup Lint
    uses: bec-galaxy/setup-lint@{Version}

  - name: Run Lint tests
    run: ansible-lint
```

**Lint and Molecule**

```yaml
---
name: Molecule

"on":
  pull_request:

env:
  PY_COLORS: "1"
  ANSIBLE_FORCE_COLOR: "1"
jobs:
  lint:
    name: Lint
    runs-on: ubuntu-latest
    steps:
      - name: Checkout the codebase
        uses: actions/checkout@v3

      - name: Setup Lint
        uses: bec-galaxy/setup-lint@{Version}

      - name: Run Lint tests
        run: ansible-lint

  molecule:
    name: Molecule
    runs-on: ubuntu-latest
    needs: lint
    steps:
      - name: Checkout the codebase
        uses: actions/checkout@v3

      - name: Setup Molecule
        uses: bec-galaxy/setup-molecule@{Version}

      - name: Run Molecule tests
        run: molecule test
```

> Environment variables `PY_COLORS` and `ANSIBLE_FORCE_COLOR` are optional and only activate the color output in the pipline.

## Packages

The following python packages are installed in this action:

- [ansible-lint](https://pypi.org/project/ansible-lint/)
- [yamllint](https://pypi.org/project/yamllint/)

## YAML file

A YAML lint sample file for this action. The ansible linter does not use all rules (like `comments-indentation`) without this extra `.yamllint` file.

```yaml
---
rules:
  anchors: enable
  braces: enable
  brackets: enable
  colons: enable
  commas: enable
  comments:
    level: warning
  comments-indentation:
    level: warning
  document-end: disable
  document-start:
    level: warning
  empty-lines: enable
  empty-values: disable
  float-values: disable
  hyphens: enable
  indentation: enable
  key-duplicates: enable
  key-ordering: disable
  line-length:
    max: 120
    allow-non-breakable-words: true
    allow-non-breakable-inline-mappings: false
  new-line-at-end-of-file: enable
  new-lines: enable
  octal-values: disable
  quoted-strings: disable
  trailing-spaces: enable
  truthy:
    level: warning
```

[See here](https://yamllint.readthedocs.io/en/stable/configuration.html#default-configuration) for a current default configuration.

## Licence

This project is licensed under MIT - See the [LICENSE](LICENSE) file for more information.

---

&uarr; [Back to top](#)