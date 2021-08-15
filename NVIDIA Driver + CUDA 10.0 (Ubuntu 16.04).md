# NVIDIA Driver + CUDA 10.0 (Ubuntu 16.04)

注：本文档主要描述Ubuntu 16.04中安装NVIDIA Driver和cuda的过程

 - 查看显卡驱动
   ```
   nvidia-smi
   ```
 - 查看cuda版本
   ```
   cat /usr/local/cuda/version.txt
   nvcc -V
   ```
 - 查看cudnn版本
   ```
   cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2
   ```
   
## 安装NVIDIA Driver
 - 下载驱动
    - https://www.nvidia.cn/geforce/drivers
    - 下载NVIDIA-Linux-x86_64-440.100.run文件，大小为144MB
 - 安装
   ```
   # 1.禁用nouveau
   sudo gedit /etc/modprobe.d/blacklist.conf
   # 打开文件，在最后添加如下两行
   blacklist nouveau
   options nouveau modeset=0
   
   # 2.更新系统修改
   sudo update-initramfs -u
   # 重启电脑
   
   # 3.验证nouveau已禁用
   lsmod | grep nouveau
   # 无输出则表示已禁用
   
   # 4.按ctrl+alt+f1进入命令行界面，此时需要login（电脑账户名称），password（密码）
   
   # 5.关闭图形界面
   sudo service lightdm stop
   
   # 6.卸载系统中存在的驱动
   sudo apt-get remove nvidia-*
   
   # 7.运行.run文件
   sudo chmod  a+x NVIDIA-Linux-x86_64-440.100.run
   sudo ./NVIDIA-Linux-x86_64-440.100.run -no-x-check -no-nouveau-check -no-opengl-files
   # 在弹出的提示窗口中均保持默认选择
   
   # 重启图形界面
   sudo service lightdm start
   ```
 - 验证安装成功
   ```
   nvidia-smi
   ```

## 安装cuda 10.0
 - 安装前提
    - 已安装显卡驱动
 - 下载驱动
    - https://developer.nvidia.com/cuda-toolkit-archive
    - （CUDA Toolkit 10.0 Archive  ->  Linux  ->  x86_64  ->  Ubuntu  ->  16.04  ->  runfile（local））
    - 下载cuda_10.0.130_410.48_linux.run文件，大小为2.0GB
   ```
   sudo sh cuda_10.0.130_410.48_linux.run
   ```
 - 如果/tmp显示空间不足，则执行
   ```
   sudo mkdir /opt/tmp
   sudo sh cuda_10.0.130_410.48_linux.run --tmpdir=/opt/tmp/
   ```
 - 安装
   ```
   # Do you accept the previously read EULA?
   # accept/decline/quit: accept
 
   # Install NVIDIA Accelerated Graphics Driver for Linux-x86_64 361.62?
   # (y)es/(n)o/(q)uit: n
 
   # Install the CUDA 10.0 Toolkit?
   # (y)es/(n)o/(q)uit: y
 
   # Enter Toolkit Location
   # [ default is /usr/local/cuda-10.0 ]:
 
   # Do you want to install a symbolic link at /usr/local/cuda?
   # (y)es/(n)o/(q)uit: y
 
   # Install the CUDA 10.0 Samples?
   # (y)es/(n)o/(q)uit: y
 
   # Enter CUDA Samples Location
   # [ default is /home/lishangjie ]:
   ```
 - 安装结束后
   ```
   sudo gedit ~/.bashrc
   ```
 - 在文本最后添加
   ```
   export PATH=/usr/local/cuda-10.0/bin:$PATH
   export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
   ```
 - 保存退出
   ```
   source ~/.bashrc
   ```
 - 验证安装成功
   ```
   cd /usr/local/cuda-10.0/samples/1_Utilities/deviceQuery
   sudo make
   ./deviceQuery
   # 不报错即证明安装成功
   ```
 - 查看cuda版本（CUDA Version 10.0.130）
   ```
   cat /usr/local/cuda-10.0/version.txt
   ```
   
## 安装cudnn 7.6.5
 - 安装前提
    - 已安装cuda
 - 下载驱动（首次登录需注册并填问卷）
    - https://developer.nvidia.com/rdp/cudnn-archive
    - （Download cuDNN v7.6.5 (November 5th, 2019), for CUDA 10.0  ->  cuDNN Library for Linux）
    - 下载cudnn-10.0-linux-x64-v7.6.5.32.tgz压缩包，大小为486MB
 - 解压后在/home下生成cuda文件夹
   ```
   sudo cp cuda/include/cudnn.h /usr/local/cuda/include
   sudo cp cuda/lib64/*.* /usr/local/cuda/lib64
   sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
   ```
 - libcudnn下载（可能不需要）
    - https://developer.nvidia.com/rdp/cudnn-archive
    - （Download cuDNN v7.6.5 (November 5th, 2019), for CUDA 10.0  ->  cuDNN Runtime Library for Ubuntu16.04 (Deb) & cuDNN Developer Library for Ubuntu16.04 (Deb)）
    - 下载libcudnn7_7.6.5.32-1+cuda10.0_amd64.deb和libcudnn7-dev_7.6.5.32-1+cuda10.0_amd64.deb
   ```
   sudo dpkg -i libcudnn7_7.6.5.32-1+cuda10.0_amd64.deb
   sudo dpkg -i libcudnn7-dev_7.6.5.32-1+cuda10.0_amd64.deb

   sudo apt-get update
   sudo apt-get install libcudnn7
   ```
 - 查看cudnn版本（CUDNN_VERSION 7.6.5）
   ```
   cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2
   ```

## 切换网络源
 - 打开软件和更新，设置选择阿里的服务器
    - http://mirrors.aliyun.com/ubuntu
    - 这时候点击关闭，重新载入时会报错，这是因为sources.list里面对阿里这个源配置的这个artful属性不对。
   ```
   sudo gedit /etc/apt/sources.list
   ```
 - 删除文档内容，并添加
   ```
   deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe 
   deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe
   deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe
   deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe
   deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe
   ```

