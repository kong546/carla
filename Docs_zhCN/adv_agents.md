# CARLA 智能体

CARLA智能体脚本允许车辆跟随随机无限路线或选择最短路径到达指定目的地。智能体会遵守交通信号灯并对道路上的其他障碍物做出反应。提供三种智能体类型，可修改目标速度、制动距离、跟车行为等参数。参与者类可被修改或作为基类来创建符合用户需求的自定义智能体。

- [__智能体脚本概览__](#智能体脚本概览)
    - [规划与控制](#规划与控制)
    - [智能体行为](#智能体行为)
- [__实现智能体__](#实现智能体)
- [__行为类型__](#行为类型)
    - [创建自定义行为类型](#创建自定义行为类型)
- [__创建智能体__](#创建智能体)

---

## 智能体脚本概览

CARLA智能体的核心脚本位于`PythonAPI/carla/agents/navigation`目录，主要分为两大类：__规划与控制__ 和 __智能体行为__。

### 规划与控制

- __`controller.py`__: 整合纵向和横向PID控制器为VehiclePIDController类，用于客户端对车辆的低级控制
- __`global_route_planner.py`__: 从CARLA服务器获取详细拓扑信息，构建世界地图的图结构，为局部规划器提供路径点和道路选项信息
- __`local_planner.py`__: 根据VehiclePIDController的控制输入跟踪路径点，路径点可由全局路径规划器提供或动态计算生成（在路口随机选择路径）

### 智能体行为

- __`basic_agent.py`__: 实现基础智能体，可漫游地图或通过最短路径到达目标，避让其他车辆并响应交通信号灯（忽略停止标志）
- __`behavior_agent.py`__: 实现复杂行为智能体，通过最短路径到达目标，遵守交通信号、标志和限速，具有跟车行为
- __`behavior_types.py`__: 定义行为智能体的参数配置：谨慎型、普通型和激进型

---

## 实现智能体

本节演示如何使用CARLA智能体类，文末将展示运行示例脚本的方法。

__1.__ 导入所需的智能体类：

```py
# 导入基础智能体
from agents.navigation.basic_agent import BasicAgent

# 导入行为智能体
from agents.navigation.behavior_agent import BehaviorAgent
```

__2.__ 将任意车辆转换为智能体（需先生成车辆）：

```py
# 启动基础智能体
agent = BasicAgent(vehicle)

# 启动激进型行为智能体
agent = BehaviorAgent(vehicle, behavior='aggressive')
```

__3.__ 设置目标位置（不设置目标则随机漫游）：

```py
destination = random.choice(spawn_points).location
agent.set_destination(destination)
```

__4.__ 在仿真循环中应用控制指令：

```py
while True:
    vehicle.apply_control(agent.run_step())
```

__5.__ 检测是否到达目的地：

```py
while True:
    if agent.done():
        print("已到达目标位置，停止仿真")
        break
    
    vehicle.apply_control(agent.run_step())
```

__6.__ 到达后生成新随机路线：

```py
while True:
    if agent.done():
        agent.set_destination(random.choice(spawn_points).location)
        print("已到达目标位置，搜索新目标")
    
    vehicle.apply_control(agent.run_step())
```

基础智能体提供以下关键方法：

- `set_target_speed()`: 设置目标速度(km/h)
- `follow_speed_limits()`: 启用限速遵守
- `set_destination()`: 设置起点到终点的最短路径
- `trace_route()`: 通过全局路径规划器获取两点间最短路径
- `ignore_traffic_lights()`: 设置是否遵守交通信号灯

运行示例脚本：

```sh
# 运行基础智能体
python3 automatic_control.py --agent=Basic

# 运行激进型行为智能体
python3 automatic_control.py --agent=Behavior --behavior=aggressive
```

---

## 行为类型

行为类型通过以下参数配置：

- `max_speed`: 最高时速(km/h)
- `speed_lim_dist`: 与当前限速保持的差值
- `safety_time`: 碰撞预估时间
- `braking_distance`: 紧急制动距离

## 创建自定义行为类型

__1.__ 在`behavior_types.py`中创建行为类

__2.__ 在`behavior_agent.py`中实例化新类型

---

## 创建智能体

自定义智能体需包含以下核心要素：

```py
class CustomAgent(BasicAgent):
    def __init__(self, vehicle, target_speed=20):
        super().__init__(target_speed)
        
    def run_step(self):
        control = carla.VehicleControl()
        # 添加自定义控制逻辑
        return control
```

建议参考`basic_agent.py`和`behavior_agent.py`的实现方式，根据需求扩展智能体功能。