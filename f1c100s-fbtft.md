接线表


选用内核

kernel 4.14

```
$ cd
$ wget https://github.com/steward-fu/miyoo/releases/download/v1.2/kernel.7z
$ 7za x kernel.7z
$ cd kernel
$ make ARCH=arm CROSS_COMPILE=arm-linux- miyoo_defconfig
$ make ARCH=arm CROSS_COMPILE=arm-linux- clean
$ make ARCH=arm CROSS_COMPILE=arm-linux- -j8
```

设备树

