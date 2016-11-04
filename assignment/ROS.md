### ROS

----------
#### 一、ROS概述

> ROS (Robot Operating System, 机器人操作系统) 提供一系列程序库和工具以帮助软件开发者创建机器人应用软件。它提供了硬件抽象、设备驱动、函数库、可视化工具、消息传递和软件包管理等诸多功能。ROS遵循BSD开源许可协议。 

----------
####二、ROS安装过程

> 根据官网的安装过程进行
[点击此处获取更多](http://wiki.ros.org/cn/jade/Installation/Ubuntu)


1. 配置 Ubuntu 软件仓库
   配置你的 Ubuntu 软件仓库(repositories) 以允许 "restricted"、"universe" 和 "multiverse"这三种安装模式。
2. 添加sources.list
   配置电脑使其能够安装来自 packages.ros.org的软件包。 ROS Jade 仅 支持Trusty (14.04)、Utopic (14.10) 和 Vivid (15.04)。
   本机为14.04版本
   
   ```
   sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
   ```
   可以使用国内镜像源，这样会快很多
   （中山大学）
   ```
sudo sh -c '. /etc/lsb-release && echo "deb http://mirror.sysu.edu.cn/ros/ubuntu/ $DISTRIB_CODENAME main" > /etc/apt/sources.list.d/ros-latest.list'
```
3. 添加keys

    ```
    sudo apt-key adv --keyserver hkp://pool.sks-keyservers.net --recv-key 0xB01FA116
    ```
    
4. 安装
* 首先，确保Debian软件包索引是最新的
  ```
  sudo apt-get update
  ```
  
* 本机使用ubuntu14.04，安装桌面完整版：包含ROS、rqt、rviz、通用机器人函数库、2D/3D仿真器、导航以及2D/3D感知功能。 
   ```
   sudo apt-get install ros-jade-desktop-full
   ```
5. 初始化rosdep
   在开始使用ROS之前还需要初始化rosdep。  rosdep可以方便在需要编译某些源码的时候为其安装一些系统依赖，同时也是某些ROS核心功能组件所必需用到的工具。 
   ```
   sudo rosdep init
   rosdep update
   
   ```
6. 环境配置
   如果每次打开一个新的终端时ROS环境变量都能够自动配置好（即添加到bash会话中），那将会方便很多： 
   ```
   echo "source /opt/ros/jade/setup.bash" >> ~/.bashrc
source ~/.bashrc

   ```
7. 安装rosinstall
    rosinstall 是ROS中一个独立分开的常用命令行工具，它可以方便让你通过一条命令就可以给某个ROS软件包下载很多源码树。 
 要在ubuntu上安装这个工具，运行以下指令
 ```
 sudo apt-get install python-rosinstall
 
 ```
8. Build farm 状态
安装的各种软件包都是通过ROS build farm来编译构建的，可以在[这里](http://repositories.ros.org/status_page/ros_jade_default.html)查看每个软件包的编译状态

#### 三、实验感想
本次实验是ROS的安装，基本上按照官网的安装步骤走，比较简单，主要是熟悉了一下ROS，一套框架，底层提供硬件驱动，软件层面支持通用的文件格式，我们以后主要会用它的仿真功能

