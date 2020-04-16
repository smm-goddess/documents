# Android官方模拟器中安装Magisk

1. 下载Android模拟器的官方镜像
2. 解压ramdisk.img `gzip -d -S .img ramdisk.img`  `mv ramdisk ramdisk.cpio` , 复制一份 `cp ramdisk.cpio ramdisk.cpio.orig` 
3. 准备一个 `config` 文件，内容

``` 
KEEPVERITY=false
KEEPFORCEENCRYPT=false
RECOVERYMODE=false
```

4. 用 `magiskboot` (程序从Magisk.zip中提取出来) 修改 `ramdisk.cpio` 镜像

``` Shell
./magiskboot cpio ramdisk.cpio "add 750 init magiskinit"
./magiskboot cpio ramdisk.cpio "patch"
./magiskboot cpio ramdisk.cpio "backup ramdisk.cpio.orig"
./magiskboot cpio ramdisk.cpio "mkdir 000 .backup"
./magiskboot cpio ramdisk.cpio "add 000 .backup/.magisk config"
```

5. 将修改过的 `ramdisk.cpio` 重新压缩为 `ramdisk.img` 

完整的脚本如下：

``` Shell
gzip -d -S .img ramdisk.img
mv ramdisk ramdisk.cpio
cp ramdisk.cpio ramdisk.cpio.orig
./magiskboot cpio ramdisk.cpio "add 750 init magiskinit"
./magiskboot cpio ramdisk.cpio "patch"
./magiskboot cpio ramdisk.cpio "backup ramdisk.cpio.orig"
./magiskboot cpio ramdisk.cpio "mkdir 000 .backup"
./magiskboot cpio ramdisk.cpio "add 000 .backup/.magisk config"
gzip -9 ramdisk.cpio
mv ramdisk.cpio.gz ramdisk.img
```

