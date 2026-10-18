# clang-format binaries

This repo contain pre-built binaries of clang-format to be used in other projects.

Reason is to pin clang-format against exact versions, since formatting rules are directly
tied to the version of clang-format.

We target recent Windows and Linux (See each branch).

## v23 branch

Current version of clang-format is 23.1.2.

## Windows

It contains the officially built clang-format.exe from https://github.com/llvm/llvm-project/releases/tag/llvmorg-23.1.2.

## Linux (glibc-2.28)

Here, we build using Rocky Linux 8.10, and try to minimize the dependencies.

Required software (may be incomplete):

```bash
sudo dnf groupinstall -y "Development Tools"
sudo dnf install -y wget cmake ninja-build
```

```bash
wget https://github.com/llvm/llvm-project/archive/refs/tags/llvmorg-23.1.2.tar.gz
tar xf llvmorg-23.1.2.tar.gz
cd llvm-project-llvmorg-23.1.2
cmake -S llvm -B build -G Ninja  \
    -DLLVM_ENABLE_PROJECTS="clang" \
    -DCMAKE_BUILD_TYPE=Release \
    -DLLVM_TARGETS_TO_BUILD="X86" \
    -DLLVM_DISTRIBUTION_COMPONENTS="clang-format" \
    -DCMAKE_INSTALL_PREFIX="$PWD/install" \
    -DLLVM_ENABLE_ZLIB=OFF \
    -DLLVM_ENABLE_ZSTD=OFF
ninja install-distribution
```

This takes a long time. Resulting binaries are in `llvm-project-llvmorg-23.1.2/install/bin` based on the original path when starting the build.