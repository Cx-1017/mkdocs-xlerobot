# XLeRobot 项目文档

基于 MkDocs Material 构建的 XLeRobot 项目分析文档。

## 📁 项目结构

```
documents/
├── docs/                    # 文档源文件
│   ├── index.md            # XLeRobot项目分析（首页）
│   ├── image/              # 文档图片资源
│   └── stylesheets/        # 自定义样式
├── mkdocs.yml              # MkDocs 配置文件
├── requirements.txt        # Python 依赖
├── .github/
│   └── workflows/
│       └── ci.yml          # GitHub Actions 自动部署配置
└── site/                   # 构建输出（自动生成）
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

```markdown
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

## 📖 文档内容

当前文档包含 XLeRobot 项目的完整分析，包括：

- 项目简介和官方资源
- 技术架构（硬件、软件、通讯系统）
- 核心技术栈
- 关键技术特性
- 项目文档指引

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
- B站视频: [XLeRobot 演示](https://www.bilibili.com/video/BV1bbaFzLEga)

---

<div align="center">

**祝你的 XLeRobot 项目顺利！** 🤖✨

</div>
