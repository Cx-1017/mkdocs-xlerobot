# 远程控制调试流程

以下为远程控制调试流程，包括 PC 端和树莓派端两部分，具体清单为：

- PC 端

  1. 左右机械臂 bus 1/2(左右机械臂各 6 电机)

- 树莓派端
  1. bus 1(头部相机 2 电机、左机械臂 6 电机)
  2. bus 2(右机械臂 6 电机)
  3. 手部 OpenCV 相机 ×2
  4. Realsense/Orbbec

## 一、共同

进行以下调试前均需要进行以下操作:

1. 移动到指定`lerobot-last`文件路径下
2. `conda activate lerobot` 激活 python 环境

## 二、树莓派端

### 2.1 匹配左右机械臂串口号

在终端输入一下命令，确认左右机械臂端口(根据提示插拔即可)

```bash
lerobot-find-port
```

调整为左机械臂端口为`ttyACM0`,右机械臂为`ttyACM1`,调整时可以先将两个 USB 都断开，先插左臂即可确保端口为`ttyACM0`。

### 2.2 使能机械臂端口

```bash
sudo chmod 777 /dev/ttyACM0
sudo chmod 777 /dev/ttyACM1

# 或者使用
sudo chmod 777 /dev/ttyACM*
```

### 2.3 分别校准左右机械臂

终端运行一下命令，根据提示加载机械臂各个关节的最大最小值

```bash
# 左总线校准，包括左机械臂+头部相机
lerobot-calibrate --robot.type=so101_follower --robot.port=/dev/ttyACM0 --robot.id=host_left_arm
# 右总线校准，包括仅右机械臂(但是需要确认底盘接线牢固)
lerobot-calibrate --robot.type=so101_follower --robot.port=/dev/ttyACM1 --robot.id=host_right_arm
```

### 2.4 配置 OpenCV 摄像头端口

在终端中输入以下命令：

```bash
lerobot-find-cameras opencv

# 输出类似结果
--- Detected Cameras ---
Camera #0:
  Name: OpenCV Camera @ /dev/video0
  Type: OpenCV
  Id: /dev/video0
  Backend api: V4L2
  Default stream profile:
    Format: 0.0
    Width: 640
    Height: 480
    Fps: 30.0
--------------------
Camera #1:
  Name: OpenCV Camera @ /dev/video23
  Type: OpenCV
  Id: /dev/video23
  Backend api: V4L2
  Default stream profile:
    Format: 0.0
    Width: 640
    Height: 480
    Fps: -1.0
--------------------
Camera #2:
  Name: OpenCV Camera @ /dev/video24
  Type: OpenCV
  Id: /dev/video24
  Backend api: V4L2
  Default stream profile:
    Format: 0.0
    Width: 640
    Height: 480
    Fps: -1.0
--------------------
Camera #3:
  Name: OpenCV Camera @ /dev/video25
  Type: OpenCV
  Id: /dev/video25
  Backend api: V4L2
  Default stream profile:
    Format: 0.0
    Width: 640
    Height: 480
    Fps: -1.0
--------------------
Camera #4:
  Name: OpenCV Camera @ /dev/video26
  Type: OpenCV
  Id: /dev/video26
  Backend api: V4L2
  Default stream profile:
    Format: 0.0
    Width: 640
    Height: 480
    Fps: -1.0
--------------------
Camera #5:
  Name: OpenCV Camera @ /dev/video31
  Type: OpenCV
  Id: /dev/video31
  Backend api: V4L2
  Default stream profile:
    Format: 0.0
    Width: 640
    Height: 480
    Fps: -1.0
--------------------
Camera #6:
  Name: OpenCV Camera @ /dev/video32
  Type: OpenCV
  Id: /dev/video32
  Backend api: V4L2
  Default stream profile:
    Format: 0.0
    Width: 640
    Height: 480
    Fps: -1.0
--------------------
Camera #7:
  Name: OpenCV Camera @ /dev/video33
  Type: OpenCV
  Id: /dev/video33
  Backend api: V4L2
  Default stream profile:
    Format: 0.0
    Width: 640
    Height: 480
    Fps: -1.0
--------------------
Camera #8:
  Name: OpenCV Camera @ /dev/video34
  Type: OpenCV
  Id: /dev/video34
  Backend api: V4L2
  Default stream profile:
    Format: 0.0
    Width: 640
    Height: 480
    Fps: -1.0
--------------------
Camera #9:
  Name: OpenCV Camera @ /dev/video4
  Type: OpenCV
  Id: /dev/video4
  Backend api: V4L2
  Default stream profile:
    Format: 0.0
    Width: 640
    Height: 480
    Fps: 30.0
--------------------
Camera #10:
  Name: OpenCV Camera @ /dev/video6
  Type: OpenCV
  Id: /dev/video6
  Backend api: V4L2
  Default stream profile:
    Format: 0.0
    Width: 640
    Height: 480
    Fps: 30.0
--------------------
Camera #11:
  Name: OpenCV Camera @ /dev/video8
  Type: OpenCV
  Id: /dev/video8
  Backend api: V4L2
  Default stream profile:
    Format: 0.0
    Width: 640
    Height: 480
    Fps: 30.0
--------------------
```

