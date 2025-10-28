# 常见问题 FAQ

## 🔧 环境配置问题

### Q1: Miniconda 安装后无法接受条款？

**错误信息**:

```
CondaToSNonInteractiveError: Too many arguments
```

**解决方案**:

```bash
# 树莓派/Linux 环境
conda config --set auto_activate_base false
conda init bash
source ~/.bashrc

# 手动接受条款
conda config --set allow_conda_downgrades true
```

**参考**: [Miniconda 错误解决](https://jishuzhan.net/article/1958023261471682561)

---

### Q2: 树莓派 VNC 连接灰屏？

**问题描述**: 使用 VNC Viewer 连接树莓派显示灰屏

**解决方案**:

```bash
# 1. SSH 连接到树莓派
ssh pi@your_raspberry_pi_ip

# 2. 编辑配置文件
sudo nano /boot/config.txt

# 3. 添加以下内容
hdmi_force_hotplug=1
hdmi_group=2
hdmi_mode=82

# 4. 重启
sudo reboot

# 5. 启动 VNC 服务
vncserver-virtual
```

**参考**: [VNC 灰屏解决](https://www.bilibili.com/opus/1083615112051294228)

---

### Q3: pip install -r requirements.txt 失败？

**常见原因**:

1. **网络问题** - 使用国内镜像源

```bash
pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple
```

2. **依赖冲突** - 逐个安装

```bash
# 先安装基础包
pip install numpy opencv-python

# 再安装其他包
pip install -r requirements.txt
```

3. **Python 版本不匹配** - 确认版本

```bash
python --version  # 应该是 3.10+
```

---

## 🔌 硬件连接问题

### Q4: 找不到 /dev/ttyUSB0 设备？

**检查步骤**:

```bash
# 1. 列出所有 USB 设备
ls /dev/ttyUSB*

# 2. 查看 USB 设备信息
lsusb

# 3. 检查权限
sudo chmod 666 /dev/ttyUSB0

# 4. 添加用户到 dialout 组
sudo usermod -a -G dialout $USER
# 需要重新登录生效
```

**如果仍然没有设备**:

- 检查 USB 线缆是否正常
- 尝试其他 USB 接口
- 检查驱动是否安装（CP2102/CH340）

---

### Q5: 舵机不响应或抖动？

**排查步骤**:

1. **检查供电**

   ```
   舵机需要独立电源（6-8.4V, >5A）
   不要使用树莓派的 5V 供电
   ```

2. **检查波特率**

   ```python
   # 确认波特率设置正确
   ser = serial.Serial('/dev/ttyUSB0', baudrate=1000000)
   ```

3. **检查 ID 配置**

   ```python
   # 使用飞特调试软件检查舵机ID
   # 确保代码中的ID与实际一致
   ```

4. **检查数据线**
   - 确认 TX、RX 没有接反
   - 检查接线是否牢固

---

## 💻 软件运行问题

### Q6: YOLO 模型加载失败？

**错误信息**:

```
FileNotFoundError: yolo11n.pt not found
```

**解决方案**:

```python
from ultralytics import YOLO

# 第一次运行会自动下载
model = YOLO('yolo11n.pt')

# 或手动指定路径
model = YOLO('/path/to/your/yolo11n.pt')

# 如果下载失败，手动下载后放到指定目录
# https://github.com/ultralytics/assets/releases
```

---

### Q7: OpenCV 无法打开摄像头？

**检查步骤**:

```python
import cv2

# 尝试不同的摄像头索引
cap = cv2.VideoCapture(0)  # 或 1, 2, 3...

if not cap.isOpened():
    print("无法打开摄像头")
    # 检查设备列表
    import subprocess
    result = subprocess.run(['v4l2-ctl', '--list-devices'],
                           capture_output=True, text=True)
    print(result.stdout)
```

**树莓派 CSI 摄像头**:

```bash
# 启用摄像头
sudo raspi-config
# Interface Options → Camera → Enable

# 测试
raspistill -o test.jpg
```

---

### Q8: SSH 连接需要频繁输入密码？

**配置免密登录**:

```bash
# 1. 本地生成 SSH 密钥（如果还没有）
ssh-keygen -t rsa

# 2. 复制公钥到树莓派
ssh-copy-id pi@192.168.1.100

# 3. 测试连接
ssh pi@192.168.1.100  # 不再需要密码
```

**VSCode Remote SSH 配置**:

```bash
# ~/.ssh/config
Host raspberry-pi
    HostName 192.168.1.100
    User pi
    IdentityFile ~/.ssh/id_rsa
```

---

## 🤖 机器人控制问题

### Q9: 机械臂运动不准确？

**校准步骤**:

1. **硬件校准**

   ```bash
   # 使用飞特上位机校准舵机零位
   # 确保机械臂在零位时各关节角度正确
   ```

2. **软件校准**

   ```python
   # 在配置文件中调整偏移量
   motor_offsets = {
       "shoulder": 5.0,  # 度
       "elbow": -3.2,
       "wrist": 1.5
   }
   ```

3. **运动学参数**
   ```python
   # 确认DH参数正确
   # 参考 github 仓库中的运动学笔记
   ```

---

### Q10: ZMQ 通讯连接超时？

**排查网络**:

```bash
# 1. 检查 IP 地址
# Windows
ipconfig
# Linux
ifconfig

# 2. 测试连通性
ping 192.168.1.100

# 3. 检查防火墙
# Windows
# 控制面板 → 防火墙 → 允许应用
# Linux
sudo ufw allow 5555/tcp

# 4. 测试端口
# 服务端
import zmq
context = zmq.Context()
socket = context.socket(zmq.REP)
socket.bind("tcp://*:5555")
print("服务端等待连接...")

# 客户端
socket = context.socket(zmq.REQ)
socket.connect("tcp://192.168.1.100:5555")
print("已连接到服务端")
```

---

## 🎓 学习相关问题

### Q11: 从哪里开始学习？

**推荐路径**:

1. **完全新手** → [Python 基础](learning/python.md)
2. **有编程基础** → [树莓派配置](learning/raspberry-pi.md)
3. **有硬件经验** → [硬件组装](hardware/assembly.md)
4. **想快速上手** → [快速开始](development/quickstart.md)

---

### Q12: 需要什么硬件基础？

**必备技能**:

- ✅ 基本的焊接能力
- ✅ 使用螺丝刀组装
- ✅ 识别电子元件

**可选技能**:

- 3D 打印
- 电路设计
- 机械加工

**如果完全没有经验**:

- 建议从套件开始
- 参考组装视频
- 加入社区讨论

---

## 🐛 其他问题

### Q13: GitHub 下载太慢？

**解决方案**:

```bash
# 1. 使用 GitHub 镜像
git clone https://ghproxy.com/https://github.com/Vector-Wangel/XLeRobot.git

# 2. 使用 Gitee 镜像（如果有）
git clone https://gitee.com/xxx/XLeRobot.git

# 3. 下载 Release 压缩包
# 访问 GitHub → Releases → 下载 Source code.zip
```

---

### Q14: 如何贡献代码？

**贡献流程**:

1. Fork 项目到自己的仓库
2. Clone 到本地
3. 创建新分支: `git checkout -b feature-xxx`
4. 提交修改: `git commit -am 'Add xxx'`
5. Push 到 GitHub: `git push origin feature-xxx`
6. 创建 Pull Request

**代码规范**:

- 遵循 PEP 8
- 添加必要的注释
- 更新相关文档

---

## 🆘 获取帮助

如果以上 FAQ 没有解决你的问题：

1. **搜索 Issues**

   - [GitHub Issues](https://github.com/Vector-Wangel/XLeRobot/issues)
   - 可能已有类似问题

2. **提交新 Issue**

   - 描述问题现象
   - 提供错误信息
   - 说明环境配置

3. **社区讨论**

   - [GitHub Discussions](https://github.com/Vector-Wangel/XLeRobot/discussions)
   - [LeRobot 中文社区](https://wiki.wowrobo.com/)

4. **视频评论区**
   - B 站视频评论区
   - 作者可能会回复

!!! tip "提问技巧" - 详细描述问题 - 附上错误截图/日志 - 说明已尝试的解决方法 - 提供系统和版本信息
