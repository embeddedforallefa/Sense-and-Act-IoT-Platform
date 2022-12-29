# Steps for building Linux image for BeagleBone Black using Yocto Project 

sudo apt-get install gawk wget git-core diffstat unzip texinfo gcc-multilib build-essential chrpath socat cpio python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping libsdl1.2-dev xterm bc build-essential chrpath diffstat gawk git libncurses5-dev pkg-config socat subversion texi2html texinfo u-boot-tools lz4

git clone -b kirkstone git://git.yoctoproject.org/poky.git poky-kirkstone

mkdir bbb

source poky-kirkstone/oe-init-build-env ./bbb/build

vim conf/local.conf 
MACHINE ?= "beaglebone-yocto"

## build the minimal image

bitbake core-image-minimal

## Flash the image to BBB

cd tmp/deploy/images/beaglebone-yocto

make sure to delete all the partition of the sd card

dd if=core-image-minimal-beaglebone-yocto.wic of=/dev/sdb bs=4M