运行以上代码后，如果终端最后提示：

```bash
Finalizing image saving...
Image capture finished. Images saved to outputs/captured_images
```

即可在`outputs/captured_images`路径下，找到各个摄像头的图片，方便快速确定摄像头所在的机械臂位置(用于判断左右机械臂)。
分析以上输出的结果，需要记录 fps 不为 0 的摄像头下标/路径，得到该数据后，修改`src/lerobot/robots/so101_bimanual_remote/so101_bimanual_host_all.py`中关于 opencv 的配置，如根据参数：

```bash
Camera #11:
  Name: OpenCV Camera @ /dev/video8
  Type: OpenCV
  Id: /dev/video8
  Backend api: V4L2
  Default stream profile:
    Format: 0.0
    Width: 640
    Height: 480
    Fps: 30.0
```

在代码中设置为：

```python
"left": OpenCVCameraConfig(index_or_path="/dev/video8", width=640, height=480, fps=30),
```

### 2.5 深度相机 Realsense

运行一下代码，检测 realsense 的串口序号和分辨率

```bash
lerobot-find-cameras Realsense

# 终端输出
--- Detected Cameras ---
Camera #0:
  Name: Intel RealSense D415
  Type: RealSense
  Id: 935722061309
  Firmware version: 5.17.0.10
  Usb type descriptor: 2.1
  Physical port: /sys/devices/platform/axi/1000120000.pcie/1f00300000.usb/xhci-hcd.1/usb3/3-2/3-2:1.0/video4linux/video2
  Product id: 0AD3
  Product line: D400
  Default stream profile:
    Stream_type: Color
    Format: rgb8
    Width: 640
    Height: 480
    Fps: 15
--------------------
Error reading from RealSense camera 935722061309: RealSenseCamera(935722061309) read failed (status=False).
Error reading from RealSense camera 935722061309: RealSenseCamera(935722061309) read failed (status=False).

Finalizing image saving...
Image capture finished. Images saved to outputs/captured_images
```

根据以上设置，修改代码配置为：

```bash
"head": RealSenseCameraConfig(
              serial_number_or_name="935722061309",  # Replace with camera SN
              fps=15,
              width=640,
              height=480,
              color_mode=ColorMode.RGB, # Request RGB output
              rotation=Cv2Rotation.NO_ROTATION,
              use_depth=True
          ),
```

### 2.6 奥菲中光 Orbbec

在终端运行以下代码，可快捷找到现有可使用的 Orbbec 摄像头，并输出对应的配置信息。

