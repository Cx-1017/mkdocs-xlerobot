---
author: cx-1017
title: XLeRobot项目分析
time: 2025-09-07 周日
rating: '10'
tags:
  - ROS
  - 电机
  - 视觉
  - YOLO
  - 深度学习
  - 3D建模
  - 树莓派
banner: '[[total_file/Banner folder/732122b7e3c71850644878b28cdc4df.jpg]]'
banner_y: 0.735
banner_lock: true
---

# 一、项目简介

## 1.1 项目概述

XLeRobot 是一个实用的双臂移动家用机器人项目，成本约 660 美元（约 4000-5000 元人民币）。该项目基于 HuggingFace 的 LeRobot 框架开发，旨在通过端到端学习让 AI 机器人技术更加普及和易用。

![XLeRobot 示意图](image/Pasted%20image%2020251006154201.png)

## 1.2 官方资源

### 项目仓库

- **XLeRobot 仓库**：[Vector-Wangel/XLeRobot](https://github.com/Vector-Wangel/XLeRobot/tree/main)
- **LeRobot 仓库**：[huggingface/lerobot](https://github.com/huggingface/lerobot)
- **LeRobot HuggingFace**：[lerobot](https://huggingface.co/lerobot)
- **LeRobot 中文社区**：[WowRobo Wiki](https://wiki.wowrobo.com/)

### 演示视频

- [XLeRobot 家务展示（4千成本）](https://www.bilibili.com/video/BV1bbaFzLEga)
- [新版 Lerobot SO101 机械臂教程：机械臂校准及遥操作](https://www.bilibili.com/video/BV1i2bazGEHo)

### 社区项目

- [双臂远程遥操和底盘控制项目](https://github.com/brucecai-2001/lerobot_piper)
- [相关演示视频](https://www.bilibili.com/video/BV1ooGWzyE8a/)

# 二、技术架构

## 2.1 硬件组成

XLeRobot 的硬件系统主要包括：

- **双臂机械臂**：基于 SO101 设计，每臂多自由度
- **移动底盘**：LeKiwi 底盘，提供移动能力
- **视觉系统**：摄像头 + 深度相机（Intel RealSense D415）
- **控制系统**：树莓派 5 / 3B+
- **执行机构**：ST3215 UART 串行总线舵机

## 2.2 软件架构

### 运动控制系统

- 电机驱动：`src/lerobot/motors/feetech` 目录下的 `FeetechMotorBus` 类
- 运动学逆解：支持 2 关节机械臂运动学逆解
  ![运动学逆解示意](image/Pasted%20image%2020251014170648.png)

### 视觉系统

- **目标检测**：YOLO11n 模型（ultralytics 库）
- **图像处理**：OpenCV
- **深度感知**：Intel RealSense D415（可选，用于 VR 功能）

### 通讯系统

- **串口通讯**：树莓派通过 USB 串口控制舵机（/dev/ttyUSB*）
- **网络通讯**：TCP + ZMQ 实现 WiFi 无线控制
  - 支持两主两从远程遥操作
  - Socket 编程实现设备间通信

### 强化学习与仿真

- **物理仿真**：MuJoCo
- **场景仿真**：ManiSkill3 + SAPIEN
- **训练框架**：基于 LeRobot 的端到端学习

![仿真环境示例](image/Pasted%20image%2020251014165959.png)

## 2.3 核心技术栈

| 技术领域 | 使用技术 |
|---------|---------|
| 电机控制 | ST3215 舵机，UART 串行总线 |
| 视觉识别 | YOLO11n + OpenCV |
| 通讯协议 | 串口 + TCP/IP + ZMQ |
| 仿真平台 | MuJoCo + ManiSkill3 + SAPIEN |
| 硬件平台 | 树莓派 5 / 3B+ |
| 操作系统 | Ubuntu 20.04 LTS |
| 编程语言 | Python |
| 3D 建模 | SolidWorks |
| 深度学习 | PyTorch + HuggingFace |

## 2.4 关键技术特性

### ST3215 舵机

- 高转速 20KG.CM 扭矩
- 360° 磁编码器
- UART 串行总线通信
- 项目路径：`src/lerobot/motors/feetech`

### 机械臂运动学

- 支持 2 关节运动学逆解
- 手写笔记和代码实现在 GitHub 仓库中

### 仿真环境操作

基本操作：
- `WASD` 键：前后左右移动
- 鼠标右键：调整视角
- 鼠标滚轮：缩放视图
- Camera 切换：可切换到头部或手臂视角

# 三、项目文档

详细的硬件连接、软件配置和使用说明请参考：

- **中文文档**：`docs/zh/source/` 目录
- **软件配置**：`docs/zh/source/software/index.md`
- **仿真入门**：`docs/zh/source/simulation/getting_started/index.md`
- **技术路线**：查看仓库根目录的相关文档

---

**注意**：本文档仅包含 XLeRobot 项目的核心信息和技术架构概述。详细的技术教程、环境配置步骤和学习资源请参考项目 GitHub 仓库中的完整文档。
