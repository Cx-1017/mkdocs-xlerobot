# 计算机视觉

## YOLO

项目中使用了 yolo11n 模型，相关 python 库为：[ultralytics/ultralytics: Ultralytics YOLO 🚀](https://github.com/ultralytics/ultralytics)
相关 API 的使用：[Python 用法 - Ultralytics YOLO 文档](https://docs.ultralytics.com/zh/usage/python/)
Yolo 模型训练与使用建议教程：[【yolo 全系列教程，2025 年 8 月最新】](https://www.bilibili.com/video/BV1QKh6zbE42?vd_source=b313f11521a9a14487c38aa4fa1c5066)标定数据集网页版：[Make Sense](https://www.makesense.ai/)
Yolo 环境配置：[Yolov11 环境配置](https://xiaolian.blog.csdn.net/article/details/143270109?fromshare=blogdetail&sharetype=blogdetail&sharerId=143270109&sharerefer=PC&sharesource=m0_74029092&sharefrom=from_link)

## OpenCV

[【【计算机视觉实战项目】基于 Python 与 OpenCV 实现的图像处理全套解析！从基础原理到代码实战，全程通俗易懂，适合所有零基础入门学习！-ML/DL/CV】](https://www.bilibili.com/video/BV14A411C7ZE?vd_source=b313f11521a9a14487c38aa4fa1c5066)

## 深度相机(应该是项目后期 VR 需要使用)

[CSDN 博客-配置相关内容](https://blog.csdn.net/weixin_41628708/article/details/139098755?fromshare=blogdetail&sharetype=blogdetail&sharerId=139098755&sharerefer=PC&sharesource=m0_74029092&sharefrom=from_link)

Intel RealSense D415：[github SDK](https://github.com/IntelRealSense/librealsense/releases)

官网资料：[环境配置](https://dev.intelrealsense.com/docs/supported-platforms-and-languages?_ga=2.156964472.291999339.1661862516-1920419538.1609727088)

## 奥菲中光Orbbec SDK配置

官方链接：
developer.orbbec.com.cn/develop_details.html?id=1
github.com/orbbec/OrbbecSDK/releases

推荐配置链接：
可视化工具sdk:https://github.com/orbbec/OrbbecSDK_v2
python sdk:https://wiki.seeedstudio.com/cn/yolov11_with_depth_camera/

> [Note!]
> 其中在配置python sdk时，进行到Cmake步骤可能会出现报错，因为lerobot的环境为Python3.10，而教程给的是3.8，所以你可以通过在终端输入以下命令完成Cmake步骤：

```bash

# 清理并重新配置
rm -rf build
mkdir build && cd build


# 获取 conda 环境中 Python 的路径
python_path=$(which python)
python_dir=$(dirname $(dirname $python_path))

# 使用这些路径配置 CMake
cmake \
  -Dpybind11_DIR=`pybind11-config --cmakedir` \
  -DPython3_EXECUTABLE=$python_path \
  -DPython3_INCLUDE_DIR=$python_dir/include/python3.10 \
  -DPython3_LIBRARY=$python_dir/lib/libpython3.10.so \
  ..

# 编译
make -j4
make install
```
