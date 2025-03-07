# 角色与蓝图

CARLA中的角色是模拟环境中执行动作并能影响其他角色的实体，包括车辆、行人、传感器、交通标志、交通灯以及观察者。理解如何操作这些角色至关重要。

本文档将涵盖角色的生成、处理、销毁以及各类角色的技术细节，更多可能性请参考[教程](tutorials.md)或在[CARLA论坛](https://github.com/carla-simulator/carla/discussions/)交流。

- [__蓝图__](#蓝图)  
	- [蓝图库管理](#蓝图库管理)  
- [__角色生命周期__](#角色生命周期)  
	- [生成](#生成)  
	- [处理](#处理)  
	- [销毁](#销毁)  
- [__角色类型__](#角色类型)  
	- [传感器](#传感器)  
	- [观察者](#观察者)  
	- [交通标志与信号灯](#交通标志与信号灯)  
	- [车辆](#车辆)  
	- [行人](#行人)  

---
## 蓝图

蓝图是预制模板，包含模型、动画及可定制属性（如车辆颜色、激光雷达通道数、行人速度等）。完整蓝图列表及属性说明请参考[蓝图库文档](bp_library_zhCN.md)，其中车辆和行人蓝图包含新旧版本标识。

### 蓝图库管理

通过[carla.BlueprintLibrary](python_api.md#carla.BlueprintLibrary)类访问蓝图库：
```py
blueprint_library = world.get_blueprint_library()
```
支持通过ID精确查找、随机选择或通配符过滤：
```py
# 查找特定蓝图
collision_sensor_bp = blueprint_library.find('sensor.other.collision')
# 随机选择车辆蓝图
vehicle_bp = random.choice(blueprint_library.filter('vehicle.*.*'))
```

蓝图属性支持读写操作：
```py
if vehicle.get_attribute('number_of_wheels') == 2:
    vehicle.set_attribute('color', '255,0,0')
```
!!! 注意
    部分属性不可修改，详见[蓝图库文档](bp_library_zhCN.md)。

---
## 角色生命周期

### 生成

使用[carla.World](python_api.md#carla.World)生成角色：
```py
transform = Transform(Location(x=230, y=195, z=40), Rotation(yaw=180))
actor = world.spawn_actor(blueprint, transform)
```

推荐使用生成点避免碰撞：
```py
# 车辆生成点
spawn_points = world.get_map().get_spawn_points()
# 行人生成点
spawn_point.location = world.get_random_location_from_navigation()
```

角色支持刚性/弹性绑定：
```py
camera = world.spawn_actor(camera_bp, relative_transform, attach_to=my_vehicle, carla.AttachmentType.Rigid)
```

### 处理

通过角色实例访问属性和状态：
```py
print(vehicle.get_velocity())
vehicle.set_simulate_physics(False)  # 冻结物理模拟
```

### 销毁

必须显式销毁角色：
```py
if actor.is_alive:
    destroyed = actor.destroy()
```

---
## 角色类型

### 传感器

传感器需绑定到父角色并设置回调函数：
```py
camera.listen(lambda image: image.save_to_disk('output/%06d.png' % image.frame))
```

### 交通信号系统

通过边界框检测信号影响：
```py
if vehicle.is_at_traffic_light():
    traffic_light = vehicle.get_traffic_light()
    traffic_light.set_state(carla.TrafficLightState.Green)
```

### 车辆控制

支持物理参数精细调整：
```py
physics_control = vehicle.get_physics_control()
physics_control.use_sweep_wheel_collision = True  # 启用精确碰撞检测
vehicle.apply_physics_control(physics_control)
```

完整技术细节请参考[CARLA官方文档](https://carla.readthedocs.io/)。

<div class="build-buttons">
<p>
<a href="../core_map_zhCN" class="btn btn-neutral">下一章：地图与导航</a>
</p>
</div>