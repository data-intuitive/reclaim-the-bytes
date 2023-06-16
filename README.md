# Reclaim The Bytes

Remove unused software to reclaim disk space.

The inspiration for this action:

- [easimon/maximize-build-space](https://github.com/easimon/maximize-build-space)
- [ThewApp/free-actions](https://github.com/ThewApp/free-actions)

**Caveat:** Removal of unnecessary software is implemented by `rm -rf`
on specific folders, not by using a package manager or anything
sophisticated. While this is quick and easy, it might delete
dependencies that are required by your job and so break your build
(e.g. because your build job uses a .NET based tool and you removed the
required runtime). Please verify which software may or may not be
removed for your specific use case.

## Usage

``` yaml
name: My build action requiring more space
on: push

jobs:
  build:
    name: Build my artifact
    runs-on: ubuntu-latest
    steps:
      - name: Reclaim the bytes
        uses: data-intuitive/reclaim-the-bytes@v1

      - name: Checkout
        uses: actions/checkout@v3

      - name: Build
        run: |
          echo "Free space:"
          df -h
```

## Inputs

- `remove-dotnet`: Remove .NET runtime and libraries. Default: `true`.
- `remove-android`: Remove Android SDKs and Tools. Default: `true`.
- `remove-haskell`: Remove GHC (Haskell) artifacts. Default: `true`.
- `remove-codeql`: Remove CodeQL Action Bundles. Default: `true`.
- `remove-docker-images`: Remove cached Docker images. Default: `true`.
- `remove-large-packages`: Remove large packages. Default: `false`.
