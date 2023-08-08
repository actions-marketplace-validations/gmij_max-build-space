# max-build-space
Maximize available disk space for build tasks


Added [maximize-build-disk-space](https://github.com/jlumbroso/free-disk-space) ability to remove large packets on the basis of [maximize-build-disk-space](https://github.com/easimon/maximize-build-space)

On Ubuntu 22.04.2, max have 67G disk space for you workspace, use this action.


## Usage
```

name: My build action requiring more space
on: push

jobs:
  build:
    name: Build my artifact
    runs-on: ubuntu-latest
    steps:
      - name: Maximize build space
        uses: gmij/max-build-space@main
        with:
          root-reserve-mb: 512
          swap-size-mb: 1024
          remove-dotnet: 'true'
      - name: Checkout
        uses: actions/checkout@v3

      - name: Build
        run: |
          echo "Free space:"
          df -h

```

## Inputs

```

  root-reserve-mb:
    description: 'Space to be left free on the root filesystem, in Megabytes.'
    required: false
    default: '1024'
  temp-reserve-mb:
    description: 'Space to be left free on the temp filesystem (/mnt), in Megabytes.'
    required: false
    default: '100'
  swap-size-mb:
    description: 'Swap space to create, in Megabytes.'
    required: false
    default: '4096'
  overprovision-lvm:
    description: |
      Create the LVM disk images as sparse files, making the space required for the LVM image files *appear* unused on the
      hosting volumes until actually allocated. Use with care, this can lead to surprising out-of-disk-space situations.
      You should prefer adjusting root-reserve-mb/temp-reserve-mb over using this option.
    required: false
    default: 'false'
  build-mount-path:
    description: 'Absolute path to the mount point where the build space will be available, defaults to $GITHUB_WORKSPACE if unset.'
    required: false
  pv-loop-path:
    description: 'Absolute file path for the LVM image created on the root filesystem, the default is usually fine.'
    required: false
    default: '/pv.img'
  tmp-pv-loop-path:
    description: 'Absolute file path for the LVM image created on the temp filesystem, the default is usually fine. Must reside on /mnt'
    required: false
    default: '/mnt/tmp-pv.img'
  remove-dotnet:
    description: 'Removes .NET runtime and libraries.'
    required: false
    default: 'false'
  remove-android:
    description: 'Removes Android SDKs and Tools.'
    required: false
    default: 'false'
  remove-haskell:
    description: 'Removes GHC (Haskell) artifacts.'
    required: false
    default: 'false'
  remove-codeql:
    description: 'Removes CodeQL Action Bundles.'
    required: false
    default: 'false'
  remove-docker-images:
    description: 'Removes cached Docker images.'
    required: false
    default: 'false'
  # option inspired by:
  # https://github.com/apache/flink/blob/master/tools/azure-pipelines/free_disk_space.sh    
  remove-large-packages:
    description: 'Removes large packages. (frees ~4.6 GB)'
    required: false
    default: 'false'

```
