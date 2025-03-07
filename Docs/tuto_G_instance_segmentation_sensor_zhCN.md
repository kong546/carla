# 实例分割传感器

*实例分割*是一种新型相机传感器，可为场景中的每个对象生成唯一的像素值。这与语义分割传感器不同，后者对同类对象实例（如车辆）使用相同ID。

要生成实例分割相机，需要使用`sensor.camera.instance_segmentation`蓝图：

```py
instance_camera_bp = world.get_blueprint_library().find('sensor.camera.instance_segmentation')
```

# 示例

我们将建立包含实例分割相机的世界，并在场景中生成多个车辆。

连接到服务器并设置为同步模式：

```py
import carla
import random
import time
import queue

# 连接客户端并设置CARLA服务器为同步模式
client = carla.Client('localhost', 2000)
world = client.get_world()
settings = world.get_settings()
settings.synchronous_mode = True
world.apply_settings(settings)
```

设置实例分割传感器并生成在指定位置：

```py
# 获取地图生成点和观察者
spawn_points = world.get_map().get_spawn_points()
spectator = world.get_spectator()

# 设置相机在场景中的位置
cam_location = carla.Location(x=-46., y=152, z=18)
cam_rotation = carla.Rotation(pitch=-21, yaw=-93.4, roll=0)
camera_transform = carla.Transform(location=cam_location, rotation=cam_rotation)
spectator.set_transform(camera_transform)

# 获取实例分割相机蓝图并生成相机
instance_camera_bp = world.get_blueprint_library().find('sensor.camera.instance_segmentation')
instance_camera = world.try_spawn_actor(instance_camera_bp, camera_transform)
```

在相机周围80米范围内生成车辆：

```py
# 在相机周围80米范围内生成车辆
vehicle_bp_library = world.get_blueprint_library().filter('*vehicle*')
radius = 80
for spawn_point in spawn_points:
    vec = [spawn_point.location.x - cam_location.x, spawn_point.location.y - cam_location.y]
    if vec[0]*vec[0] + vec[1]*vec[1] < radius*radius:
        world.try_spawn_actor(random.choice(vehicle_bp_library), spawn_point)
world.tick()
```

生成并保存实例分割图像：

```py
# 保存图像到本地
instance_image_queue = queue.Queue()
instance_camera.listen(instance_image_queue.put)
world.tick()
instance_image=instance_image_queue.get()
instance_image.save_to_disk('instance_segmentation.png')
```

## 图像输出

生成的实例分割图像将实例ID编码在RGB图像的G和B通道，R通道包含标准语义ID。

![instance_segmentation](img/instance_segmentation.png)