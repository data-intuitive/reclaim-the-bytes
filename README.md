# Reclaim The Bytes

Remove unused software to reclaim disk space.

The inspiration for this action:

- [easimon/maximize-build-space](https://github.com/easimon/maximize-build-space)
- [ThewApp/free-actions](https://github.com/ThewApp/free-actions)
- [Other GitHub
  actions](https://github.com/search?q=%22rm+-rf+%2Fusr%2Fshare%2Fdotnet%22&type=code)
- [This
  discussion](https://github.com/actions/runner-images/discussions/3242)

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
        uses: data-intuitive/reclaim-the-bytes@v2
        with:
          remove-hosted-tool-cache: true
          remove-go: false
          remove-codeql: false
          remove-powershell: false
          remove-android-sdk: true
          remove-haskell-ghc: true
          remove-swift: true
          remove-dotnet: true
          remove-docker-images: true
          remove-swap: false

      - name: Checkout
        uses: actions/checkout@v3

      - name: Report free space
        run: |
          echo "Free space:"
          df -h
```

## Inputs

- `remove-hosted-tool-cache`: Remove the hosted tool cache, including
  Go, CodeQL, Powershell. Execution time: 5s. Space freed: 10GB.
  Default: `true`.
- `remove-go`: Remove Go libraries. Execution time: 2s. Space freed:
  1GB. Default: `false`.
- `remove-codeql`: Remove CodeQL. Execution time: 1s. Space freed: 6GB.
  Default: `false`.
- `remove-powershell`: Remove PowerShell. Execution time: 1s. Space
  freed: 1GB. Default: `false`.
- `remove-android-sdk`: Remove Android SDK. Execution time: 35s. Space
  freed: 12GB. Default: `true`.
- `remove-haskell-ghc`: Remove Haskell GHC. Execution time: 3s. Space
  freed: 5GB. Default: `true`.
- `remove-swift`: Remove Swift. Execution time: 1s. Space freed: 2GB.
  Default: `true`.
- `remove-dotnet`: Remove .NET libraries. Execution time: 5s. Space
  freed: 2GB. Default: `true`.
- `remove-docker-images`: Remove cached Docker images. Execution time:
  18s. Space freed: 5GB. Default: `true`.
- `remove-swap`: Remove swap. Execution time: 1s. Space freed: 5GB on
  /mnt. Default: `true`.

## Measurements

In deciding which software to remove, you do not only need to take into
account whether the software is needed or not, but also how long it
takes to remove vs. the amount of disk space removing it frees up. Here
is a visualisation of that information.

| Software          | OS           | Duration (s) | Space freed (GB) |
|:------------------|:-------------|-------------:|-----------------:|
| android-sdk       | ubuntu-20.04 |         26.8 |               12 |
| android-sdk       | ubuntu-22.04 |         37.0 |               13 |
| codeql            | ubuntu-20.04 |          1.0 |                6 |
| codeql            | ubuntu-22.04 |          1.0 |                6 |
| docker-images     | ubuntu-20.04 |         18.4 |                5 |
| docker-images     | ubuntu-22.04 |         17.0 |                4 |
| dotnet            | ubuntu-20.04 |          5.6 |                3 |
| dotnet            | ubuntu-22.04 |          2.2 |                2 |
| go                | ubuntu-20.04 |          2.2 |                1 |
| go                | ubuntu-22.04 |          1.2 |                2 |
| haskell-ghc       | ubuntu-20.04 |          2.8 |                5 |
| haskell-ghc       | ubuntu-22.04 |          3.4 |                5 |
| hosted-tool-cache | ubuntu-20.04 |          5.0 |               10 |
| hosted-tool-cache | ubuntu-22.04 |          3.4 |                9 |
| powershell        | ubuntu-20.04 |          0.8 |                1 |
| powershell        | ubuntu-22.04 |          0.6 |                2 |
| python            | ubuntu-20.04 |          0.0 |                0 |
| python            | ubuntu-22.04 |          0.4 |                0 |
| swap              | ubuntu-20.04 |          0.0 |                0 |
| swap              | ubuntu-22.04 |          0.0 |                0 |
| swift             | ubuntu-20.04 |          0.6 |                2 |
| swift             | ubuntu-22.04 |          0.4 |                2 |

![](resources/README_files/measurements-plot-1.png)
