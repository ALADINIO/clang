
# Clang Installation Instructions

These instructions describe how to build and install Clang.

# Table of Contents

- [Organization](#organization)
- [Configure and Build LLVM](#configure)
- [(Optional) Verify Your Build](#verify)
- [Install Clang](#install)

## Step 1: Organization
<a name='organization'></a>

Clang is designed to be built as part of an LLVM build. Assuming that the LLVM
source code is located at $LLVM_SRC_ROOT, then the clang source code should be
installed as:

  ```$LLVM_SRC_ROOT/tools/clang```

The directory is not required to be called clang, but doing so will allow the
LLVM build system to automatically recognize it and build it along with LLVM.

## Step 2: Configure and Build LLVM
<a name='configure'></a>

Configure and build your copy of LLVM (see ``$LLVM_SRC_ROOT/GettingStarted.html``
for more information).

Assuming you installed clang at ``$LLVM_SRC_ROOT/tools/clang`` then Clang will
automatically be built with LLVM. Otherwise, run ``make`` in the Clang source
directory to build Clang.

## Step 3: (Optional) Verify Your Build
<a name='verify'></a>

It is a good idea to run the Clang tests to make sure your build works
correctly. From inside the Clang build directory, run ``make test`` to run the
tests.

## Step 4: Install Clang
<a name='install'></a>

From inside the Clang build directory, run ``make install`` to install the Clang
compiler and header files into the prefix directory selected when LLVM was
configured.

The Clang compiler is available as 'clang' and 'clang++'. It supports a gcc like
command line interface. See the man page for clang for more information.
