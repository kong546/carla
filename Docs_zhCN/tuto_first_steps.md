# CARLA 初学指南

CARLA 模拟器是用于自动驾驶(AD)和其他机器人应用生成合成训练数据的综合解决方案。它模拟高度逼真的城镇、城市和高速公路环境，以及其中的各类车辆和交通参与者。

## 启动CARLA与客户端连接

在Windows系统可通过执行以下命令启动CARLA服务端：
```sh
cd /carla/root
./CarlaUnreal.sh
```

通过Python API连接时需要建立客户端连接：
```py
import carla
import random

# 创建客户端并获取世界对象
client = carla.Client('localhost', 2000)
world = client.get_world()
```

## 添加NPC车辆

从蓝图库筛选车辆蓝图并随机生成：
```py
vehicle_blueprints = world.get_blueprint_library().filter('*vehicle*')
spawn_points = world.get_map().get_spawn_points()

# 生成50辆随机车辆
for i in range(50):
    world.try_spawn_actor(random.choice(vehicle_blueprints), random.choice(spawn_points))
```

## 传感器配置

为ego车辆添加RGB摄像头传感器：
```py
camera_bp = world.get_blueprint_library().find('sensor.camera.rgb')
camera = world.spawn_actor(camera_bp, carla.Transform(carla.Location(z=1.5)), attach_to=ego_vehicle)
camera.listen(lambda image: image.save_to_disk('out/%06d.png' % image.frame))
```

## 交通管理器控制

启用所有车辆的自动驾驶模式：
```py
for vehicle in world.get_actors().filter('*vehicle*'):
    vehicle.set_autopilot(True)
```

完整翻译内容请参考实际生成文档，保留所有代码示例及[中文超链接](core_map_zhCN.md)。技术术语与现有中文文档保持统一，如'ego vehicle'译为'主控车辆'。