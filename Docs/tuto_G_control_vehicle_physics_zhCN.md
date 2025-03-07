# 控制与监控车辆物理属性

车辆及车轮的物理参数可以在运行时进行动态调整。
所有修改仅在运行时生效，程序结束后参数将恢复默认值。

这些属性通过[carla.VehiclePhysicsControl](python_api.md#carla.VehiclePhysicsControl)对象控制，
并通过[carla.WheelPhysicsControl](python_api.md#carla.WheelPhysicsControl)对象实现对每个车轮的独立物理控制。

- [__车辆控制代码示例__](#车辆控制代码示例)
- [__查看车辆遥测数据__](#查看车辆遥测数据)

---
## 车辆控制代码示例

```py
import carla
import random

def main():
    # 连接客户端
    client = carla.Client('127.0.0.1', 2000)
    client.set_timeout(2.0)

    # 获取世界和参与者列表
    world = client.get_world()
    actors = world.get_actors()

    # 随机选择一辆车辆（场景中至少应存在一辆）
    vehicle = random.choice([actor for actor in actors if 'vehicle' in actor.type_id])

    # 创建车轮物理控制参数
    front_left_wheel  = carla.WheelPhysicsControl(tire_friction=2.0, max_steer_angle=70.0)
    front_right_wheel = carla.WheelPhysicsControl(tire_friction=2.0, max_steer_angle=70.0)
    rear_left_wheel   = carla.WheelPhysicsControl(tire_friction=3.0, max_steer_angle=0.0)
    rear_right_wheel  = carla.WheelPhysicsControl(tire_friction=3.0, max_steer_angle=0.0)

    wheels = [front_left_wheel, front_right_wheel, rear_left_wheel, rear_right_wheel]

    # 修改车辆物理控制参数
    physics_control = vehicle.get_physics_control()

    physics_control.torque_curve = [carla.Vector2D(x=0, y=400), carla.Vector2D(x=1300, y=600)]
    physics_control.max_rpm = 10000
    physics_control.moi = 1.0
    physics_control.use_gear_autobox = True
    physics_control.gear_switch_time = 0.5
    physics_control.mass = 10000
    physics_control.drag_coefficient = 0.25
    physics_control.steering_curve = [carla.Vector2D(x=0, y=1), carla.Vector2D(x=100, y=1), carla.Vector2D(x=300, y=1)]
    physics_control.use_sweep_wheel_collision = True
    physics_control.wheels = wheels

    # 应用新的物理控制参数
    vehicle.apply_physics_control(physics_control)
    print(physics_control)

if __name__ == '__main__':
    main()
```

---

## 查看车辆遥测数据

通过调用[`Actor.enable_debug_telemetry`](python_api.md#carla.Actor.enable_debug_telemetry)方法可实时可视化车辆遥测数据。该功能将在服务器窗口显示各项指标的曲线图，并在仿真界面展示车辆参考点。

![车辆遥测](img/vehicle_telemetry.png)

可在`PythonAPI/examples`目录下的`manual_control.py`示例脚本中体验该功能，通过按下`T`键激活遥测视图。