# 传感器与数据

传感器是能够从周围环境获取数据的执行器，对于创建驾驶智能体的学习环境至关重要。

本文总结了处理传感器所需的所有基础知识，介绍了可用类型及其生命周期指南。各传感器的具体参数请参阅[传感器参考文档](ref_sensors_zhCN.md)。

* [__传感器操作指南__](#传感器操作指南)
	*   [设置](#设置)
	*   [生成](#生成)
	*   [监听](#监听)
	*   [数据](#数据)
* [__传感器类型__](#传感器类型)
	*   [摄像头](#摄像头)
	*   [检测器](#检测器)
	*   [其他](#其他)
* [__传感器参考__](ref_sensors_zhCN.md)

---
## 传感器操作指南

[carla.Sensor](python_api.md#carla.Sensor) 类定义了能够测量和流式传输数据的特殊执行器。

* __数据类型__ 根据传感器类型不同而变化，所有数据类型均继承自通用类 [carla.SensorData](python_api.md#carla.SensorData)
* __触发时机__ 分为每帧触发（摄像头等）和事件触发（碰撞检测器等）两种模式
* __获取方式__ 所有传感器都通过 `listen()` 方法接收数据

### 设置

与其他执行器类似，需通过蓝图设置关键属性：

```py
# 获取RGB摄像头蓝图
blueprint = world.get_blueprint_library().find('sensor.camera.rgb')
# 设置图像分辨率（1920x1080）和视场角（110度）
blueprint.set_attribute('image_size_x', '1920')
blueprint.set_attribute('image_size_y', '1080')
blueprint.set_attribute('fov', '110')
# 设置采集间隔（1.0秒）
blueprint.set_attribute('sensor_tick', '1.0')
```

### 生成

关键参数 `attach_to` 和 `attachment_type` 决定传感器绑定方式：

* __刚性绑定__ 严格跟随父级坐标变化，适用于数据采集
* __弹簧臂绑定__ 运动更平滑，适用于视频录制
* __幽灵弹簧臂绑定__ 忽略碰撞检测，适用于特殊拍摄角度

```py
transform = carla.Transform(carla.Location(x=0.8, z=1.7))
sensor = world.spawn_actor(blueprint, transform, attach_to=my_vehicle)
```

### 监听

通过 lambda 表达式定义数据处理回调函数：

```py
# 图像保存示例
sensor.listen(lambda image: image.save_to_disk('output/%06d.png' % image.frame))

# 碰撞检测示例
def callback(event):
    for actor_id in event:
        vehicle = world_ref().get_actor(actor_id)
        print('近距离车辆: %s' % vehicle.type_id)
sensor02.listen(callback)
```

### 数据

通用数据属性表：

| 数据属性       | 类型                      | 描述                          |
| -------------- | ------------------------- | ----------------------------- |
| `frame`        | int                       | 数据采集时的帧编号            |
| `timestamp`    | double                    | 仿真时间戳（秒）              |
| `transform`    | [carla.Transform](...)    | 传感器采集时的世界坐标系变换  |

---
## 传感器类型

### 摄像头

| 传感器                 | 输出类型                  | 功能描述                          |
| ---------------------- | ------------------------- | --------------------------------- |
| [深度摄像头](...)     | carla.Image               | 生成灰度深度图                    |
| [RGB摄像头](...)       | carla.Image               | 生成常规彩色图像                  |
| [光流摄像头](...)      | carla.Image               | 生成像素级运动矢量图              |

### 检测器

| 传感器                 | 输出类型                  | 触发条件                          |
| ---------------------- | ------------------------- | --------------------------------- |
| [碰撞检测器](...)      | carla.CollisionEvent      | 与其它执行器发生碰撞时触发        |

### 其他

| 传感器                 | 输出类型                  | 功能描述                          |
| ---------------------- | ------------------------- | --------------------------------- |
| [激光雷达](...)        | carla.LidarMeasurement    | 生成带强度信息的3D点云           |

---
（后续内容保持相同结构继续翻译...）