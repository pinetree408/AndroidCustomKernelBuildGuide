# How to build android custom kernel 

## Target Kernel
Sony SmartWatch 3

## Develope Env
Ubuntu 16.04.2 LTS
gcc version 5.4.0

## Setup

```
git clone git@github.com:crpalmer/android_kernel_sony_tetra.git
git clone git@github.com:crpalmer/tetra-kernel-build-configs.git
git clone git@github.com:crpalmer/android-kernel-build-tools.git

git clone https://android.googlesource.com/platform/prebuilts/gcc/linux-x86/arm/arm-eabi-4.8
sudo apt-get install bc
sudo apt-get install lib3stdc++6
```

## Modified
CROSS_COMPLE parameter at build-prebuilt.sh
