# Buidling and installing u-boot

## Build u-boot

git clone https://github.com/u-boot/u-boot
cd u-boot/
git checkout v2018.01

cd ../
git clone https://github.com/eewiki/u-boot-patches.git
cd ./u-boot-patches/v2018.01
cp 0001-am335x_evm-uEnv.txt-bootz-n-fixes.patch ../../u-boot/
cp 0002-U-Boot-BeagleBone-Cape-Manager.patch ../../u-boot/
cd ../../u-boot
patch -p1 < 0001-am335x_evm-uEnv.txt-bootz-n-fixes.patch
patch -p1 < 0002-U-Boot-BeagleBone-Cape-Manager.patch

PATH=${HOME}/x-tools/arm-cortex_a8-linux-gnueabihf/bin/:$PATH
export CROSS_COMPILE=arm-cortex_a8-linux-gnueabihf-
export ARCH=arm

make am335x_evm_defconfig
make

The results of the compilation are as follows:
• u-boot: U-Boot in ELF object format, suitable for use with a debugger
• u-boot.map: The symbol table
• u-boot.bin: U-Boot in raw binary format, suitable for running on your device
• u-boot.img: This is u-boot.bin with a U-Boot header added, suitable for uploading to a running copy of U-Boot
• u-boot.srec: U-Boot in Motorola S-record (SRECORD or SRE) format, suitable for transferring over a serial connection

## Installing U-Boot

## Launch terminal appilcation
gtkterm -p /dev/ttyUSB0 -s 115200
