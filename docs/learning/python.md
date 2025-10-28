# Python 语法学习

## 🐍 Python 基础

XLeRobot 项目主要使用 Python 3.10+ 开发，需要掌握基本的 Python 语法和常用库。

## 📚 必备知识点

### 1. 基础语法

- 变量和数据类型
- 控制流（if/for/while）
- 函数定义和调用
- 类和面向对象
- 异常处理
- 文件操作

### 2. 常用库

```python
# 数值计算
import numpy as np

# 数据处理
import pandas as pd

# 绘图
import matplotlib.pyplot as plt

# 系统操作
import os, sys
from pathlib import Path
```

## 🎯 项目特定语法

### dataclass 装饰器

在 XLeRobot 中大量使用 `dataclass` 来定义配置类：

```python
from dataclasses import dataclass, field
from typing import Optional

@dataclass
class RobotConfig:
    """机器人配置类"""
    name: str
    dof: int = 6  # 默认值
    max_speed: float = 1.0
    motors: list = field(default_factory=list)  # 可变默认值

# 使用示例
config = RobotConfig(name="XLeRobot", dof=12)
print(config.name)  # XLeRobot
```

#### 为什么使用 dataclass？

✅ **自动生成方法** - `__init__`, `__repr__`, `__eq__` 等  
✅ **类型提示** - 增强代码可读性  
✅ **减少样板代码** - 比传统类定义更简洁  
✅ **不可变选项** - `frozen=True` 创建不可变对象

**推荐阅读**:

- [:material-link: Python dataclass 详解](https://zhuanlan.zhihu.com/p/651783317)
- [:material-book: 官方文档](https://docs.python.org/zh-cn/3/library/dataclasses.html)

### 类型提示 (Type Hints)

```python
from typing import List, Dict, Tuple, Optional, Union

def move_robot(
    position: List[float],
    speed: Optional[float] = None,
    mode: str = "linear"
) -> Dict[str, any]:
    """
    移动机器人

    Args:
        position: 目标位置 [x, y, z]
        speed: 移动速度 (可选)
        mode: 移动模式

    Returns:
        包含执行结果的字典
    """
    return {"success": True, "position": position}
```

### 上下文管理器

```python
# 串口通讯中常用
import serial

with serial.Serial('/dev/ttyUSB0', 1000000) as ser:
    ser.write(b'command')
    response = ser.read(10)
    # 自动关闭串口

# 自定义上下文管理器
class MotorController:
    def __enter__(self):
        print("初始化电机")
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        print("关闭电机")
        return False

with MotorController() as mc:
    # 使用电机
    pass
```

## 🎓 学习资源

### 入门教程

| 资源                | 难度   | 时长 | 链接                                                         |
| ------------------- | ------ | ---- | ------------------------------------------------------------ |
| **菜鸟教程**        | ⭐     | 1 周 | [链接](https://www.runoob.com/python3/python3-tutorial.html) |
| **廖雪峰教程**      | ⭐⭐   | 2 周 | [链接](https://www.liaoxuefeng.com/wiki/1016959663602400)    |
| **Python 官方教程** | ⭐⭐⭐ | 3 周 | [链接](https://docs.python.org/zh-cn/3/tutorial/)            |

### 视频教程

- [:fontawesome-brands-bilibili: Python 基础教程](https://www.bilibili.com/video/BV1ex411x7Em)
- [:fontawesome-brands-bilibili: Python 进阶技巧](https://www.bilibili.com/video/BV1wD4y1o7AS)

## 🔧 实战练习

### 练习 1: 电机数据类

```python
from dataclasses import dataclass
from typing import List

@dataclass
class Motor:
    """电机配置"""
    id: int
    name: str
    position: float = 0.0
    velocity: float = 0.0
    torque: float = 0.0

    def move_to(self, target: float, speed: float = 1.0):
        """移动到目标位置"""
        print(f"Motor {self.name} moving to {target}")
        self.position = target

# 创建电机组
motors = [
    Motor(1, "shoulder"),
    Motor(2, "elbow"),
    Motor(3, "wrist")
]

# 批量控制
for motor in motors:
    motor.move_to(90.0)
```

### 练习 2: 配置文件读取

```python
import json
from pathlib import Path
from typing import Dict, Any

def load_config(config_path: str) -> Dict[str, Any]:
    """加载 JSON 配置文件"""
    path = Path(config_path)

    if not path.exists():
        raise FileNotFoundError(f"配置文件不存在: {config_path}")

    with open(path, 'r', encoding='utf-8') as f:
        config = json.load(f)

    return config

# 使用示例
try:
    config = load_config("robot_config.json")
    print(f"机器人名称: {config['name']}")
except FileNotFoundError as e:
    print(f"错误: {e}")
```

### 练习 3: 异步操作

```python
import asyncio
from typing import List

async def read_sensor(sensor_id: int) -> float:
    """异步读取传感器数据"""
    await asyncio.sleep(0.1)  # 模拟 I/O 延迟
    return sensor_id * 10.0

async def main():
    """并发读取多个传感器"""
    tasks = [read_sensor(i) for i in range(5)]
    results = await asyncio.gather(*tasks)
    print(f"传感器数据: {results}")

# 运行
asyncio.run(main())
```

## 📖 常用代码片段

### 路径操作

```python
from pathlib import Path

# 项目根目录
ROOT_DIR = Path(__file__).parent.parent
DATA_DIR = ROOT_DIR / "data"
CONFIG_FILE = ROOT_DIR / "config" / "robot.yaml"

# 创建目录
DATA_DIR.mkdir(parents=True, exist_ok=True)

# 遍历文件
for file in DATA_DIR.glob("*.json"):
    print(file.name)
```

### 日志记录

```python
import logging

# 配置日志
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
)

logger = logging.getLogger(__name__)

# 使用日志
logger.info("机器人启动")
logger.warning("电池电量低")
logger.error("电机通讯失败")
```

### 命令行参数

```python
import argparse

def parse_args():
    parser = argparse.ArgumentParser(description="XLeRobot 控制程序")

    parser.add_argument("--config", type=str, required=True,
                       help="配置文件路径")
    parser.add_argument("--mode", choices=["teleop", "auto"],
                       default="teleop", help="运行模式")
    parser.add_argument("--debug", action="store_true",
                       help="开启调试模式")

    return parser.parse_args()

# 使用
args = parse_args()
print(f"配置文件: {args.config}")
print(f"运行模式: {args.mode}")
```

## 🎯 学习检查清单

完成以下检查点后，你就具备了开发 XLeRobot 所需的 Python 基础：

- [ ] 理解面向对象编程（类、继承、多态）
- [ ] 熟练使用 dataclass 定义数据类
- [ ] 掌握类型提示的使用
- [ ] 了解上下文管理器 (with 语句)
- [ ] 会使用 pathlib 处理文件路径
- [ ] 掌握异常处理机制
- [ ] 了解 asyncio 异步编程
- [ ] 会使用 argparse 处理命令行参数
- [ ] 熟悉 logging 日志模块
- [ ] 理解装饰器的基本用法

## 🚀 下一步

掌握 Python 基础后，可以继续学习：

- [:material-linux: Linux 系统操作](raspberry-pi.md)
- [:material-camera: 计算机视觉](computer-vision.md)
- [:material-robot: 机器人控制](motors-robotics.md)

!!! tip "学以致用"
建议边学边做，尝试阅读 XLeRobot 源代码，理解实际应用场景。
