# XLeRobot 项目文档

基于 MkDocs Material 构建的 XLeRobot 项目分析文档。

## 🚀 快速开始

### 安装依赖

```bash
# 创建虚拟环境
conda create -n mkdocs python=3.11
conda activate mkdocs

# 安装依赖
pip install -r requirements.txt
```

### 本地预览

```bash
# 启动开发服务器（支持热重载）
mkdocs serve

# 访问 http://127.0.0.1:8000
```

## 📦 部署到 GitHub Pages

### 方法 1: 使用 mkdocs 命令（推荐）

```bash
# 一键部署到 gh-pages 分支
mkdocs gh-deploy --force
```

该命令会自动：

1. 构建网站到 `site/` 目录
2. 创建/更新 `gh-pages` 分支
3. 推送到 GitHub
4. 网站将发布到: https://cx-1017.github.io/mkdocs-xlerobot/

## � 文档内容

文档采用模块化组织结构：

### 📋 核心文档

- **首页** (`index.md`) - 项目概览和快速导航
- **完整项目分析** (`XLeRobot项目分析.md`) - 完整的项目技术分析

### 📚 学习模块

- **相关链接** (`link/link.md`) - 项目 GitHub 仓库、B 站视频教程等资源链接
- **Python 语法** (`python_learn/python_learn.md`) - Python 编程语法和最佳实践
- **3D 建模 SolidWork** (`3D_SolidWork/3D.md`) - SolidWorks 3D 建模教程
- **树莓派 & Linux** (`Pi & Linux/pi&Linux.md`) - 树莓派配置、Linux 命令、系统设置
- **计算机视觉** (`Camera/Camera.md`) - YOLO、OpenCV、RealSense 深度相机
- **电机 & 机器人** (`Motor_Robot/Motor_Robot.md`) - ST3215 舵机、运动学、机器人控制
- **通讯** (`Communication/Communication.md`) - 串口通讯、TCP、ZMQ、Socket 编程
- **模型学习** (`Model_Learn/Model_learn.md`) - MuJoCo、ManiSkill、SAPIEN 仿真环境

### 🛠️ 实践指南

- **硬件与命令** (`hardware_and_command/hardware_and_command.md`) - 硬件配置与控制命令
- **远程控制调试** (`remote_control/remote_control.md`) - PC 端与树莓派端远程控制完整流程
