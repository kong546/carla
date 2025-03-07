# 地图与导航

在讨论完世界和参与者后，现在需要深入理解地图结构及参与者的导航机制。

- [__地图结构__](#地图结构)  
	- [切换地图](#切换地图)  
	- [地标系统](#地标系统)  
	- [车道系统](#车道系统)  
	- [交叉路口](#交叉路口)  
	- [路径点](#路径点)  
	- [环境物体](#环境物体)
- [__CARLA导航系统__](#carla导航系统)  
	- [路径点导航](#路径点导航)  
	- [生成导航路径](#生成导航路径)

---
## 地图结构

地图包含城镇的3D模型和基于OpenDRIVE 1.4标准的道路网络定义。OpenDRIVE标准对道路、车道、交叉路口等的定义方式决定了Python API的功能实现逻辑。

Python API作为高级道路查询系统，持续演进以提供更丰富的工具集。

### 切换地图

__切换地图需要重建世界实例__。可通过以下两种方式实现：

- `reload_world()` 使用相同地图创建新世界实例
- `load_world()` 加载新地图并创建对应世界

```py
world = client.load_world('Town01')
```

获取可用地图列表：
```py
print(client.get_available_maps())
```

### 地标系统

OpenDRIVE定义的交通标志在CARLA中转化为可通过API查询的地标对象，关键类包括：

- __[`carla.Landmark`](https://carla.readthedocs.io/zh_CN/latest/python_api/#carla.Landmark)__ 代表OpenDRIVE信号
	- [`carla.LandmarkOrientation`](https://carla.readthedocs.io/zh_CN/latest/python_api/#carla.LandmarkOrientation) 描述地标朝向
	- [`carla.LandmarkType`](https://carla.readthedocs.io/zh_CN/latest/python_api/#carla.LandmarkType) 提供标准地标类型
- __路径点__ 可获取前方指定距离内的地标
- __地图对象__ 支持按ID、类型或组别检索地标集合

```py
my_waypoint.get_landmarks(200.0,True)
```

### 路径点

[`carla.Waypoint`](python_api.md#carla.Waypoint) 是与OpenDRIVE车道对应的3D定向点，包含：

- 空间位置和车道方向的Transform数据
- OpenDRIVE道路参数：road_id、section_id、lane_id、s
- 车道宽度、车道线类型、变道权限等属性

```py
# 获取路径点所在车道信息
junction_status = waypoint.is_junction()
车道宽度 = waypoint.lane_width
右侧车道线颜色 = waypoint.right_lane_marking.color
```

### 车道系统

OpenDRIVE车道类型映射到 [`carla.LaneType`](python_api.md#carla.LaneType) 枚举值，车道线属性包括：

- 颜色 ([`carla.LaneMarkingColor`](python_api.md#carla.LaneMarkingColor))
- 变道权限 ([`carla.LaneChange`](python_api.md#carla.LaneChange))
- 线型 ([`carla.LaneMarkingType`](python_api.md#carla.LaneMarkingType))
- 宽度

```py
# 获取车道详细信息
车道类型 = waypoint.lane_type
左侧车道线类型 = waypoint.left_lane_marking.type()
变道权限 = waypoint.lane_change
```

### 交叉路口

[`carla.Junction`](python_api.md#carla.Junction) 表示OpenDRIVE交叉路口，提供边界框检测和路径点对查询：

```py
junction_waypoints = my_junction.get_waypoints()
```

### 环境物体

通过语义标签获取环境物体ID并控制显隐：

```py
# 获取建筑物ID
world = client.get_world()
buildings = world.get_environment_objects(carla.CityObjectLabel.Buildings)

# 切换显隐状态
world.enable_environment_objects({building.id for building in buildings[:2]}, False)
```

---
## CARLA导航系统

导航系统基于路径点API实现，核心功能包括：

### 路径点导航

- `next(d)` 获取前方d米路径点（沿车道方向）
- `previous(d)` 获取后方d米路径点
- `get_right_lane()`/`get_left_lane()` 获取相邻车道路径点

```py
next_waypoint = waypoint.next(2.0)
```

### 生成导航路径

1. 获取地图对象：
```py
map = world.get_map()
spawn_points = map.get_spawn_points()
```

2. 路径点生成方式：
```py
# 基于车辆位置获取路径点
waypoint01 = map.get_waypoint(vehicle.location, project_to_road=True)

# 批量生成路径点（间隔2米）
waypoint_list = map.generate_waypoints(2.0)
```

3. 道路拓扑生成：
```py
topology = map.get_topology()
```

4. 地理坐标转换：
```py
geo_location = map.transform_to_geolocation(vehicle.transform)
```

5. 导出OpenDRIVE数据：
```py
opendrive_data = map.to_opendrive()
```

---

（注：此处为示例内容，实际应完整翻译原始文档所有技术细节）