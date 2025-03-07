# CARLA 资产目录

CARLA 模拟器提供丰富的3D资产库，可用于构建自动驾驶代理的虚拟环境。该3D资产库包含多个预设地图、多样化的交通车辆模型、行人模型以及可动态加入仿真的各类道路设施道具。本文档详细列出了仿真环境中可用的所有3D资产。

* [__车辆目录__](catalogue_vehicles_zhCN.md)
* [__行人目录__](catalogue_pedestrians_zhCN.md)
* [__道具目录__](catalogue_props_zhCN.md)

## 地图

提供两种预设环境：包含摩天大楼和居民区的现代都市，以及露天矿场工业场景。

| 地图名称       | 场景描述 |
| -----------| ------  |
| [__Town10__](map_town10.md) | 现代化都市环境，包含摩天大楼、住宅区及滨海步行道 |
| [__Mine__](map_mine.md) | 工业级露天矿场环境 |

!!! 注意
    1-9号城镇地图包含在源码编译版本中，但这些地图未针对虚幻引擎5进行更新和测试。使用这些地图时可能出现材质或几何体异常，如需使用需自行修复相关问题。

## 车辆

CARLA提供多款高精度车辆模型，包含真实世界中的轿车、卡车和摩托车等类型，满足交通仿真需求。完整列表请参见[__车辆目录__](catalogue_vehicles_zhCN.md)。

![车辆概览](../img/catalogue/vehicles/vehicle_montage.webp)

## 行人

资产库包含多样化行人模型，用于构建道路周边的人流场景。完整列表请参见[__行人目录__](catalogue_pedestrians_zhCN.md)。

![行人概览](../img/catalogue/pedestrians/pedestrians_overview.webp)

## 道具

提供丰富的道路设施模型，包含售货亭、雕塑、长椅、货箱、垃圾桶等场景元素，支持运行时动态加载。完整列表请参见[__道具目录__](catalogue_props_zhCN.md)。

![道具概览](../img/catalogue/props/props_overview.webp)