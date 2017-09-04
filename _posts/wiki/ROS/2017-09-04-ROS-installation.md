---
title: Ubuntu安装ROS和基本概念
layout: page
category: category
---

```bash
sudo sh -c '. /etc/lsb-release && echo "deb http://mirrors.ustc.edu.cn/ros/ubuntu/ $DISTRIB_CODENAME main" > /etc/apt/sources.list.d/ros-latest.list'
wget http://packages.ros.org/ros.key -O - | sudo apt-key add -
sudo apt-get update
sudo apt-get install ros-fuerte-desktop-full
echo "source /opt/ros/fuerte/setup.bash" >> ~/.bashrc
. ~/.bashrc
sudo apt-get install python-rosinstall python-rosdep
```

## 功能包

package都在$ROS_PACKAGE_PATH里面。

- bin/
- include/package_name/
- msg/
- scripts/
- src/
- srv/
- CMakeLists.txt
- manifest.xml

- rospack
- roscreate-pkg
创建新的package，需要在$ROS_PACKAGE_PATH里面运行，或者把当前目录加到$ROS_PACKAGE_PATH中去。
- rosmake
编译$ROS_PACKAGE_PATH中的package
- rosdep
- roseps

- roscd
- rosed
直接编辑对应的package相关的文件
- roscp
- rosd
- rosls

- rosnode
    * list
    * info
    * type
    * bw

- rxgraph
可视化的实时订阅调试工具

## 消息类型


## 节点

节点可以发布消息也可以接受消息，发布和订阅

服务也可以使节点相互通信
