# 基础概念

本文介绍理解CARLA服务器与客户端如何通过API进行通信所需的核心概念。CARLA采用客户端-服务器架构，服务器运行仿真，客户端通过API发送指令。客户端代码通过[__API__](python_api.md)与服务器通信，使用前需通过PIP安装Python模块：

```sh
pip install carla-simulator # Python 2
pip3 install carla-simulator # Python 3
```

在Python脚本中需导入CARLA包：

```py
import carla
```

- [__世界与客户端__](#世界与客户端)  
	- [客户端](#客户端) 
    - [世界](#世界)
- [__同步与异步模式__](#同步与异步模式)  
	- [设置同步模式](#设置同步模式) 
    - [使用同步模式](#使用同步模式)
- [__记录器__](#记录器)  
	- [记录](#记录) 
    - [仿真回放](#仿真回放)
    - [记录文件格式](#记录文件格式)

---

## 世界与客户端

### 客户端

__客户端__是用户用来获取仿真信息或请求变更的模块。客户端通过特定IP和端口运行，与服务器进行终端通信。支持多个客户端同时运行，但高级多客户端管理需深入理解CARLA及[同步机制](adv_synchrony_timestep.md)。

通过CARLA客户端对象进行初始化：

```py
client = carla.Client('localhost', 2000)
```

此代码设置客户端与运行在本地机器（localhost）的CARLA服务器通信，默认端口2000。若需连接远程服务器，应替换为对应IP地址。

客户端对象支持多种功能，包括加载新地图、启动记录等：

```py
client.load_world('Town07')
client.start_recorder('recording.log')
```

### 世界

__世界__对象代表仿真环境，包含生成参与者、更改天气、获取当前状态等方法。每个仿真只有一个世界对象，切换地图时将销毁重建。

通过客户端获取世界对象：

```py
world = client.get_world()
```

世界对象可访问仿真中的各类元素：

```py
level = world.get_map()
weather = world.get_weather()
blueprint_library = world.get_blueprint_library()
```

## 同步与异步模式

CARLA默认运行在__异步模式__，服务器自主推进仿真，客户端请求即时处理。__同步模式__下客户端通过代码控制服务器更新时机。

__异步模式__适合实验性调试，__同步模式__适用于生成训练数据或部署智能体，提供更高的可控性。

!!! 注意
    多客户端架构中应仅有一个客户端发送tick指令，服务器会将所有tick视为同一来源，多客户端发送会导致状态不一致。

### 设置同步模式

通过布尔状态切换模式：

```py
settings = world.get_settings()
settings.synchronous_mode = True # 启用同步模式
settings.fixed_delta_seconds = 0.05
world.apply_settings(settings)
```

!!! 警告
    启用同步模式时，需同时设置交通管理器为同步模式，详见[此文档](adv_traffic_manager.md#同步模式)。

禁用同步模式：

```sh
cd PythonAPI/util && python3 config.py --no-sync
```

### 使用同步模式

同步模式对客户端处理速度较慢或需要传感器同步时尤为重要。以下示例演示相机传感器数据同步采集：

```py
settings = world.get_settings()
settings.synchronous_mode = True
world.apply_settings(settings)

camera = world.spawn_actor(blueprint, transform)
image_queue = queue.Queue()
camera.listen(image_queue.put)

while True:
    world.tick()
    image = image_queue.get()
```

!!! 重要
    GPU传感器（如相机）数据通常会有数帧延迟，同步模式对此类传感器至关重要。

世界对象提供异步方法实现客户端等待：

```py
# 等待下一帧并获取快照
world_snapshot = world.wait_for_tick()

# 注册回调函数接收新快照
world.on_tick(lambda world_snapshot: do_something(world_snapshot))
```

## 记录器

记录器可将仿真数据保存为二进制文件，用于精确复现历史仿真。记录内容包含车辆位置速度、交通灯状态、行人运动及环境条件等。

### 记录

启动记录仅需指定文件名：

```py
client.start_recorder("/home/carla/recording01.log")
```

启用附加数据记录：

```py
client.start_recorder("/home/carla/recording01.log", True)
```

!!! 注意
    附加数据包含：车辆行人运动速度、交通灯时序、执行时间、碰撞框及车辆物理控制参数。

停止记录：

```py
client.stop_recorder()
```

### 仿真回放

回放参数说明：

```py
client.replay_file("recording01.log", start, duration, camera)
```

| 参数     | 说明                                 | 备注                      |
|----------|--------------------------------------|---------------------------|
| start    | 开始时间（秒）                       | 正数从开头计，负数从末尾计 |
| duration | 回放时长（0表示完整回放）             | 结束时车辆切为自动驾驶     |
| camera   | 摄像机聚焦的参与者ID（0为自由视角）  |                           |

### 记录文件格式

记录文件采用自定义二进制格式，详见[格式说明文档](ref_recorder_binary_file_format.md)。