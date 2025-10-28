# 树莓派 & Linux 系统

## 🍓 树莓派简介

树莓派是 XLeRobot 项目的核心主控平台，负责：

- 🧠 运行主控程序
- 📡 协调各个硬件模块
- 🎥 处理视觉数据
- 🤖 执行控制算法

## 📋 硬件选型

### 推荐配置

| 型号           | CPU         | RAM   | 价格  | 推荐度     |
| -------------- | ----------- | ----- | ----- | ---------- |
| **树莓派 5**   | 4 核 2.4GHz | 4/8GB | ¥500+ | ⭐⭐⭐⭐⭐ |
| **树莓派 4B**  | 4 核 1.8GHz | 4GB   | ¥300+ | ⭐⭐⭐⭐   |
| **树莓派 3B+** | 4 核 1.4GHz | 1GB   | ¥200+ | ⭐⭐⭐     |

**操作系统**: Ubuntu 20.04 LTS (ARM64)

## 🔧 Linux 基础

### 必备命令

#### 文件操作

```bash
# 切换目录
cd /home/pi/XLeRobot

# 查看当前路径
pwd

# 列出文件
ls -la

# 复制文件
cp source.txt dest.txt

# 移动/重命名
mv old.txt new.txt

# 删除文件
rm file.txt
rm -rf directory/  # 删除目录
```

#### 文件权限

```bash
# 查看权限
ls -l

# 修改权限 (读写执行 = 4+2+1 = 7)
chmod 777 script.sh  # 所有人可读写执行
chmod +x script.sh   # 添加执行权限

# 修改所有者
sudo chown pi:pi file.txt
```

#### 进程管理

```bash
# 查看进程
ps aux | grep python

# 杀死进程
kill PID
kill -9 PID  # 强制杀死

# 后台运行
python script.py &

# 查看后台任务
jobs
```

### USB 设备管理

#### 查看 USB 设备

```bash
# 列出 USB 串口设备
ls /dev/ttyUSB*

# 查看 USB 设备详情
lsusb

# 查看设备信息
dmesg | grep ttyUSB
```

#### 配置串口权限

```bash
# 添加用户到 dialout 组
sudo usermod -a -G dialout $USER

# 修改设备权限
sudo chmod 666 /dev/ttyUSB0

# 永久配置 (udev 规则)
sudo nano /etc/udev/rules.d/99-usb-serial.rules
# 添加: SUBSYSTEM=="tty", ATTRS{idVendor}=="1a86", MODE="0666"
```

## 🌐 网络配置

### WiFi 配置

#### 命令行配置

```bash
# 扫描 WiFi
sudo iwlist wlan0 scan | grep ESSID

# 配置 WiFi
sudo nano /etc/netplan/50-cloud-init.yaml
```

```yaml
network:
  version: 2
  wifis:
    wlan0:
      dhcp4: true
      access-points:
        'YourWiFiName':
          password: 'YourPassword'
```

```bash
# 应用配置
sudo netplan apply
```

#### 查看 IP 地址

```bash
# 查看所有网络接口
ifconfig

# 或使用 ip 命令
ip addr show

# 查看 WiFi 连接状态
iwconfig
```

### SSH 远程连接

#### 启用 SSH

```bash
# Ubuntu 系统
sudo systemctl enable ssh
sudo systemctl start ssh

# 检查状态
sudo systemctl status ssh
```

#### 从电脑连接

```bash
# Windows PowerShell / Linux Terminal
ssh pi@192.168.1.100

# 指定端口
ssh -p 22 pi@192.168.1.100
```

#### 配置免密登录

```bash
# 1. 本地生成密钥 (如果还没有)
ssh-keygen -t rsa -b 4096

# 2. 复制公钥到树莓派
ssh-copy-id pi@192.168.1.100

# 3. 测试连接
ssh pi@192.168.1.100  # 不再需要密码
```

#### VSCode Remote SSH

```bash
# ~/.ssh/config
Host raspberry-pi
    HostName 192.168.1.100
    User pi
    IdentityFile ~/.ssh/id_rsa
```

