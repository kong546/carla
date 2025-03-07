# ROS2 原生接口

![inverted_ai_logo](img/logos/ros_carla.png)

CARLA 模拟器原生支持从服务器端集成 ROS2。要使用 ROS2 功能，请通过命令行启动 CARLA 模拟器并添加 `--ros2` 参数：

```sh
./CarlaUnreal.sh --ros2
```

## 传感器数据

CARLA 服务器会广播所有已启用 ROS 支持的传感器数据。通过调用传感器类的 `enable_for_ros()` 方法启用 ROS 支持：

```py
sensor = world.spawn_actor(sensor_blueprint, transform)
sensor.enable_for_ros()
```

在生成传感器前通过蓝图设置 ROS 主题名称（默认会生成随机字符串名称）：

```py
bp = bp_lib.find('sensor.camera.rgb')
bp.set_attribute('ros_name', 'front_camera')
```

此时图像数据将发布到：`/carla/front_camera/image`。

若传感器挂载在角色（如 ego 车辆）上，主题名称会包含角色名称：`/carla/ego/front_camera/image`。

完整传感器消息格式请参考[ROS2 传感器参考文档](ros2_native_sensors_zhCN.md)。

## 控制数据

控制指令可发送至指定的 ego 车辆：

```py
bp = bp.find("vehicle.lincoln.mkz_2020")
bp.set_attribute("ros_name", "ego")

ego = carla.spawn_actor(bp, spawn_point)
```

当 ego 车辆生成时，会自动创建对应的控制指令订阅者：`/carla/ego/vehicle_control_cmd`。

## CarlaEgoVehicleControl.msg 控制消息

发送控制指令需安装 [ros-carla-msgs ROS 包](https://github.com/carla-simulator/ros-carla-msgs/tree/master)，控制消息包含以下字段：

| 字段                 | 类型                                                                 | 描述                                                                 |
|----------------------|---------------------------------------------------------------------|--------------------------------------------------------------------|
| `header`             | [Header](https://docs.ros.org/zh_CN/api/std_msgs/html/msg/Header.html) | 消息发布时间戳和坐标系ID                                           |
| `throttle`           | float32                                                             | 油门控制量：**[0.0, 1.0]**                                          |
| `steer`              | float32                                                             | 方向盘转向量：**[-1.0, 1.0]**                                       |
| `brake`              | float32                                                             | 刹车控制量：**[0.0, 1.0]**                                          |
| `hand_brake`         | bool                                                                | **True** 时启用手刹                                                 |
| `reverse`            | bool                                                                | **True** 时车辆倒车                                                 |
| `gear`               | int32                                                               | 切换车辆档位                                                       |
| `manual_gear_shift`  | bool                                                                | **True** 时启用 `gear` 参数进行手动换档                             |

---

（注：此处保留原始文档的代码块结构，完成技术术语中文化并保持表格格式）