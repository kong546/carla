# 蓝图库
蓝图库 ([`carla.BlueprintLibrary`](../python_api/#carlablueprintlibrary-class)) 是CARLA中所有可用[`carla.ActorBlueprint`](../python_api/#carla.ActorBlueprint)及其属性([`carla.ActorAttribute`](../python_api/#carla.ActorAttribute))的汇总。

以下示例代码展示了如何打印所有actor蓝图及其属性：
```py
blueprints = [bp for bp in world.get_blueprint_library().filter('*')]
for blueprint in blueprints:
   print(blueprint.id)
   for attr in blueprint:
       print('  - {}'.format(attr))
```

请参考[蓝图基础介绍](core_actors.md)。

## 蓝图分类

### 行人蓝图（Walker）
- 标识符格式：`walker.pedestrian.*`
- 可修改属性：
  - `gender`：性别（男性/女性）
  - `speed`：行走速度（1.0-3.5m/s）

### 静态物体蓝图（Static）
- 标识符格式：`static.prop.*`
- 固定属性：
  - `collision`：碰撞检测开关
  - `texture`：材质贴图路径

### 载具蓝图（Vehicle）  
- 标识符格式：`vehicle.*`
- 关键参数：
  - `num_wheels`：车轮数量（不可修改）
  - `color`：车身颜色（RGB格式）

### 传感器蓝图（Sensor）
- 标识符格式：`sensor.*`
- 配置参数：
  - `range`：检测范围（米）
  - `fov`：视场角（角度）

## 属性类型说明
| 属性类型 | 描述 |
|---------|-----|
| Modifiable | 运行时可修改参数 |
| ReadOnly | 仅用于蓝图识别的固定属性 |

!警告 部分传感器蓝图需要特定地图版本支持，请参考[版本兼容性说明](build_update.md)