然后在 VSCode 中安装 Remote-SSH 插件，选择配置的主机即可连接。

## 🖥️ VNC 图形界面

### 安装 VNC Server

```bash
# 启动 VNC 服务
vncserver

# 或使用虚拟桌面
vncserver-virtual

# 查看 VNC 会话
vncserver -list

# 停止 VNC
vncserver -kill :1
```

### VNC 灰屏问题解决

如果 VNC Viewer 连接后显示灰屏：

```bash
# 1. SSH 连接到树莓派
ssh pi@192.168.1.100

# 2. 编辑配置文件
sudo nano /boot/config.txt

# 3. 添加以下内容
hdmi_force_hotplug=1
hdmi_group=2
hdmi_mode=82

# 4. 保存并重启
sudo reboot
```

## 🎓 树莓派学习资源

### 视频教程

| 教程名称               | 时长 | 难度 | 链接                                                                                      |
| ---------------------- | ---- | ---- | ----------------------------------------------------------------------------------------- |
| **树莓派开发套件教程** | 系列 | ⭐⭐ | [:fontawesome-brands-bilibili: BV1TF411y7kn](https://www.bilibili.com/video/BV1TF411y7kn) |
| **Python 开发入门**    | 系列 | ⭐⭐ | 教程 P3-Python 部分                                                                       |

### 博客教程

#### 无线安装树莓派

- [:material-book: 无线安装教程](https://blog.csdn.net/2301_79835444/article/details/142747112)

#### 串口通信

- [:material-book: 树莓派串口收发](https://www.cnblogs.com/popepy/p/18470035)

#### SSH 免密登录

- [:material-book: SSH 免密登录配置](https://www.cnblogs.com/jesn/articles/14317949.html)

## ⚙️ 环境配置

### 系统更新

```bash
# 更新软件源
sudo apt update

# 升级已安装软件
sudo apt upgrade -y

# 清理缓存
sudo apt autoremove
sudo apt clean
```

### 安装基础工具

```bash
# 开发工具
sudo apt install -y git vim build-essential

# Python 依赖
sudo apt install -y python3-pip python3-dev

# 网络工具
sudo apt install -y net-tools wireless-tools

# USB 工具
sudo apt install -y usbutils
```

### Miniconda 安装

#### 树莓派 3B+ / 4B

```bash
# 下载 Miniconda (ARM64)
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-aarch64.sh

# 安装
bash Miniconda3-latest-Linux-aarch64.sh

# 初始化
source ~/.bashrc

# 创建环境
conda create -n xlerobot python=3.10
conda activate xlerobot
```

#### 树莓派 5

```bash
# 树莓派 5 使用相同步骤
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-aarch64.sh
bash Miniconda3-latest-Linux-aarch64.sh
```

#### 接受许可协议

```bash
# 如果遇到许可协议问题
conda config --set auto_activate_base false
conda init bash
source ~/.bashrc
```

## 🔍 常见问题

### Q: 如何查看树莓派温度？

```bash
# CPU 温度
vcgencmd measure_temp

# 持续监控
watch -n 1 vcgencmd measure_temp
```

### Q: 如何扩展 SD 卡空间？

```bash
# Ubuntu 自动扩展，如果没有：
sudo raspi-config
# 选择 Advanced Options → Expand Filesystem
```

### Q: WiFi 连接不稳定？

```bash
# 禁用电源管理
sudo iw dev wlan0 set power_save off

# 永久配置
sudo nano /etc/rc.local
# 添加: /sbin/iw dev wlan0 set power_save off
```

## 🚀 下一步

学习完树莓派基础后，可以继续：

- [:material-language-python: Python 开发](python.md)
- [:material-chip: 电机控制](motors-robotics.md)
- [:material-network: 通讯技术](communication.md)

!!! tip "实践建议"
先熟悉 SSH 远程连接，后续开发都可以通过电脑远程操作树莓派，更加方便！
