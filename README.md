# README

  根据自己最近看到的这篇文章[http://gnuarmeclipse.github.io](http://gnuarmeclipse.github.io) ，记录下在Linux下配置GNU MCU Eclipse：<br>
  
  * 目的： 在Ubuntu下实现`STM32CubeMX`和`Eclipse`使用和调试
  * 环境： 基于 VMware Workstations 12 Pro 英文版 下 `Ubuntu 16.04 LTS 64 bit` 英文版
  
## 前期Linux准备工作
  * VMware下安装`Ubuntu 16.04 LTS 64 bit` （推荐安装Ubuntu 64 bit，不然后面安装包会出现异常导致安装失败，这里不做详细步骤记录。）
  * Linux基本配置和指令，请参考[http://blog.csdn.net/zhengyangliu123/article/details/54780835](http://blog.csdn.net/zhengyangliu123/article/details/54780835) ，这位博主很有耐心，步骤详尽，且时间很近，可参考性较高。<br>
  这里列出我使用的几个配置：<br>
    * 安装VMware tools 以实现主机和虚拟机间拖拽文件，自动调整分辨率等功能：<br>
    下拉菜单`VM` --> `Install VMware tools` --> Ubuntu 提示有磁盘插入 --> 打开Terminal（Ctrl+Alt+T）
    ```Bash
    $ cd /media/LoginName/VMware\ Tools/
    $ cp VVMwareTools-10.1.6-5214329.tar.gz /home/LoginName/
    $ cd ~
    $ tar -zxvf VVMwareTools-10.1.6-5214329.tar.gz
    $ cd vmware-tools-distrib/
    $ ./vmware-install.pl
    ```
    --> 一路回车，看到Enjoy， --the VMware team --> 删除源文件和解压缩后的文件 -->
    ```Bash
    $ rm VMwareTools-10.1.6-5214329.tar.gz
    $ rm -rf vmware-tools-distrib/
    ```
    * 更新已安装的软件
    ```Bash
    $ sudo apt-get dist-upgrade
    ```
    * 其他关于语言和输入法的设置[链接](http://blog.csdn.net/zhengyangliu123/article/details/54780835)中的每个章节都有配图步骤。
    
## 下载二进制文件（Download the binary files）

* Tips:根据作者[http://gnuarmeclipse.github.io/downloads](http://gnuarmeclipse.github.io/downloads) 提到两种下载途径，一个是自动，另一个是手动。这里作者推荐在手动在`GitHub Releases`下载。

* 手动下载二进制文件：(在每个对应的repository当中--> releases page --> latest release --> 找到对应的版本)<br>
    * Plug-ins(releases)： [gnuarmeclipse/eclipse-plugins](https://github.com/gnu-mcu-eclipse/eclipse-plugins)
    * QEMU（releases）: [gnuarmeclipse/QEMU](https://github.com/gnu-mcu-eclipse/qemu)
    * OpenOCD (releases): [gnuarmeclipse/OpenOCD](https://github.com/gnu-mcu-eclipse/openocd)
    
## 安装ARM工具链
  * [工具链下载地址](https://developer.arm.com/open-source/gnu-toolchain/gnu-rm/downloads)
   
   如果你使用的是32位的Linux就需要安装一些库文件https://gnu-mcu-eclipse.github.io/toolchain/arm/install/
   
  * 安装工具链
   ```Bash
   $ mkdir -p ${HOME}/opt
   $ cd ${HOME}/opt
   $ tar xjf ~/Downloads/gcc-arm-none-eabi-6-2017-q2-update-linux.tar.bz2
   $ chmod -R -w ${HOME}/opt/gcc-arm-none-eabi-6-2017-q2-update
   ```
* 测试工具链是否运行（**这里作者强烈建议不要改变安装路径，也不要把工具链添加到用户或者系统的环境变量当中**）

   ```Bash
   $ ${HOME}/opt/gcc-arm-none-eabi-6-2017-q2-update/bin/arm-none-eabi-gcc --version
   arm-none-eabi-gcc (GNU Tools for ARM Embedded Processors 6-2017-q2-update) 6.3.1 20170620 (release) [ARM/embedded-6-branch     revision 249437]
   ```

## 安装JDK 和 Eclipse 

* 安装JDK 
```Bash
  $ cd ${HOME}/opt
  $ tar -zxvf ~/Downloads/jdk-8u151-linux-x64.tar.gz
```
   
 
  
    
    
    
  


