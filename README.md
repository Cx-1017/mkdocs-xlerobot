# XLeRobot 项目文档

基于 MkDocs Material 构建的 XLeRobot 项目分析文档。

## 📁 项目结构

```
documents/
├── docs/                          # 文档源文件
│   ├── index.md                  # 首页
│   ├── XLeRobot项目分析.md        # 完整项目分析
│   ├── link/                     # 相关链接
│   │   └── link.md
│   ├── python_learn/             # Python语法学习
│   │   └── python_learn.md
│   ├── 3D_SolidWork/             # 3D建模教程
│   │   └── 3D.md
│   ├── Pi & Linux/               # 树莓派与Linux
│   │   └── pi&Linux.md
│   ├── Camera/                   # 计算机视觉
│   │   └── Camera.md
│   ├── Motor_Robot/              # 电机与机器人
│   │   └── Motor_Robot.md
│   ├── Communication/            # 通讯技术
│   │   └── Communication.md
│   ├── Model_Learn/              # 强化学习
│   │   └── Model_learn.md
│   ├── Attention/                # 注意事项
│   │   └── Attention.md
│   ├── image/                    # 文档图片资源
│   │   ├── 机械臂示意图.png
│   │   ├── Miniconda条款.png
│   │   ├── 运动学逆解路径.png
│   │   ├── ManiSkill数据集.png
│   │   └── ManiSkill.png
│   └── stylesheets/              # 自定义样式
│       └── extra.css
├── mkdocs.yml                    # MkDocs 配置文件
├── requirements.txt              # Python 依赖
└── site/                         # 构建输出（自动生成）
```

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

### 构建网站

```bash
# 构建静态网站到 site/ 目录
mkdocs build

# 严格模式（警告视为错误）
mkdocs build --strict
```

## 📦 部署到 GitHub Pages

### 方法 1: 使用 mkdocs 命令（推荐）

```bash
# 一键部署到 gh-pages 分支
mkdocs gh-deploy
```

该命令会自动：

1. 构建网站到 `site/` 目录
2. 创建/更新 `gh-pages` 分支
3. 推送到 GitHub
4. 网站将发布到: https://cx-1017.github.io/mkdocs-xlerobot/

### 方法 2: 使用 GitHub Actions

项目已配置 GitHub Actions，只需推送代码：

```bash
git add .
git commit -m "更新文档"
git push origin main
```

GitHub Actions 会自动构建并部署到 GitHub Pages。

## 🎨 功能特性

- ✅ Material Design 主题
- ✅ 中文语言支持
- ✅ 日间/夜间模式切换
- ✅ 搜索功能（中英文）
- ✅ 代码高亮和复制
- ✅ Git 修订日期显示
- ✅ Markdown 扩展语法
- ✅ 自定义样式

## 📝 编辑文档

文档采用 Markdown 格式，编辑 `docs/index.md` 即可更新内容。

### 支持的 Markdown 扩展

````markdown
# 警告框

!!! note "注意"
这是一个提示框

# 代码块

```python
print("Hello World")
\`\`\`

# 表格
| 列1 | 列2 |
|-----|-----|
| A   | B   |

# 按钮
[访问 GitHub](https://github.com/Vector-Wangel/XLeRobot){ .md-button }
```
````

## 🔧 配置说明

### mkdocs.yml

核心配置：

```yaml
site_name: XLeRobot 项目文档
site_url: https://cx-1017.github.io/mkdocs-xlerobot/
repo_url: https://github.com/Vector-Wangel/XLeRobot

theme:
  name: material
  language: zh

nav:
  - 首页: index.md
```

### requirements.txt

Python 依赖包：

- `mkdocs>=1.5.0` - MkDocs 核心
- `mkdocs-material>=9.5.0` - Material 主题
- `mkdocs-git-revision-date-localized-plugin` - Git 日期插件
- `markdown-callouts` - Callout 支持

## 🌐 网站地址

- **在线文档**: https://cx-1017.github.io/mkdocs-xlerobot/
- **项目仓库**: https://github.com/Vector-Wangel/XLeRobot

## � 文档内容

文档采用模块化组织结构：

### 📋 核心文档

- **首页** (`index.md`) - 项目概览和快速导航
- **完整项目分析** (`XLeRobot项目分析.md`) - 完整的项目技术分析

### 📚 学习模块

- **相关链接** - 项目 GitHub 仓库、B 站视频教程等资源链接
- **Python 语法** - Python 编程语法和最佳实践
- **3D 建模 SolidWork** - SolidWorks 3D 建模教程
- **树莓派 & Linux** - 树莓派配置、Linux 命令、系统设置
- **计算机视觉** - YOLO、OpenCV、RealSense 深度相机
- **电机 & 机器人** - ST3215 舵机、运动学、机器人控制
- **通讯** - 串口通讯、TCP、ZMQ、Socket 编程
- **强化学习** - MuJoCo、ManiSkill、SAPIEN 仿真环境

### ⚠️ 其他

- **注意事项** - 开发过程中的重要提示和常见问题

## 💡 开发提示

### 添加图片

将图片放在 `docs/image/` 目录，然后在 Markdown 中引用：

```markdown
![图片描述](image/your-image.png)
```

### 自定义样式

编辑 `docs/stylesheets/extra.css` 文件可以添加自定义样式。

### 实时预览

使用 `mkdocs serve` 命令会启动实时预览服务器，修改文档后浏览器会自动刷新。

## 🎯 部署流程

```bash
# 1. 编辑文档
vim docs/index.md

# 2. 本地预览
mkdocs serve

# 3. 测试构建
mkdocs build

# 4. 部署到 GitHub Pages
mkdocs gh-deploy

# 或者推送到 GitHub 让 Actions 自动部署
git add .
git commit -m "更新文档"
git push origin main
```

## 📞 联系方式

- GitHub Issues: [提交问题](https://github.com/Vector-Wangel/XLeRobot/issues)
- B 站视频: [XLeRobot 演示](https://www.bilibili.com/video/BV1bbaFzLEga)

---

<div align="center">

**祝你的 XLeRobot 项目顺利！** 🤖✨

</div>
