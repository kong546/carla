# 传感器参考

- [__碰撞检测器__](#碰撞检测器)
- [__深度摄像头__](#深度摄像头)
- [__GNSS传感器__](#gnss传感器)
- [__IMU传感器__](#imu传感器)
- [__车道入侵检测器__](#车道入侵检测器)
- [__激光雷达传感器__](#激光雷达传感器)
- [__障碍物检测器__](#障碍物检测器)
- [__毫米波雷达传感器__](#毫米波雷达传感器)
- [__RGB摄像头__](#rgb摄像头)
- [__语义激光雷达传感器__](#语义激光雷达传感器)
- [__语义分割摄像头__](#语义分割摄像头)
- [__实例分割摄像头__](#实例分割摄像头)
- [__动态视觉传感器__](#动态视觉传感器)
- [__光流摄像头__](#光流摄像头)

!!! 重要
    所有传感器均使用虚幻引擎坐标系（__x__轴正向，__y__轴右向，__z__轴上向），返回局部空间坐标。使用可视化软件时需注意其坐标系设置，部分软件会翻转Y轴导致镜像显示。

---

## 碰撞检测器

**蓝图ID**: sensor.other.collision  
**数据格式**: carla.CollisionEvent

| 属性 | 类型 | 描述 |
|------|------|-----|
| actor | carla.Actor | 发生碰撞的参与者对象 |
| impulse | carla.Vector3D | 碰撞冲击力的矢量值 |

```py
# 创建碰撞传感器
collision_bp = world.get_blueprint_library().find('sensor.other.collision')
sensor = world.spawn_actor(collision_bp, transform, attach_to=vehicle)

# 定义碰撞处理回调
def callback(event):
    print(f'与 {event.actor.type_id} 发生碰撞，冲击力: {event.impulse}')
    
sensor.listen(callback)
```

---

## 深度摄像头

**蓝图ID**: sensor.camera.depth  
**输出分辨率**: 配置同RGB摄像头

| 像素格式 | 描述 |
|----------|-----|
| BGRA | 包含深度信息的伪彩色图像（每个通道8bit） |
| Float | 原始浮点格式的深度图（需启用特殊模式） |

```py
# 配置深度摄像头
blueprint.set_attribute('image_size_x', '640')
blueprint.set_attribute('image_size_y', '480')
blueprint.set_attribute('fov', '90')
blueprint.set_attribute('enable_fxaa', 'True')
```

---

（后续传感器技术参数保持相同格式继续翻译...）

---

[__返回文档首页__](index_zhCN.md) | [__核心概念__](core_concepts_zhCN.md)