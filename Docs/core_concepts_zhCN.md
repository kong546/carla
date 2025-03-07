# 核心概念

本文档介绍CARLA仿真平台的主要功能模块。各主题的详细说明可在对应章节中找到。

要了解API中的各类与方法，请参阅[Python API参考文档](python_api.md)。

*   [__基础概念__](#基础概念)
	*   [1. 世界与客户端](#1-世界与客户端)
	*   [2. 参与者与蓝图](#2-参与者与蓝图)
	*   [3. 地图与导航](#3-地图与导航)
	*   [4. 传感器与数据](#4-传感器与数据)
  *   [__高级功能__](#高级功能)

!!! 重要
    **本文档适用于CARLA 0.9.X版本**。<br>
    与旧版(0.8.X)API有重大变更，旧版文档请访问[此链接](https://carla.readthedocs.io/en/stable/getting_started/)。

---
## 基础概念

### 1. 世界与客户端

__客户端__是用户与仿真服务器交互的模块。客户端通过IP和端口与服务器通信，支持多客户端同时连接。高级多客户端管理需要深入理解[同步机制](adv_synchrony_timestep_zhCN.md)。

__世界对象__代表整个仿真环境，提供生成参与者、修改天气、获取世界状态等方法。每个仿真实例只有一个世界对象，切换地图时会创建新世界。

### 2. 参与者与蓝图

参与者指仿真环境中的动态实体：

* 车辆
* 行人
* 传感器
* 观察者
* 交通标志与信号灯

__蓝图__是预定义的参与者模板，包含模型、动画及可配置属性。所有可用蓝图参见[蓝图库](bp_library_zhCN.md)。

### 3. 地图与导航

__地图__代表仿真世界的场景，基于OpenDRIVE 1.4标准描述道路网络。通过[Python API](python_api.md)可访问道路、车道、路口等元素，配合__路径点__类实现导航。

交通标志通过[carla.Landmark](#python_api.md#carla.landmark)对象访问其OpenDRIVE定义，仿真运行时自动生成对应的碰撞边界框。

### 4. 传感器与数据

__传感器__监听特定事件并采集数据，类型包括：

* 摄像头（RGB/深度/语义分割）
* 碰撞检测器
* GNSS定位
* IMU惯性测量
* 激光雷达
* 车道入侵检测
* 障碍物检测
* 毫米波雷达

---
## 高级功能

CARLA提供以下高级特性：

* [__OpenDRIVE独立模式__](adv_opendrive_zhCN.md)：基于OpenDRIVE文件生成道路网格
* [__PTV-Vissim联合仿真__](adv_ptv_zhCN.md)：与PTV-Vissim交通仿真器同步运行
* [__记录回放系统__](adv_recorder_zhCN.md)：精确记录与复现仿真状态
* [__渲染设置__](adv_rendering_options_zhCN.md)：图形质量调节与无渲染模式
* [__时间同步机制__](adv_synchrony_timestep_zhCN.md)：仿真时间步长与客户端同步控制
* [__SUMO联合仿真__](adv_sumo_zhCN.md)：与SUMO交通仿真器协同运行
* [__交通管理器__](adv_traffic_manager_zhCN.md)：控制自动驾驶车辆的交通流模拟

---

<div text-align: center>
<div class="build-buttons">
<p>
<a href="https://github.com/carla-simulator/carla/discussions/" target="_blank" class="btn btn-neutral">CARLA论坛</a>
</p>
</div>
<div class="build-buttons">
<p>
<a href="../core_world_zhCN" target="_blank" class="btn btn-neutral">世界与客户端详解</a>
</p>
</div>
</div>