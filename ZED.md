# ZED

注：本文档主要描述安装ZED驱动的过程

## 参考
 - 使用方法
    - http://wiki.ros.org/zed-ros-wrapper
    - https://www.stereolabs.com/docs/ros/
 - CUDA驱动下载
    - https://developer.nvidia.com/cuda-toolkit-archive
 - SDK下载
    - https://www.stereolabs.com/developers/release/
 
## 准备
 - 查看GPU型号（GeForce GTX 1050 Mobile）
   ```
   lspci | grep -i nvidia
   ```
 - 查看NVIDIA驱动版本（找不到命令）
   ```
   sudo pkg --list | grep nvidia-*
   ```
 - 查看NVIDIA显卡使用情况（227MiB / 4042MiB）
   ```
   nvidia-smi
   ```
 - 查看CUDA版本（CUDA Version 10.2.89）
   ```
   cat /usr/local/cuda/version.txt
   ```
   
## 安装
 - 安装CUDA
    - https://developer.nvidia.com/cuda-toolkit-archive
    - （CCUDA Toolkit 10.2 Download  ->  Linux  ->  x86_64  ->  Ubuntu  ->  16.04  ->  deb（local））
   ```
   wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-ubuntu1604.pin

   sudo mv cuda-ubuntu1604.pin /etc/apt/preferences.d/cuda-repository-pin-600

   wget http://developer.download.nvidia.com/compute/cuda/10.2/Prod/local_installers/cuda-repo-ubuntu1604-10-2-local-10.2.89-440.33.01_1.0-1_amd64.deb

   sudo dpkg -i cuda-repo-ubuntu1604-10-2-local-10.2.89-440.33.01_1.0-1_amd64.deb

   sudo apt-key add /var/cuda-repo-10-2-local-10.2.89-440.33.01/7fa2af80.pub

   sudo apt-get update

   sudo apt-get -y install cuda
   ```

 - 安装SDK
    - https://www.stereolabs.com/developers/release/
    - （CUDA 10.2 ZED SDK for Ubuntu 16 3.2.2  ->  ZED_SDK_Ubuntu16_cuda10.2_v3.2.2.run）
    - 使用时下载最新版本

 - 编译
   ```
   cd ~/stereo_ws/src
   git clone https://github.com/stereolabs/zed-ros-wrapper.git
   cd ..
   rosdep install --from-paths src --ignore-src -r -y
   catkin_make -DCMAKE_BUILD_TYPE=Release
   source ./devel/setup.bash
   ```


 - 使用（可能需要重启计算机）
   ```
   roslaunch zed_wrapper zed_pointcloud.launch
   # （自动弹出RVIZ  ->  Add  ->  By topic  ->  /zed/zed_node/point_cloud/cloud_registered）
   ```

