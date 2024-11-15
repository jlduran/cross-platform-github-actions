https://github.com/cross-platform-actions/action

# Minimal Example

```yaml
name: CI

on: [push]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Test
        uses: cross-platform-actions/action@v0.25.0
        with:
          operating_system: freebsd
          version: '14.1'
          run: |
            uname -a
            echo $SHELL
            pwd
            ls -lah
            whoami
            env | sort
```

# Full Example

```yaml
name: CI

on: [push]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        os:
          - name: freebsd
            architecture: x86-64
            version: '14.1'

          - name: freebsd
            architecture: arm64
            version: '10.0'

          - name: openbsd
            architecture: x86-64
            version: '7.6'

          - name: openbsd
            architecture: arm64
            version: '7.6'

          - name: netbsd
            architecture: x86-64
            version: '10.0'

          - name: netbsd
            architecture: arm64
            version: '10.0'

    steps:
      - uses: actions/checkout@v4

      - name: Test on ${{ matrix.os.name }}
        uses: cross-platform-actions/action@v0.25.0
        env:
          MY_ENV1: MY_ENV1
          MY_ENV2: MY_ENV2
        with:
          environment_variables: MY_ENV1 MY_ENV2
          operating_system: ${{ matrix.os.name }}
          architecture: ${{ matrix.os.architecture }}
          version: ${{ matrix.os.version }}
          shell: bash
          memory: 5G
          cpu_count: 4
          run: |
            uname -a
            echo $SHELL
            pwd
            ls -lah
            whoami
            env | sort
```
