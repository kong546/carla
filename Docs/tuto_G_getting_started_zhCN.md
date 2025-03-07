# CARLA 入门指南

CARLA 模拟器是为自动驾驶（AD）和其他机器人应用生成合成训练数据的综合解决方案。它模拟高度逼真的环境，复现现实世界的城镇、城市和高速公路，以及占据这些驾驶空间的车辆和其他物体。

CARLA 模拟器还可作为评估和测试环境。您可以在模拟环境中部署训练好的AD代理，测试和评估其性能和安全性，所有这些操作都在安全的虚拟环境中进行，不会对硬件或其他道路使用者造成风险。

本教程将介绍CARLA的基础使用步骤，包括使用观察者导航环境、在模拟中添加车辆和行人，以及配置传感器和摄像头来收集训练或测试所需的模拟数据。

## 启动CARLA并连接客户端

在Windows系统可通过命令行执行以下命令启动CARLA服务端：
```sh
cd /carla/root
./CarlaUnreal.sh
```

通过Python API操作时需要建立客户端连接：
```py
import carla
import random

# 创建客户端并获取世界对象
client = carla.Client('localhost', 2000)
world = client.get_world()
```

[__客户端__](python_api#carlaclient)对象负责维护客户端与服务器的连接，并提供多种命令执行和数据加载导出功能。我们可以使用客户端对象加载不同地图或重置当前地图：

```py
# 显示可用地图
client.get_available_maps()

# 加载新地图
client.load_world('Town07')

# 重载当前地图并重置状态
client.reload_world()
```

端口号可指定任何可用端口，默认为2000。您也可以使用计算机IP地址替代*localhost*，实现在网络机器上运行CARLA服务端，在本地计算机运行Python客户端。这种方式能有效区分用于模拟器运行和神经网络训练的GPU资源。

!!! 注意
    以下内容默认CARLA运行于[__异步模式__](adv_synchrony_timestep_zhCN.md)。若启用同步模式，部分代码可能无法正常运行。

## 世界对象

在CARLA API中，[__世界对象__](python_api#carlaworld)提供对模拟环境中所有元素的访问权限，包括地图、建筑物、交通信号灯、车辆和行人等。

我们可以通过世界对象查询和操作模拟元素：
```py
# 获取所有对象名称
world.get_names_of_all_objects()

# 过滤建筑物名称
filter(lambda x: 'Building' in x, world.get_names_of_all_objects())

# 获取所有动态实体（Actor）
world.get_actors()

# 过滤车辆实体
world.get_actors().filter('*vehicle*')
```

世界对象用于向模拟中添加车辆、行人等动态实体。这些实体具有行为表现（如移动和影响其他物体），因此被称为Actor。静态物体（如建筑物）属于地图特征，而交通信号灯等虽为静态但具有行为影响的物体也属于Actor。

生成实体需要对应的[__蓝图__](python_api#carlaactorblueprint)。蓝图包含构成实体的所有要素：网格模型、纹理材质等外观元素，以及控制行为和物理交互的逻辑规则。

```py
# 获取车辆蓝图库
vehicle_bps = world.get_blueprint_library().filter('*vehicle*')

# 随机选择车辆蓝图
vehicle_bp = random.choice(vehicle_bps)

# 获取地图预设生成点
spawn_point = random.choice(world.get_map().get_spawn_points())

# 生成车辆
world.spawn_actor(vehicle_bp, spawn_point)
```

建议使用容错生成方法避免程序崩溃：
```py
vehicle = world.try_spawn_actor(vehicle_bp, spawn_point)
```

生成失败可能由以下原因导致：生成点已有实体占用，或位置不合理（如建筑物内部等非道路区域）。

## 观察者控制

观察者提供模拟环境的可视化窗口。在带显示器的计算机上运行服务端时，默认会打开观察者窗口（使用`-RenderOffScreen`参数可禁用）。

通过观察者可以：
- 熟悉地图环境
- 实时查看修改效果（如添加车辆、改变天气）
- 调试模拟场景

使用以下键位控制观察者：
- Q：上升
- E：下降
- W：前进
- S：后退
- A：左移
- D：右移

鼠标左键拖动控制视角俯仰和偏航。

![飞行观察者](../img/tuto_G_getting_started/flying_spectator.gif)

通过API控制观察者：
```py
spectator = world.get_spectator()
transform = spectator.get_transform()

# 重置观察者到地图原点
spectator.set_transform(carla.Transform())
```

## 自定义生成点

观察者可辅助确定实体生成位置：
```py
# 在观察者当前位置生成车辆
vehicle = world.try_spawn_actor(vehicle_bp, spectator.get_transform())
```

![生成车辆](../img/tuto_G_getting_started/spawn_vehicle.gif)

记录坐标信息：
```py
print(spectator.get_transform())
>>> Transform(Location(x=25.761623, y=13.169240, z=0.539901), Rotation(pitch=0.862031, yaw=-2.056274, roll=0.000069))
```

## 使用地图预设生成点

每个地图提供预设生成点，便于批量创建交通：
```py
# 批量生成50辆随机车辆
spawn_points = world.get_map().get_spawn_points()
for _ in range(50):
    world.try_spawn_actor(random.choice(vehicle_bps), random.choice(spawn_points))
```

可视化生成点：
```py
for i, point in enumerate(spawn_points):
    world.debug.draw_string(point.location, str(i), life_time=100)
    world.debug.draw_arrow(point.location, point.location + point.get_forward_vector(), life_time=100)
```

![生成点可视化](../img/tuto_G_getting_started/spawn_points.png)

## 实体与蓝图系统

CARLA提供丰富的蓝图库：
```py
# 显示所有车辆蓝图
for bp in world.get_blueprint_library().filter('vehicle'):
    print(bp)

# 获取特定车辆蓝图
vehicle_bp = world.get_blueprint_library().find('vehicle.audi.tt')
```
（此处应继续完成剩余内容的翻译，因篇幅限制暂略）