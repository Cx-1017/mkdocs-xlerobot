# XLeRobot 项目文档

基于 MkDocs Material 构建的 XLeRobot 机器人项目技术文档站点。

## 📖 项目简介

XLeRobot 是一个机器人学习与控制项目，本仓库为项目的完整技术文档系统，涵盖机器人硬件设计、软件开发、视觉算法、通信协议、强化学习等全方位内容。文档采用 MkDocs Material 主题构建，支持中文搜索、代码高亮、夜间模式等现代化特性。

## 🌐 在线访问

- 📚 文档站点: [https://cx-1017.github.io/mkdocs-xlerobot/](https://cx-1017.github.io/mkdocs-xlerobot/)
- 💻 项目仓库: [https://github.com/Vector-Wangel/XLeRobot](https://github.com/Vector-Wangel/XLeRobot)

## 🚀 快速开始

### 环境准备

```bash
# 创建虚拟环境（推荐）
conda create -n mkdocs python=3.11
conda activate mkdocs

# 或使用 venv
python -m venv venv
source venv/bin/activate  # Linux/Mac
# venv\Scripts\activate   # Windows
```

### 安装依赖

```bash
pip install -r requirements.txt
```

主要依赖包括：
- `mkdocs` - 静态站点生成器
- `mkdocs-material` - Material Design 主题
- `mkdocs-git-revision-date-localized-plugin` - Git 修订日期插件
- `markdown-callouts` - Markdown 标注扩展

### 本地预览

```bash
# 启动开发服务器（支持热重载）
mkdocs serve

# 访问 http://127.0.0.1:8000
```

### 构建静态网站

```bash
# 生成静态 HTML 文件到 site/ 目录
mkdocs build
```

## 📦 部署到 GitHub Pages

### 一键部署（推荐）

```bash
# 自动构建并部署到 gh-pages 分支
mkdocs gh-deploy --force
```

该命令会自动：
1. 构建网站到 `site/` 目录
2. 创建/更新 `gh-pages` 分支
3. 推送到 GitHub 远程仓库
4. 网站将在几分钟内发布

## 📚 文档结构

### 目录组织

```
documents/
├── mkdocs.yml              # MkDocs 配置文件
├── requirements.txt        # Python 依赖包
├── README.md              # 本说明文档
├── docs/                  # 文档源文件目录
│   ├── index.md          # 首页
│   ├── 3D_SolidWork/     # 3D 建模与 SolidWorks 教程
│   ├── Attention/        # 注意事项与最佳实践
│   ├── Camera/           # 计算机视觉与相机
│   ├── Communication/    # 通讯协议与网络编程
│   ├── hardware_and_command/  # 硬件连接与控制命令
│   ├── Model_Learn/      # 强化学习与仿真环境
│   ├── Motor_Robot/      # 电机控制与机器人学
│   ├── Pi & Linux/       # 树莓派与 Linux 系统
│   ├── python_learn/     # Python 编程基础
│   ├── remote_control/   # 远程控制与监控
│   ├── link/            # 相关资源链接
│   ├── image/           # 文档图片资源
│   └── stylesheets/     # 自定义样式表
└── site/                 # 构建生成的静态网站（自动生成）
```

### 📋 文档内容导航

#### 🏠 首页
- **首页** (`index.md`) - 项目概览与快速导航

#### 🔗 资源链接
- **相关链接** (`link/link.md`) - GitHub 仓库、B 站视频教程、相关资源

#### 📚 学习资源
1. **Python语法** (`python_learn/python_learn.md`)
   - Python 编程基础、语法特性、最佳实践

2. **3D建模 SolidWork** (`3D_SolidWork/3D.md`)
   - SolidWorks 3D 建模教程、机器人结构设计

3. **树莓派 & Linux** (`Pi & Linux/pi&Linux.md`)
   - 树莓派配置、Linux 命令、系统管理

4. **计算机视觉** (`Camera/Camera.md`)
   - YOLO 目标检测、OpenCV 图像处理、RealSense 深度相机、Orbbec 奥菲中光 gemini 2

5. **电机 & 机器人** (`Motor_Robot/Motor_Robot.md`)
   - ST3215 舵机控制、运动学分析、机器人控制算法

6. **通讯** (`Communication/Communication.md`)
   - 串口通信、TCP/IP、ZMQ、Socket 网络编程

7. **强化学习** (`Model_Learn/Model_learn.md`)
   - MuJoCo 物理引擎、ManiSkill2 仿真、SAPIEN 环境

#### ⚠️ 重要提示
- **注意事项** (`Attention/Attention.md`) - 开发过程中的重要注意事项与常见问题

#### 🛠️ 实践指南
- **硬件连接与常用命令** (`hardware_and_command/hardware_and_command.md`)
  - 硬件连接配置、控制命令参考、接口说明

- **远程控制与监控** (`remote_control/remote_control.md`)
  - PC 端与树莓派远程控制流程、调试方法

## 🐛 问题反馈

如遇到文档问题或有改进建议，请：
- 提交 [GitHub Issue](https://github.com/Vector-Wangel/XLeRobot/issues)
- 或直接提交 Pull Request

## 📄 许可证

Copyright © 2025 XLeRobot Project

---

**最后更新**: 2026年2月1日
