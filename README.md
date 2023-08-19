# armbian-kernel-precompiled  

## amlogic-s905d-phicomm-n1  
* [armbian-kernel-s905d-phicomm-n1/](https://github.com/osnosn/armbian-kernel-precompiled/tree/main/armbian-kernel-s905d-phicomm-n1/)  
  自编译内核，用于斐讯 N1 盒子，armbian  
  * 从 github.com/ophub 的 kernel_stable 处获取 5.15.126 内核成品，解压 boot-5.15.126-ophub.tar.gz 获得 config-5.15.126-ophub，作为编译的配置模板  
  * 从github.com/unifreq/linux-5.15.y 处，获取内核源代码，  
  * 执行 make menuconfig 进行精简的定制，重新编译的内核。  

