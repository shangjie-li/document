# CVbridge_in_python3

注：本文档主要描述Python3中使用CVbridge的方法

## 安装kinect驱动包
 - 参考
    - https://github.com/code-iai/iai_kinect2.git
 - 修改
   ```
   sudo gedit /opt/ros/ros-distro/lib/python2.dist-packages/message_filters/__init__.py
   ```
 - 添加
   ```
   from functools import reduce 
   ```

## 解决python3无法直接使用cv_bridge
 - 参考
    - https://cyaninfinite.com/ros-cv-bridge-with-python-3/#Dependencies
   ```
   # python-catkin-tools is needed for catkin tool
   # python3-dev and python3-catkin-pkg-modules is needed to build cv_bridge
   # python3-numpy and python3-yaml is cv_bridge dependencies
   # ros-${ROS_DISTRO}-cv-bridge is needed to install a lot of cv_bridge deps. Probaply you already have it installed
   ```
 - Install dependencies
   ```
   sudo apt-get install python-catkin-tools python3-dev python3-catkin-pkg-modules python3-numpy python3-yaml ros-${ROS_DISTRO}-cv-bridge
   ```
 - Create workspace
   ```
   mkdir catkin_workspace
   cd catkin_workspace
   catkin init
   ```
 - Instruct catkin to set cmake variables
   ```
   catkin config -DPYTHON_EXECUTABLE=/usr/bin/python3 -DPYTHON_INCLUDE_DIR=/usr/include/python3.5m -DPYTHON_LIBRARY=/usr/lib/x86_64-linux-gnu/libpython3.5m.so
   ```
 - Instruct catkin to install built packages into install place. It is $CATKIN_WORKSPACE/install folder
   ```
   catkin config --install
   ```
 - Clone cv_bridge src
   ```
   git clone https://github.com/ros-perception/vision_opencv.git src/vision_opencv
   ```
 - Find version of cv_bridge in your repository. Version: 1.12.8-0xenial-20180416-143935-0800
   ```
   apt-cache show ros-${ROS_DISTRO}-cv-bridge | grep Version
   ```
 - Checkout right version in git repo. In our case it is 1.12.8
   ```
   cd src/vision_opencv/
   git checkout 1.12.8
   cd ../../
   ```
 - Build
   ```
   catkin build cv_bridge
   ```
 - if error occured with *Could not find the following Boost libraries: boost_python3* 
   ```
   cd /usr/lib/x86_64-linux-gnu
   sudo ln -s libboost_python-py35.so libboost_python3.so 
   ```
 - Extend environment with new package 
   ```
   source install/setup.bash --extend
   ```
 - 运行程序之前，source 新编译的cv_bridge所在工作控件，可将其加入.bashrc

## 解决python3与ROS兼容性问题
 - 在要执行的脚本.py第一行添加 #!env /usr/bin/python3
 - 安装依赖
   ```
   python3 -m pip install catkin-tools
   python3 -m pip install rospkg
   ```
