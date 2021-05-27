# Anaconda

 - 安装Anaconda
   ```
   下载安装包https://www.anaconda.com/products/individual#Downloads
   bash Anaconda3-2020.11-Linux-x86_64.sh
   ```
 - 创建yolact-env环境
   ```
   conda create -n yolact-env python=3.6.9
   conda activate yolact-env
   
   conda install pytorch==1.5.1 torchvision==0.6.1 cudatoolkit=10.2 -c pytorch
   pip install cython
   pip install opencv-python pillow pycocotools matplotlib
   ```
 - 切换Anaconda环境
   ```
   conda activate yolact-env # 进入yolact-env虚拟环境
   conda deactivate # 退回root环境
   ```
