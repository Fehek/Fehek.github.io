---
title: 树莓派搭建 OpenCV 3 环境
categories: 树莓派
abbrlink: aee93aa1
tags:
  - Raspberry Pi
  - OpenCV
date: 2020-08-02 11:21:00
index_img: /image/index/raspberry.png
banner_img: /image/banner/raspberry.jpg
---

# 下载相关工具及包
+ 安装OpenCV相关工具
```
sudo apt install build-essential cmake git pkg-config libgtk-3-dev libcanberra-gtk*
sudo apt install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev libxvidcore-dev libx264-dev
sudo apt install libjpeg-dev libpng-dev libtiff-dev gfortran openexr libatlas-base-dev opencl-headers
sudo apt install python3-dev python3-numpy libtbb2 libtbb-dev libdc1394-22-dev
````

+ 创建一个新目录并从 Github 克隆 OpenCV 和 OpenCV contrib 存储库
```
mkdir ~/opencv_build
cd ~/opencv_build
git clone https://github.com/opencv/opencv.git
git clone https://github.com/opencv/opencv_contrib.git
```

+ 创建一个临时构建目录，然后切换到该目录
```
mkdir -p ~/opencv_build/opencv/build
cd ~/opencv_build/opencv/build
```

# 编译
+ 设置编译参数，"\\" 代表将代码延续到下一行
```
cmake -D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D INSTALL_C_EXAMPLES=OFF \
    -D INSTALL_PYTHON_EXAMPLES=OFF \
    -D OPENCV_GENERATE_PKGCONFIG=ON \
    -D ENABLE_NEON=ON \
    -D ENABLE_VFPV3=ON \
    -D BUILD_TESTS=OFF \
    -D OPENCV_ENABLE_NONFREE=ON \
    -D OPENCV_EXTRA_MODULES_PATH=~/opencv_build/opencv_contrib/modules \
    -D BUILD_EXAMPLES=OFF ..
```

+ 输出结果
```
...
-- Configuring done
-- Generating done
-- Build files have been written to: /home/pi/opencv_build/opencv/build
```

+ 开始编译
```
make -j4
```
花费时间很长，请耐心等待
若在途中失败，再次运行该命令，会从停止的位置继续

+ 编译中出现问题

提示缺少boostdesc_bgm.i文件，将此[文件](https://pcs.baidu.com/rest/2.0/pcs/file?method=download&app_id=778750&filename=boostdesc_bgm.i%E7%AD%89.zip&path=%2Fshare%2Fboostdesc_bgm.i%E7%AD%89.zip&filename=boostdesc_bgm.i%E7%AD%89.zip)拷贝到opencv_contrib/modules/xfeatures2d/src/目录下

+ 结束后会出现
```
...
[100%] Linking CXX shared module ../../lib/python3/cv2.cpython-35m-arm-linux-gnueabihf.so
[100%] Built target opencv_python3
```

# 验证成功
+ C++ 库
```
pkg-config --modversion opencv4
```
+ Python 库
```
python3 -c "import cv2; print(cv2.__version__)"
```
若成功安装，会输出版本号