# Xavier_environment

## ROS
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
   
## Pytorch & torchvision
 - First, try to install by downloading installation package
 - Refer
    - Check Jetson info: http://www.gpus.cn/gpus_list_page_techno_support_content?id=39
    - Download Pytorch package: https://elinux.org/Jetson_Zoo#PyTorch_.28Caffe2.29
 - Install
   ```
   # Download Pytorch package according to Jetson info, for example, JetPack 4.3 -> Pytorch v1.4.0 -> torchvision 0.5.0
   sudo apt-get install libopenblas-base libopenmpi-dev
   sudo apt-get install python3-pip
   pip3 install Cython
   pip3 install numpy torch-1.4.0-cp36-cp36m-linux_aarch64.whl
   
   git clone --branch v0.5.0 https://github.com/pytorch/vision.git torchvision
   cd torchvision
   pip3 install setuptools
   sudo python3 setup.py install
   ```
   
 - If errors occur: from torch._C import * ImportError, try to install by building from source
 - Refer
    - https://www.ncnynl.com/archives/201903/2901.html
    - https://github.com/pytorch/pytorch
    - https://github.com/pytorch/vision
 - Install
   ```
   # Install PyTorch v1.3.0 & torchvision v0.4.1 in Xavier (JetPack 4.3)
   # Create dir
   mkdir -p ~/PyTorch
   cd ~/PyTorch
   
   # Set max performance
   sudo nvpmodel -m 0
   
   # Download source code
   git clone --recursive --branch v1.3.0 http://github.com/pytorch/pytorch # Failure may occur because of Internet things, don't worry
   cd pytorch
   git submodule sync
   git submodule update --init --recursive # It may take several times to download all the parts
   
   # Configure environment variable
   export USE_NCCL=0
   export USE_DISTRIBUTED=0
   export TORCH_CUDA_ARCH_LIST="5.3;6.2;7.2"
   export PYTORCH_BUILD_NUMBER=1
   export PYTORCH_BUILD_VERSION=1.3.0
   
   # Build and create wheel for python3
   sudo apt-get install python3-pip cmake
   sudo pip3 install -U setuptools
   sudo pip3 install -r requirements.txt
   sudo pip3 install scikit-build
   python3 setup.py bdist_wheel
   
   # Install whell
   cd ~/PyTorch/pytorch/dist
   pip3 install torch-1.3.0-cp36-cp36m-linux_aarch64.whl --user
   
   # Test torch
   python3
   import torch # No error appears
   print(torch.__version__)
   
   # Install torchvision
   cd ~/PyTorch
   git clone https://github.com/pytorch/vision
   cd vision
   git checkout v0.4.1
   sudo python3 setup.py install
   ```
 
 
 
 
 
 
 

