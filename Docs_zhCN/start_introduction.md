# CARLA

!!! important
    本文档适用于基于Unreal Engine 5构建的最新版CARLA。如需Unreal Engine 4版本的文档，请访问[该版本文档](https://carla.readthedocs.io/en/latest/)。

CARLA 是为自动驾驶研究开发的开源模拟器。它采用模块化架构和灵活API，致力于解决自动驾驶领域中的各类技术挑战。CARLA的核心目标是降低自动驾驶研发门槛，通过提供可自由定制的工具满足不同场景需求（如驾驶策略学习、感知算法训练等）。该模拟器基于Unreal Engine实现，采用OpenDRIVE 1.4标准定义道路环境，并通过持续完善的Python/C++ API提供仿真控制。

---
## 模拟器架构

CARLA采用可扩展的客户端-服务器架构：
- **服务器端**：负责传感器渲染、物理计算、世界状态更新等核心仿真功能，需要配备高性能GPU
- **客户端**：通过Python/C++ API控制场景中的智能体行为和环境状态，该接口持续迭代增强功能

![系统模块示意图](img/carla_modules.png)

关键功能模块包括：

* __交通管理器__：接管非学习车辆的控制权，生成逼真的城市交通流
* __传感器系统__：支持摄像头、雷达、激光雷达等多种传感器数据采集与存储
* __场景回放__：精确复现仿真过程，支持不同传感器配置的对比测试
* __ROS2集成__：原生支持机器人操作系统ROS2的通信接口
* __开放资产库__：提供可自定义的城市地图、天气条件和角色蓝图
* __场景编排器__：预设多种驾驶场景路线，支撑[CARLA挑战赛](https://carlachallenge.org/)的算法评估

---
## 开源生态

CARLA遵循开源理念构建透明技术生态：
- 开发者社区共同推进自动驾驶技术探索
- 提供完整工具链支持算法开发、测试与验证
- 通过文档体系（[快速入门](start_quickstart_zhCN.md) | [编译指南](build_carla_zhCN.md)）降低使用门槛

欢迎加入CARLA社区！

<div class="build-buttons">
<p>
<a href="../build_linux_ue5_zhCN" target="_blank" class="btn btn-neutral">
<b>Linux</b> 编译指南</a>
</p>
<p>
<a href="../build_windows_ue5_zhCN" target="_blank" class="btn btn-neutral">
<b>Windows</b> 编译指南</a>
</p>
</div>