```bash
python src/lerobot/cameras/orbbec/find_cameras.py

# 终端输出
INFO: ================================================================================
INFO: Orbbec Camera Discovery Tool
INFO: ================================================================================
INFO: Scanning for available Orbbec cameras...

INFO: ✓ Found Orbbec camera at device index: 0
INFO:   Color stream profiles:
INFO:     - 640x480 @ 30fps (RGB)
INFO:     - 1280x720 @ 30fps (RGB)
INFO:     - 1920x1080 @ 30fps (RGB)
INFO:   Depth stream profiles (supported):
INFO:     - 640x480 @ 30fps (DEPTH)

INFO:
INFO: ✓ Found 1 valid Orbbec camera(s)
INFO:

INFO: ================================================================================
INFO: QUICK REFERENCE - COPY-PASTE READY
INFO: ================================================================================
INFO:
INFO: Valid Orbbec camera indices for OrbbecCameraConfig(index_or_path=?):
INFO:
INFO:   ✓ Device Index: 0
INFO:     └─ Recommended: index_or_path=0, width=640, height=480, fps=30
INFO:     └─ Depth support: ✓ Yes
INFO:
INFO: ================================================================================
INFO:
INFO: ================================================================================
INFO: CONFIGURATION TEMPLATE FOR so101_bimanual_host_all.py
INFO: ================================================================================
INFO:
INFO: # Replace in cameras dict:
"head": OrbbecCameraConfig(
    index_or_path=0,  # ✓ Valid device index
    fps=30,
    width=640,
    height=480,
    color_mode=ColorMode.RGB,
    rotation=Cv2Rotation.NO_ROTATION,
    use_depth=True  # Depth supported
),
INFO: ================================================================================
INFO:
INFO: ================================================================================
INFO: USAGE EXAMPLES
INFO: ================================================================================
INFO:
INFO: For Camera 0 (using device index):
INFO:
from lerobot.cameras.orbbec import OrbbecCamera, OrbbecCameraConfig

config = OrbbecCameraConfig(
    index_or_path=0,
    fps=30,
    width=640,
    height=480,
    color_mode=ColorMode.RGB,
    rotation=Cv2Rotation.NO_ROTATION,
    use_depth=True
)

camera = OrbbecCamera(config)
camera.connect()
frame = camera.read()
camera.disconnect()

INFO: --------------------------------------------------------------------------------
INFO:
INFO: ================================================================================
INFO: SUCCESS!
INFO: ================================================================================
INFO:
INFO: Use the indices above in OrbbecCameraConfig(index_or_path=...)
INFO:
```

根据终端输出的提示，同理修改代码为：

```bash
"head": OrbbecCameraConfig(
                index_or_path=0,  # Orbbec camera device index (use 0 for first camera, or serial number)
                fps=30,
                width=640,
                height=480,
                color_mode=ColorMode.RGB,  # Use BGR to match Orbbec native output
                rotation=Cv2Rotation.NO_ROTATION,
                use_depth=False  # Set to True only if camera supports depth
            ),
```

### 2.7 运行树莓派端代码

注意请在`lerobot-last`根目录下运行以下代码,并根据终端显示，判断各个外设是否连接，选择是否保留校准信息：

```bash
PYTHONPATH=src python -m lerobot.robots.so101_bimanual_remote.so101_bimanual_host_all
```

## 三、PC 端

部分操作同理树莓派端

### 3.1 匹配左右机械臂串口号

在终端输入一下命令，确认左右机械臂端口(根据提示插拔即可)

```bash
lerobot-find-port
```

调整为左机械臂端口为`ttyACM0`,右机械臂为`ttyACM1`,调整时可以先将两个 USB 都断开，先插左臂即可确保端口为`ttyACM0`。

### 3.2 使能机械臂端口

```bash
sudo chmod 777 /dev/ttyACM0
sudo chmod 777 /dev/ttyACM1

# 或者使用
sudo chmod 777 /dev/ttyACM*
```

### 3.3 分别校准左右机械臂

终端运行一下命令，根据提示加载机械臂各个关节的最大最小值

> Attention!
> 注意这里不要用树莓派的命令，两者是不一样的！

```bash
    # 左从臂校准
    lerobot-calibrate --teleop.type=so101_leader --teleop.port=/dev/ttyACM0 --teleop.id=client_left_arm
    # 右从臂校准
    lerobot-calibrate --teleop.type=so101_leader --teleop.port=/dev/ttyACM1 --teleop.id=client_right_arm
```

### 3.4 运行 PC 端代码

在确认树莓派 host 代码运行完成后，在 PC 端运行`teleoperate_all.py`
具体运行代码路径为：`examples\so101_bimanual_remote\teleoperate_all.py`

## 四、总流程简图

![alt text](../image/配置流程图.png)
