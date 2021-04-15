# Xavier_environment

## Install ROS
 - Refer
    - ROS web: http://wiki.ros.org/cn/melodic/Installation/Ubuntu
 - Install
   ```
   sudo sh -c '. /etc/lsb-release && echo "deb http://mirrors.tuna.tsinghua.edu.cn/ros/ubuntu/ `lsb_release -cs` main" > /etc/apt/sources.list.d/ros-latest.list'
   
   sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
   
   sudo apt update
   
   sudo apt install ros-melodic-desktop-full
   
   sudo rosdep init # Errors occur: command not found
   sudo apt install rospack-tools
   
   sudo rosdep init # Errors occur: Website may be down
   sudo chmod a+rw /etc/hosts
   gedit /etc/hosts # Add: 151.101.84.133      raw.githubusercontent.com
   
   sudo rosdep init
   rosdep update # Errors occur: unable to process source, doesn't matter
   
   echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
   source ~/.bashrc
   ```
   
## Install cuda
 - Check Jetson AGX Xavier info
   ```
   # Check architecture
   uname -m
   
   # Check version of Linux
   cat /etc/*release
   
   # Check JetPack info
   sudo pip3 install jetson-stats
   sudo jtop
   ```
 - Download installation packages
   ```
   # In another pc, download installation packages using 'NVIDIA SDK Manager'
   
   # Visit https://developer.nvidia.com and sign in
   # Click 'JetPack'(in Popular SDKs) -> 'FOR ANY JETSON DEVELOPER KIT'(in Installing JetPack) -> 'Download NVIDIA SDK Manager' and get more information by following 'Install Jetson Software with SDK Manager'
   
   # install SDK Manager
   sudo dpkg -i XXX.deb
   sdkmanager
   
   # Download packages that Xavier needs
   #   In STEP 01: Jetson -> Host Machine -> Target Hardware Jetson AGX Xavier -> Linux JetPack 4.4 -> CONTINUE
   #   In STEP 02: I accept ... -> Download now. Install later -> CONTINUE
   #   In STEP 03: Download packages
   #   In STEP 04: Finish
   
   # Next, copy three packages from downloading dir into Xavier, these packages are:
   # 'cuda-repo-l4t-10-2-local-10.2.89_1.0-1_arm64.deb'
   # 'libcudnn8_8.0.0.180-1+cuda10.2_arm64.deb'
   # 'libcudnn8-dev_8.0.0.180-1+cuda10.2_arm64.deb'
   ```
 - Install
   ```
   # In Xavier, run installation commands
   
   sudo dpkg -i cuda-repo-l4t-10-2-local-10.2.89_1.0-1_arm64.deb
   sudo dpkg -i libcudnn8_8.0.0.180-1+cuda10.2_arm64.deb
   sudo dpkg -i libcudnn8-dev_8.0.0.180-1+cuda10.2_arm64.deb
   
   sudo apt update
   sudo apt install cuda-toolkit-10-2
   ```
   
## Install Pytorch & torchvision
 - Refer
    - Check Jetson info: http://www.gpus.cn/gpus_list_page_techno_support_content?id=39
    - Download PyTorch installation package: https://elinux.org/Jetson_Zoo#PyTorch_.28Caffe2.29
    - Pytorch web: https://github.com/pytorch/pytorch
    - torchvision web: https://github.com/pytorch/vision
 - Install
   ```
   # Download Pytorch package according to Jetson info, for example, JetPack 4.4 -> Pytorch v1.6.0 -> torchvision 0.7.0
   
   sudo apt-get install libopenblas-base libopenmpi-dev
   sudo apt-get install python3-pip
   pip3 install Cython
   pip3 install numpy torch-1.6.0-cp36-cp36m-linux_aarch64.whl
   
   git clone --branch v0.7.0 https://github.com/pytorch/vision.git torchvision
   cd torchvision
   pip3 install setuptools
   sudo python3 setup.py install
   ```
 - Test
   ```
   python3
   import torch # no error appears
   print(torch.__version__) # get 1.6.0
   print(torch.cuda.is_available()) # get Ture
   import torchvision # no error appears
   ```
 - Uninstall if necessary
   ```
   sudo pip3 uninstall torch
   sudo pip3 uninstall torchvision
   ```
 
 
 
 
 
 
 

