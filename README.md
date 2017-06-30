# How to build android custom kernel 

## Target Kernel
LG Urbane SmartWatch
- Build-Version : M1D65H

## Develope Env
### Complie Env
Ubuntu 16.04.2 LTS
gcc version 5.4.0

## Setup

### Download kernel code 
- http://opensource.lge.com/osSch/list?types=ALL&search=urbane
- Choose your watch's android version at git branch
```
git clone -b android-msm-bass-3.10-lollipop-mr1-wear-release https://android.googlesource.com/kernel/msm
```

### Tool Chain
```
git clone https://android.googlesource.com/platform/prebuilts/gcc/linux-x86/arm/arm-eabi-4.8
```

### Run scripts
```
cd {kernel folder}
make CROSS_COMPILE={/home/ubuntu/arm-eabi-4.8/bin/arm-eabi- ARCH=arm} bass_defconfig
make CROSS_COMPILE={/home/ubuntu/arm-eabi-4.8/bin/arm-eabi- ARCH=arm}
```

## After Build
you can find the build image(zImage) at arch/arm/boot/zImage-dtb

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
mkbootimg --base 0 --pagesize 2048 --kernel_offset 0x80208000 --ramdisk_offset 0x82200000 --second_offset 0x81100000 --tags_offset 0x80200100 --cmdline 'console=ttyHSL0,115200,n8 androidboot.hardware=flo user_debug=31 msm_rtb.filter=0x3F ehci-hcd.park=3' --kernel {mykernel} --ramdisk ramdisk.cpio.gz -o boot.img
```

## Set your boot.img at SmartWatch
```
adb reboot bootloader
fastboot oem unlock
fastboot boot myboot.img
```
