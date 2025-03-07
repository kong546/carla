# CARLA Unreal Engine 5 文档

!!! 注意
    目前有两种不同版本的CARLA模拟器在积极开发中，一个基于Unreal Engine 4，另一个基于Unreal Engine 5。由于两个版本在功能和特性上的差异，请确保使用对应版本的文档。如果您正在使用Unreal Engine 4版本的CARLA，请参考[该版本的对应文档](https://carla.readthedocs.org)。

欢迎来到CARLA Unreal Engine 5文档。

本主页包含文档各章节的索引及简要说明。您可以按任意顺序阅读，以下是给新用户的建议：

* __安装CARLA__：通过[快速安装指南](start_quickstart.md)获取发行版，或根据目标平台[从源码构建](build_carla.md)。
* __开始使用__：[基础概念](foundations.md)章节介绍CARLA的核心概念，[入门教程](tuto_first_steps.md)演示如何创建仿真场景。
* __API参考__：方便的[Python API参考](python_api.md)可供查询可用类和方法。

CARLA论坛可供您与其他社区成员交流：
<div class="build-buttons">
<a href="https://github.com/carla-simulator/carla/discussions/" target="_blank" class="btn btn-neutral" title="访问最新CARLA版本">
CARLA论坛</a>
</div>

<br>

---

## 入门指南

[__简介__](start_introduction.md) —— 了解CARLA的功能特性  
[__快速安装__](start_quickstart.md) —— 安装预编译包并快速开始  
[__首次使用__](tuto_G_getting_started.md) —— 通过Python API进行初次体验  
[__源码编译__](build_carla.md) —— 在Linux和Windows系统上构建CARLA

## 核心组件
[__基础概念__](core_concepts.md) —— CARLA的核心架构解析  
[__参与者__](core_actors.md) —— 了解各类Actor及其管理方式  
[__地图__](core_map.md) —— 探索不同地图及车辆导航机制  
[__传感器与数据__](core_sensors.md) —— 使用传感器获取仿真数据  
[__交通仿真__](ts_traffic_simulation_overview.md) —— 场景交通生成方案总览

## 资源索引
[__Python API__](python_api.md) —— Python接口类与方法说明  
[__资产目录__](catalogue.md) —— 可用地图、车辆、行人及道具清单  
[__蓝图库__](bp_library.md) —— 生成Actor的预制蓝图  
[__教程集合__](tutorials.md) —— 重要功能使用教程  
[__C++参考__](ref_cpp.md) —— C++接口说明

## 生态系统
[__MathWorks__](large_map_roadrunner.md) —— RoadRunner地图创建指南  
[__ASAM OpenDRIVE__](adv_opendrive.md) —— OpenDRIVE标准支持详情  
[__ROS2__](ros2_native.md) —— 原生ROS2接口使用方法  
[__Scenic__](tuto_G_scenic.md) —— 使用Scenic生成测试场景  
[__SYNKROTRON__](ecosys_syncrotron.md) —— 基于CARLA的仿真解决方案  
[__Inverted AI__](inverted_ai.md) —— 生成式AI交通仿真方案

## 贡献指南
[__贡献规范__](cont_contribution_guidelines.md) —— 参与CARLA开发的指导原则  
[__编码标准__](cont_coding_standard.md) —— 代码开发最佳实践  
[__文档标准__](cont_doc_standard.md) —— 文档编写规范

<!-- ## 扩展文档

以上章节涵盖了CARLA的核心概念和功能，更多高级功能请参考[扩展文档](ext_docs.md) -->