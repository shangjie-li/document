注：本文档主要描述Win10中安装Ubuntu16.04及ROS的过程


（一）安装Ubuntu16.04
参考：
https://www.cnblogs.com/Duane/p/5424218.html
https://blog.csdn.net/weixin_40787712/article/details/89944144


（二）安装ROS Kinect
参考：
http://wiki.ros.org/cn/kinetic/Installation/Ubuntu

软件更新设置：
（1）Ubuntu软件：
mian、universe、restricted、multiverse和源代码前面打勾。
下载服务器选择中国清华网mirrors.tuna.tsinghua.edu.cn。
（2）其他软件：
该页面显示的四个选项全部勾选。
关闭时选择更新。

Terminal终端（Ctrl+Alt+T）：
（1）
```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```

（2）
```
sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116
```

（3）
```
sudo apt-get update
```

（4）
```
sudo apt-get install ros-kinetic-desktop-full
apt-cache search ros-kinetic
```

（5）
```
sudo c_rehash /etc/ssl/certs
sudo -E rosdep init
sudo rosdep init
rosdep update
```

（7）
```
echo ''source /opt/ros/kinetic/setup.bash'' >> ~/.bashrc
source ~/.bashrc
```

（8）
```
sudo apt-get install python-rosinstall python-rosinstall-generator python-wstool buid-essential
sudo apt-get update
sudo apt-get install
```

（9）
测试（roscore节点管理器）。一旦启动roscore后，便可以运行ROS程序了。

（10）
如果需要的话，可通过sudo gedit ~/.bashrc修改.bashrc中的内容。

安装libpcap-dev：
```
sudo apt-get install libpcap-dev
```


（三）安装Velodyne驱动与运行
1.安装驱动
```
sudo apt-get install ros-kinetic-velodyne
```

2.创建工作空间
```
mkdir -p ~/velodyne_ws/src
cd velodyne_ws/src/
git clone https://github.com/ros-drivers/velodyne.git
```

3.编译
```
cd ..
rosdep install --from-paths src --ignore-src --rosdistro kinetic -y
catkin_make
```

4.运行节点
```
source devel/setup.bash
roslaunch velodyne_pointcloud VLP16_points.launch
```

5.查看结果
```
rviz
```

6.rviz设置
```
frame：velodyne
PointCloud2的topic：/velodyne_points
```


（四）其他
安装python3：
检查已安装的python版本：
```
python --version
python3 --version
```
如果需要安装python3：
```
sudo add-apt-repository ppa:fkrull/deadsnakes
sudo apt-get update
sudo apt-get install python3.5
```

安装pip：
检查已安装的pip版本：
```
pip --version
pip3 --version
```
如果需要安装pip3：
```
sudo apt-get install python3-pip
```

