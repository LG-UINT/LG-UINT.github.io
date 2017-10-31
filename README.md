# README

  根据自己最近看到的这篇文章[http://gnuarmeclipse.github.io](http://gnuarmeclipse.github.io) ，记录下在Linux下配置GNU MCU Eclipse：<br>
  * 目的： 在Ubuntu下实现`STM32CubeMX`和`Eclipse`使用和调试
  * 环境： 基于 VMware Workstations 12 Pro 英文版 下 `Ubuntu 16.04 LTS` 英文版
  
## 前期Linux准备工作
  * VMware下安装`Ubuntu 16.04 LTS` （这里不做详细步骤记录，推荐使用Vmware Workstations 12）
  * Linux基本配置和指令，请参考[http://blog.csdn.net/zhengyangliu123/article/details/54780835](http://blog.csdn.net/zhengyangliu123/article/details/54780835) ，这位博主很有耐心，步骤详尽，且时间很近，可参考性较高。<br>
  这里列出我使用的几个配置：<br>
    * 安装VMware tools 以实现主机和虚拟机间拖拽文件，自动调整分辨率等功能：<br>
    下拉菜单`VM` --> `Install VMware tools` --> Ubuntu 提示有磁盘插入 --> 打开Terminal（Ctrl+Alt+T）
    ```Bash
    cd /media/LoginName/VMware\ Tools/
    cp VVMwareTools-10.1.6-5214329.tar.gz /home/LoginName/
    cd ~
    tar -zxvf VVMwareTools-10.1.6-5214329.tar.gz
    cd vmware-tools-distrib/
    ./vmware-install.pl
    ```
    
    
  


