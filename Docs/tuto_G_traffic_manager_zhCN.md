# 交通管理器

当我们训练神经网络控制自动驾驶车辆时，自动驾驶智能体面临的关键挑战之一就是处理其他道路使用者。除了识别道路拓扑、保持车道纪律外，自动驾驶智能体还必须识别其他车辆并预判其行为对规划路径的影响。CARLA的交通管理器(TM)可以管理仿真环境中的车辆群体，为主车（即我们训练或控制的车辆）创建障碍和挑战场景。

本教程将介绍交通管理器的功能，以及如何在仿真中使用它来创建和控制非玩家角色(NPC)车辆，这些车辆会像真实道路使用者一样在路网中活动。

## 设置仿真器并初始化交通管理器

首先我们初始化TM并在城市中随机生成交通流：

```py
import carla
import random

# 连接客户端并获取世界对象
client = carla.Client('localhost', 2000)
world = client.get_world()

# 设置同步模式
settings = world.get_settings()
settings.synchronous_mode = True # 启用同步模式
settings.fixed_delta_seconds = 0.05
world.apply_settings(settings)

# 设置交通管理器同步模式
traffic_manager = client.get_trafficmanager()
traffic_manager.set_synchronous_mode(True)

# 设置随机种子保证可重复性
traffic_manager.set_random_device_seed(0)
random.seed(0)

# 设置观察者视角
spectator = world.get_spectator()
```

## 生成车辆

创建TM车辆需要地图生成点。每个CARLA地图都预定义了均匀分布在路网中的生成点，我们可以直接使用这些点：

```py
spawn_points = world.get_map().get_spawn_points()
```

使用调试功能可视化生成点位置：

```py
# 在地图上标注生成点编号
for i, spawn_point in enumerate(spawn_points):
    world.debug.draw_string(spawn_point.location, str(i), life_time=10)

# 同步模式下需持续运行仿真
while True:
    world.tick()
```

生成车辆实例：

```py
# 从蓝图库选择车辆模型
models = ['dodge', 'audi', 'model3', 'mini', 'mustang', 'lincoln', 'prius', 'nissan', 'crown', 'impala']
blueprints = []
for vehicle in world.get_blueprint_library().filter('*vehicle*'):
    if any(model in vehicle.id for model in models):
        blueprints.append(vehicle)

# 设置最大车辆数
max_vehicles = 50
max_vehicles = min([max_vehicles, len(spawn_points)])
vehicles = []

# 随机选择生成点生成车辆
for i, spawn_point in enumerate(random.sample(spawn_points, max_vehicles)):
    temp = world.try_spawn_actor(random.choice(blueprints), spawn_point)
    if temp is not None:
        vehicles.append(temp)

# 运行仿真观察结果
while True:
    world.tick()
```

## 通过交通管理器控制车辆

启用自动驾驶模式并设置交通灯忽略概率：

```py
# 为所有车辆启用自动驾驶
for vehicle in vehicles:
    vehicle.set_autopilot(True)
    # 随机设置交通灯忽略概率(0-50%)
    traffic_manager.ignore_lights_percentage(vehicle, random.randint(0,50))

while True:
    world.tick()
```

![路口交通](../img/tuto_G_traffic_manager/traffic.gif)

## 指定车辆路径

使用set_path()创建汇聚交通流：

```py
# 标注路径点
spawn_points = world.get_map().get_spawn_points()

# 路径1
route_1_indices = [129, 28, 124, 33, 97, 119, 58, 154, 147]
route_1 = [spawn_points[ind].location for ind in route_1_indices]

# 路径2
route_2_indices = [21, 76, 38, 34, 90, 3]
route_2 = [spawn_points[ind].location for ind in route_2_indices]

# 可视化路径点
world.debug.draw_string(spawn_points[32].location, '生成点1', life_time=30, color=carla.Color(255,0,0))
world.debug.draw_string(spawn_points[149].location, '生成点2', life_time=30, color=carla.Color(0,0,255))

while True:
    world.tick()
```

动态生成汇聚车流：

```py
spawn_delay = 20
counter = spawn_delay
max_vehicles = 200
alt = False

while True:
    world.tick()
    
    if counter == spawn_delay and len(world.get_actors().filter('*vehicle*')) < max_vehicles:
        vehicle = world.try_spawn_actor(random.choice(blueprints), 
                                      spawn_points[32] if alt else spawn_points[149])
        
        if vehicle:
            vehicle.set_autopilot(True)
            # 禁用变道行为
            traffic_manager.auto_lane_change(vehicle, False)
            # 交替设置路径
            traffic_manager.set_path(vehicle, route_1 if alt else route_2)
            alt = not alt
        
    counter = spawn_delay if counter == 0 else counter -1
```

![汇聚路径](../img/tuto_G_traffic_manager/converging_paths.gif)