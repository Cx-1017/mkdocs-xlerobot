# XLeRobot 项目文档

!!! info "欢迎"
欢迎来到 XLeRobot 项目文档！这是一个实用的双臂移动家庭机器人项目，成本仅需 $660（约 4000 元人民币）。

## 🤖 项目简介

XLeRobot 是一个基于开源技术构建的双臂移动家庭机器人，集成了多项先进技术：

- **🦾 双臂操作** - 灵活的机械臂设计
- **📱 移动底盘** - 自主导航能力
- **👁️ 计算机视觉** - YOLO11 目标检测
- **🧠 深度学习** - LeRobot 框架支持
- **💰 低成本** - 仅需约 4000 元成本

## ✨ 主要特性

| 特性         | 描述                               |
| ------------ | ---------------------------------- |
| **硬件平台** | 树莓派 + ST3215 舵机               |
| **视觉系统** | OpenCV + YOLO11n + Intel RealSense |
| **控制方式** | 遥操作 + 自主控制                  |
| **通讯方式** | USB 串口 + TCP/ZMQ WiFi            |
| **开发语言** | Python 为主                        |

## 🚀 快速开始

### 1. 克隆项目

```bash
git clone https://github.com/Vector-Wangel/XLeRobot.git
cd XLeRobot
```

### 2. 环境配置

```bash
# 创建 conda 环境
conda create -n xlerobot python=3.10
conda activate xlerobot

# 安装依赖
pip install -r requirements.txt
```

### 3. 运行示例

```bash
# 启动机械臂控制
python examples/arm_control.py

# 启动视觉检测
python examples/vision_detect.py
```

## 📚 文档导航

<div class="grid cards" markdown>

- :material-book-open-variant: **项目概述**

  ***

  了解项目背景、技术架构和相关资源

  [:octicons-arrow-right-24: 查看详情](overview/introduction.md)

- :material-school: **学习资源**

  ***

  Python、3D 建模、树莓派、视觉、电机等学习资料

  [:octicons-arrow-right-24: 开始学习](learning/python.md)

- :material-chip: **硬件配置**

  ***

  硬件清单、组装指南和调试说明

  [:octicons-arrow-right-24: 硬件文档](hardware/components.md)

- :material-cog: **软件配置**

  ***

  环境搭建、依赖安装和配置指南

  [:octicons-arrow-right-24: 软件文档](software/environment.md)

</div>

## 🎥 演示视频

<div class="video-container" markdown>

[:fontawesome-brands-bilibili: XLeRobot 家务展示](https://www.bilibili.com/video/BV1bbaFzLEga){ .md-button .md-button--primary }
[:fontawesome-brands-bilibili: 机械臂教程](https://www.bilibili.com/video/BV1i2bazGEHo){ .md-button }

</div>

## 🔗 相关链接

- **GitHub**: [Vector-Wangel/XLeRobot](https://github.com/Vector-Wangel/XLeRobot)
- **LeRobot**: [huggingface/lerobot](https://github.com/huggingface/lerobot)
- **中文社区**: [WowRobo Wiki](https://wiki.wowrobo.com/)

## 📊 项目状态

!!! success "活跃开发中"
项目正在积极开发和维护中，欢迎贡献代码和提出建议！

---

<div align="center">

**如果这个项目对你有帮助，请给我们一个 ⭐ Star！**

Made with :heart: by XLeRobot Team

</div>
