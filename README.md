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
    
## 安装工具链（ARM，JLINK,QEMU，OpenOCD）
  * 下载AMR工具链[工具链下载地址](https://developer.arm.com/open-source/gnu-toolchain/gnu-rm/downloads)
   
   如果你使用的是32位的Linux就需要安装一些库文件https://gnu-mcu-eclipse.github.io/toolchain/arm/install/
   
  * 安装ARM工具链
   ```Bash
   $ mkdir -p ${HOME}/opt
   $ cd ${HOME}/opt
   $ tar xjf ~/Downloads/gcc-arm-none-eabi-6-2017-q2-update-linux.tar.bz2
   $ chmod -R -w ${HOME}/opt/gcc-arm-none-eabi-6-2017-q2-update
   ```
  * 测试工具链是否运行（**这里博主强烈建议不要改变安装路径，也不要把工具链添加到用户或者系统的环境变量当中**）

   ```Bash
   $ ${HOME}/opt/gcc-arm-none-eabi-6-2017-q2-update/bin/arm-none-eabi-gcc --version
   arm-none-eabi-gcc (GNU Tools for ARM Embedded Processors 6-2017-q2-update) 6.3.1 20170620 (release) [ARM/embedded-6-branch     revision 249437]
   ```
   * 安装QEMU库
   ```Bash
   $ sudo mkdir -p /opt/gnuarmeclipse
   $ cd /opt/gnuarmeclipse
   $ sudo tar xvf ~/Downloads/gnuarmeclipse-qemu-debian64-2.8.0-201703022210-head.tgz
   ```
   * 测试QEMU启动
   ```Bash
   $ ${HOME}/opt/gnuarmeclipse/qemu/2.8.0-201703022210-head/bin/qemu-system-gnuarmeclipse --version
     GNU ARM Eclipse 64-bits QEMU emulator version 2.8.0 (v2.8.0-644-ge45e0e1-dirty)
     Copyright (c) 2003-2008 Fabrice Bellard
   ```
   * 安装OpenOCD(如果调试工具不是JLink，需要安装OpenOCD)
   ```Bash
   $ sudo mkdir -p /opt/gnuarmeclipse
   $ cd /opt/gnuarmeclipse
   $ sudo tar -xvf ~/Downloads/gnu-mcu-eclipse-openocd-0.10.0-4-20171004-0812-dev-debian64.tgz 
   ```
   * 测试OpenOCD启动
   ```Bash
   $ cd ~
   $ cd opt/gnuarmeclipse/gnu-mcu-eclipse/openocd/0.10.0-4-20171004-0812-dev/bin
   $ ./openocd --version
     GNU MCU Eclipse 64-bits Open On-Chip Debugger 0.10.0+dev-00218-gda43038f (2017-10-04-10:24)
     Licensed under GNU GPL v2
   ```
   * 安装JLink [驱动链接](https://www.segger.com/downloads/jlink#J-LinkSoftwareAndDocumentationPack) <br>
   如果你使用的STlink，可以在上面链接最后的位置找到对应的软件STlink转JLink的软件和教程（ST-Link Reflash Utility）

## 安装JDK,Eclipse和Eclipse plug-ins 

* 安装JDK [下载链接](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
```Bash
  $ cd ${HOME}/opt
  $ tar -zxvf ~/Downloads/jdk-8u151-linux-x64.tar.gz
```
* 配置JAVA环境变量

  编辑 /etc/profile
  在文件末尾添加如下内容： <br>
      export JAVA_HOME=/home/LoginName/opt/jdk1.8.0_151 <br>
      export JRE_HOME=/home/LoginName/opt/jdk1.8.0_151/jre <br>
      export CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib <br>
      export PATH=$JAVA_HOME/bin:$PATH <br>
      
  使配置生效 <br>
  source /etc/profile <br>
  建议重新启动虚拟机
  
 * 安装Eclipse cpp (这里不需要多介绍)
 * 安装Eclipse plug-ins 
 步骤：Help --> Install New Software --> Add --> Archive --> 选取之前下的插件文件ilg.gnumcueclipse.repository-4.1.1-201707111115.zip
 --> 全部安装并重启Eclipse
 * 根据跟人工作习惯配置Eclipse（可选）
 [博主推荐的配置](https://gnu-mcu-eclipse.github.io/eclipse/workspace/preferences/)
 
## 配置Devices Package
  在安装完Eclipse插件以后，在工具栏会出现一个Package的图标。这是需要添加Package的搜索位置：
  
  Windwos --> Preferences --> C/C++ --> Packages --> Add --> 新建如下搜索链接
 
  |Define a New Packages Repository | |
  | ---------- | -------- |
  | Type | XCDL/CMSIS Pack|
  | Name | GNU ARM Eclipse |
  | URL | http://gnuarmeclipse.sourceforge.net/packages/content.xml |
  
  然后点击Package图标，第一次打开是No packages,点击中间窗口右上方最小化左边的黄色双箭头图标，搜索Package。这里需要的时间比较久，请耐心等待。（中途可能会出现Read error，如果几次Retry之后都不行，请Ignore）
  
  [Packs manager](https://gnu-mcu-eclipse.github.io/plugins/packs-manager/) 这里博主详细有解释，如果根据你的想要模拟的硬件（QEMU）和实际开发硬件，如何安装和卸载对应的Packs。
  
  为了能够完成下一章节关于硬件模拟测试教程，建议下载STMicroelectronics--> STM32F4 Series --> keil --> STM32F4xx_DSP --> 2.11.0(449MB) --> 右键 Install （这是我到目前记录时候，看到的最新版本。）
  
## 基于模拟硬件的测试
  这里博主模拟STM32F4的Discovy开发板上闪灯例程： [模拟Blinky例程](https://gnu-mcu-eclipse.github.io/tutorials/blinky-arm/)
  
  我个人在操作完以后，感觉比较重要的点：
  1. 在例子编译完成以后，请首先确认下在 Window --> MCU --> Global ARM Toolchain Path, Global OpenOCD Path, Global QEMU Path，是否都正确配置
  2. 要在项目属性 --> C/C++ Build --> Settings --> Device --> Boards --> 配置你的Device
  
  运行和Debug方式，按照博主的配置fangfa应该是不会出现问题。
  

  
  
  
  
  
  
  
  
  
   
 
  
    
    
    
  


