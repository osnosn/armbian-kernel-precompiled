# 编译笔记：
1. 俄罗斯 balbes150 的 armbian 5.77 默认已配备编译工具，但，需要安装 libncurses-dev 用于 menuconfig，  
   安装 zstd 用于生成 initrd.img 的时候使用 zstd 压缩协议，获得更好的压缩效果，  
   如果没有 zstd，安装程序会退而用 gzip 压缩 initrd.img  
3. 从 github.com/ophub 的 kernel_stable 处获取 5.15.126 内核成品，  
   解压 boot-5.15.126-ophub.tar.gz 获得 config-5.15.126-ophub，作为编译的配置模板  
5. 从 github.com/unifreq/linux-5.15.y 处，`git clone https://github.com/unifreq/linux-5.15.y.git` ，获取内核源代码，

   * git log 得到：  
     ```
     commit c78b6d21e2a7a5adf17bf21f71fca97059d580a5 (HEAD -> main, origin/main, origin/HEAD)
     Author: uniqfreq <flippy@sina.com>
     Date:   Sun Aug 13 21:55:10 2023 +0800
     
         up to 5.15.126
     
     commit 34a39eae0865e62de24d89682cea386ecdb8bfab
     Author: uniqfreq <flippy@sina.com>
     Date:   Wed Aug 9 16:52:16 2023 +0800
     
         up to 5.15.125
     ```
     第1行 commit c78b6d21e2a7a5adf17bf21f71fca97059d580a5 即 5.15.126 的 commit id   
   * `git checkout c78b6d21e2a7a5adf17bf21f71fca97059d580a5` 取出 5.15.126 源代码  
6. 将第2步得到的配置文件 config 拷贝至内核根目录 5.15.y 下并更名为.config，执行 make menuconfig 进行必要的定制  
7. N1的千兆网卡芯片是 RTL8111F，小心不要移除以太网卡分类下的 STMicroelectronics devices；  
   wifi/BT 芯片是 BCM/CYW43455（实际上是 Broadcomm 芯，和树莓派一样）  
8. 编译指令：`make -j4 bindeb-pkg` （-j4 完全利用了 n1 的4核，不加这参数只会利用1核，bindeb-pkg 用于编译完成后打包成 deb 安装包）  
   耗时约6小时。  
9. 编译完成后，在内核源代码目录的上一层，生成3个deb，
   只安装 header 和 image 即可，  
   需将内核目录 arch/arm64/boot 下的 Image ，重命名为 zImage 并拷贝到 /boot 下，覆盖原 zImage，
   arch/arm64/boot/dts/amlogic 下的 meson-gxl-s905d-phicomm-n1.dtb，拷贝到/boot/dtb 目录并覆盖原文件  

RAR 文件内容:   
```
sha256:
3e0920a601c3c42106d8727d4d47df6aa599946f30511564e7a1ae4922d61dd3  N1_Kernel_5.15.126+.rar

UNRAR 6.21 freeware      Copyright (c) 1993-2023 Alexander Roshal

Archive: N1_Kernel_5.15.126+.rar
Details: RAR 5

 Attributes      Size     Date    Time   Name
----------- ---------  ---------- -----  ----
    I.A....  22448640  2023-08-19 03:41  zImage
    I.A....    196867  2023-08-19 10:53  config
    I.A....   7748180  2023-08-19 03:58  linux-headers-5.15.126+_5.15.126+-4_arm64.deb
    I.A....  22503300  2023-08-19 04:00  linux-image-5.15.126+_5.15.126+-4_arm64.deb
    I.A....     41498  2023-08-19 00:46  meson-gxl-s905d-phicomm-n1.dtb
----------- ---------  ---------- -----  ----
             52938485                    5
```

