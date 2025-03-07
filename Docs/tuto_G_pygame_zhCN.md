# 使用 Pygame 控制车辆

[__Pygame__](https://www.pygame.org/news) 是一个跨平台的 Python 模块集合，可用于编写视频游戏。它提供了一种有效的方式来实时渲染 CARLA 的视觉输出以监控传感器数据（如摄像头），同时能够捕获键盘事件，是控制车辆等参与者的理想方式。

本教程将学习如何搭建简单的 Pygame 界面，用于监控交通管理器（TM）控制的自动驾驶交通，并通过键盘手动接管车辆控制。

## 设置模拟器并初始化交通管理器

首先初始化 TM 并在城市中随机生成交通流。

```py
import carla
import random
import pygame
import numpy as np

# 连接到客户端并获取世界对象
client = carla.Client('localhost', 2000)
world = client.get_world()

# 设置同步模式
settings = world.get_settings()
settings.synchronous_mode = True # 启用同步模式
settings.fixed_delta_seconds = 0.05
world.apply_settings(settings)

# 配置交通管理器同步模式
traffic_manager = client.get_trafficmanager()
traffic_manager.set_synchronous_mode(True)

# 设置随机种子确保可复现行为
traffic_manager.set_random_device_seed(0)
random.seed(0)

# 设置观察视角
spectator = world.get_spectator()
```

## 生成车辆

在城市各处的生成点创建车辆集群并交由 TM 控制。

```py
# 获取地图生成点
spawn_points = world.get_map().get_spawn_points()

# 从蓝图库选择车辆模型
models = ['dodge', 'audi', 'model3', 'mini', 'mustang', 'lincoln', 'prius', 'nissan', 'crown', 'impala']
blueprints = []
for vehicle in world.get_blueprint_library().filter('*vehicle*'):
    if any(model in vehicle.id for model in models):
        blueprints.append(vehicle)

# 设置最大车辆数并生成
max_vehicles = 50
max_vehicles = min([max_vehicles, len(spawn_points)])
vehicles = []

# 随机选择生成点生成车辆
for i, spawn_point in enumerate(random.sample(spawn_points, max_vehicles)):
    temp = world.try_spawn_actor(random.choice(blueprints), spawn_point)
    if temp is not None:
        vehicles.append(temp)

# 将所有生成车辆设为自动驾驶模式
for vehicle in vehicles:
    vehicle.set_autopilot(True)
    # 随机设置闯红灯概率
    traffic_manager.ignore_lights_percentage(vehicle, random.randint(0,50))
```

## 使用 Pygame 渲染摄像头输出并控制车辆

生成交通流后，设置跟随车辆的摄像头并通过键盘控制接管逻辑。

```py
# 创建渲染表面对象
class RenderObject(object):
    def __init__(self, width, height):
        init_image = np.random.randint(0,255,(height,width,3),dtype='uint8')
        self.surface = pygame.surfarray.make_surface(init_image.swapaxes(0,1))

# 摄像头回调函数，将原始数据转换为 Pygame 表面
def pygame_callback(data, obj):
    img = np.reshape(np.copy(data.raw_data), (data.height, data.width, 4))
    img = img[:,:,:3]
    img = img[:, :, ::-1]
    obj.surface = pygame.surfarray.make_surface(img.swapaxes(0,1))
```

创建车辆控制逻辑对象，实现键盘交互：

```py
class ControlObject(object):
    def __init__(self, veh):
        self._vehicle = veh
        self._steer = 0
        self._throttle = False
        self._brake = False
        self._steer_cache = 0
        self._control = carla.VehicleControl()

    def parse_control(self, event):
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_RETURN:
                self._vehicle.set_autopilot(False)
            if event.key == pygame.K_UP:
                self._throttle = True
            if event.key == pygame.K_DOWN:
                self._brake = True
            if event.key == pygame.K_RIGHT:
                self._steer = 1
            if event.key == pygame.K_LEFT:
                self._steer = -1
        # 处理按键释放事件...

    def process_control(self):
        # 实现油门、刹车和转向逻辑...
        self._vehicle.apply_control(self._control)
```

初始化车辆和摄像头后，设置 Pygame 显示窗口：

```py
# 初始化 Pygame 显示界面
pygame.init()
gameDisplay = pygame.display.set_mode((image_w,image_h), pygame.HWSURFACE | pygame.DOUBLEBUF)
gameDisplay.fill((0,0,0))
gameDisplay.blit(renderObject.surface, (0,0))
pygame.display.flip()
```

最后实现主游戏循环，支持 TAB 键切换车辆，ENTER 键接管控制：

```py
while not crashed:
    world.tick()
    gameDisplay.blit(renderObject.surface, (0,0))
    pygame.display.flip()
    controlObject.process_control()
    # 处理退出事件和车辆切换逻辑...
```

![手动控制演示](../img/tuto_G_pygame/manual_control.gif)

完整实现细节请参考配套代码。该方案可用于测试自动驾驶算法在异常交通场景下的表现。