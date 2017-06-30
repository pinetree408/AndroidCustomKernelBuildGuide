# How to build android custom kernel 

## Target Kernel
LG Urbane SmartWatch
- Build-Version : M1D65H

## Develope Env
### Complie Env
Ubuntu 16.04.2 LTS
gcc version 5.4.0

Amazon Linux AMI 2017.03.1
gcc version 4.8.3

## Setup

### Download kernel code 
- http://opensource.lge.com/osSch/list?types=ALL&search=urbane
- Choose your watch's android version at git branch
```
git clone -b android-msm-bass-3.10-lollipop-mr1-wear-release https://android.googlesource.com/kernel/msm
```

### Toolchain
```
git clone https://android.googlesource.com/platform/prebuilts/gcc/linux-x86/arm/arm-eabi-4.8
```
Find a toolchain matching with your device's architecture & gcc ver. (e.g. arm-eabi-4.8)
Read below for information about toolchain.
https://developer.android.com/ndk/guides/standalone_toolchain.html

### Run scripts
```
cd {kernel folder}
make CROSS_COMPILE={/home/ubuntu/arm-eabi-4.8/bin/arm-eabi- ARCH=arm} bass_defconfig
make CROSS_COMPILE={/home/ubuntu/arm-eabi-4.8/bin/arm-eabi- ARCH=arm}
```
Match the directory and ARCH with yours.

## After Build
you can find the build image(zImage) at arch/arm/boot/zImage-dtb
'''
cp {kernel_folder}/arch/arm/boot/zImage-dtb kernel
'''

### Change kernel to boot img
Download pure boot img at https://forum.xda-developers.com/g-watch/general/download-android-wear-update-m1d65b-to-t3557849

### Set up bootimg tools
```
git clone https://github.com/pbatard/bootimg-tools
cd bootimg-tools
make
```

### Unpack boot.img
```
unmkbootimg -i boot.img
```

Get result as below

```
mkbootimg --base 0 --pagesize 2048 --kernel_offset 0x80208000 --ramdisk_offset 0x82200000 --second_offset 0x81100000 --tags_offset 0x80200100 --cmdline 'console=ttyHSL0,115200,n8 androidboot.hardware=flo user_debug=31 msm_rtb.filter=0x3F ehci-hcd.park=3' --kernel kernel --ramdisk ramdisk.cpio.gz -o boot.img
```

### Repack boot.img
Change kernel with your custom kernel

```
mkbootimg --base 0 --pagesize 2048 --kernel_offset 0x80208000 --ramdisk_offset 0x82200000 --second_offset 0x81100000 --tags_offset 0x80200100 --cmdline 'console=ttyHSL0,115200,n8 androidboot.hardware=flo user_debug=31 msm_rtb.filter=0x3F ehci-hcd.park=3' --kernel {mykernel} --ramdisk ramdisk.cpio.gz -o myboot.img
```

Just copy&paste from unmkbootimg result and you could change new boot img file's name if you want. In this case, myboot.img

## Set your myboot.img at SmartWatch
```
adb reboot bootloader
'''

Type below if device is locked

'''
fastboot oem unlock
'''

Boot with your image

'''
fastboot boot myboot.img
```
