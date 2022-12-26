# 加入了 Archlinuxcn(xtom aliyun) CachyOS 源
# pkgbuild-action
GitHub action to build and check a PKGBUILD package

Replace makepkg with paru by Pinghigh

## Features
* Builds package(s) with paru
* Runs on a bare minimum Arch Linux install to help detect missing dependencies (support AUR dependence)
* Outputs built package archives
* Checks PKGBUILD and package archives with [namcap](https://wiki.archlinux.org/index.php/namcap)

## Interface
Inputs:
* `pkgdir`: Relative path to directory containing the PKGBUILD file
            (repo root by default).
* `namcapDisable`: Disable namcap checks if nonempty.
* `namcapRules`: A comma-separated list of rules for namcap to run.
* `namcapExcludeRules`: A comma-separated list of rules for namcap not to run.
* `makepkgArgs`: Additional arguments to pass to `makepkg`.

Outputs:
* `pkgfileN`: Filename of Nth built package archive (ordered as `makepkg --packagelist`).
   Empty if not built. N = 0, 1, ...

## Example Usage
```yaml
name: PKGBUILD CI

on: [push, pull_request]

jobs:
  pkgbuild:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout
      uses: actions/checkout@v2
    - name: Makepkg Build and Check
      id: makepkg
      uses: Pinghigh/pkgbuild-action@v4.2
    - name: Print Package Files
      run: |
        echo "Successfully created the following package archive"
        echo "Package: ${{ steps.makepkg.outputs.pkgfile0 }}"
    # Uncomment to upload the package as an artifact
    # - name: Upload Package Archive
    #   uses: actions/upload-artifact@v2
    #   with:
    #     path: ${{ steps.makepkg.outputs.pkgfile0 }}
```
