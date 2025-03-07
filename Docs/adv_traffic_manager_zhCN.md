# 高级交通管理器指南

本文档详细说明CARLA交通管理器的核心功能，包括车辆行为控制、交通流配置和混合物理模式等技术细节。

## 交通生成规则

```python
# 示例：配置车辆生成密度
traffic_manager.set_global_percentage_speed_difference(30)
traffic_manager.set_random_device_seed(config.seed)
```

## 混合物理模式参数
| 参数 | 描述 | 默认值 |
|------|------|--------|
| `hybrid_physics_radius` | 物理模拟范围（米） | 70 |
| `global_percentage_speed_difference` | 全局车速差异百分比 | ±30% |

## 全局交通行为配置

⚠️ 警告：修改碰撞检测设置将影响仿真确定性
```python
traffic_manager.set_collision_detection(reference_actor, enable=True)
```

完整保留原文超链接结构：[CARLA论坛讨论](https://github.com/carla-simulator/carla/discussions)