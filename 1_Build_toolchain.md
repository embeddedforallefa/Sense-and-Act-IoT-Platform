# Build toolchain

## Technical requirements

To follow along, make sure you have the following:

A Linux-based host system with autoconf, automake, bison, bzip2, cmake, flex, g++, gawk, gcc, gettext, git, gperf, help2man, libncurses5-dev, libstdc++6, libtool, libtool-bin, make, patch, python3-dev, rsync, texinfo, unzip, wget, and xz-utils or their equivalents installed.

Here is the command to install all the required packages on Ubuntu 22.04 LTS:

\$ sudo apt-get install autoconf autREADMEomake bison bzip2 cmake flex g++ gawk gcc gettext git gperf help2man libncurses5-dev libstdc++6 libtool libtool-bin make patch python3-dev rsync texinfo unzip wget xz-utils

## Installing crosstool-NG
Before we can build crosstool-NG from source, we will first need to install a native
toolchain and some build tools on your host machine as above section for complete list of build and runtime dependencies.

Next, get the current release from the crosstool-NG Git repository. I am using version 1.24.0. Extract it and create the frontend menu system, ct-ng, as shown in the following commands:

\$ git clone https://github.com/crosstool-ng/crosstool-ng.git

\$ cd crosstool-ng

\$ ./bootstrap

\$ ./configure --prefix=${PWD}

\$ make

\$ make install

## Building a toolchain using crosstool-NG for BeagleBone Black

\$ bin/ct-ng show-arm-cortex_a8-linux-gnueabi

\$ bin/ct-ng arm-cortex_a8-linux-gnueabi

\$ bin/ct-ng menuconfig

There are three configuration changes that we need to make at this point:
1. In 'Paths and misc options', disable 'Render the toolchain read-only' (CT_PREFIX_DIR_RO)
2. In 'Target options | Floating point', select 'hardware (FPU)' (CT_ARCH_FLOAT_HW).
3. In 'Target options', enter neon for 'Use specific FPU'

\$ bin/ct-ng build

The toolchain will be installed in in ~/x-tools/arm-cortex_a8-linux-gnueabihf.

## Building a toolchain using crosstool-NG for QEMU

\$ bin/ct-ng distclean

\$ bin/ct-ng arm-unknown-linux-gnueabi

We update below configuration parameter by running bin/ct-ng menuconfig.
1. In Paths and misc options, disable Render the toolchain read-only (CT_PREFIX_DIR_RO).

\$ bin/ct-ng menuconfig

\$ bin/ct-ng build

The toolchain will be installed in ~/x-tools/arm-unknown-linux-gnueabi.

## Examining the toolchiain

The ARM Cortex A8 toolchain is in the directory ~/x-tools/arm-cortex_a8-linux-gnueabihf/bin and we can find the cross compiler arm-cortex_a8-linux-gnueabihf-gcc. Let us add this directory to path to use the cross compiler.

\$ PATH=~/x-tools/arm-cortex_a8-linux-gnueabihf/bin:$PATH

We compile the helloworld.c present in source:

\$ arm-cortex_a8-linux-gnueabihf-gcc helloworld.c -o helloworld

We can confirm that it has been cross-compiled by using file command.

\$ file helloworld

To know more about the compiler: 

\$ arm-cortex_a8-linux-gnueabihf-gcc --version

To learn how the cross-compiler was configured, use -v:

\$ arm-cortex_a8-linux-gnueabihf-gcc -v

### sysroot

The toolchain sysroot is a directory that contains subdirectories for libraries, header
files, and other configuration files. You can see the location of the default sysroot by using -print-sysroot:

\$ arm-cortex_a8-linux-gnueabihf-gcc -print-sysroot

