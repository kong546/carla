# CarSim 集成指南

CARLA 与 CarSim 的集成允许将车辆控制指令转发至 CarSim，由 CarSim 完成所有车辆物理计算后，将新状态返回给 CARLA。

本文档将指导您如何生成 `.sim` 文件，解释 CARLA 与 CarSim 的车辆尺寸对应关系，并演示如何在 CARLA 中运行 CarSim 集成仿真。

*   [__准备工作__](#准备工作)  
*   [__配置 CarSim__](#配置-carsim)  
	*   [__生成 .sim 文件__](#生成-sim-文件)  
        * [__Windows 系统__](#windows-系统)
        * [__Ubuntu 系统__](#ubuntu-系统)
	*   [__车辆尺寸对应__](#车辆尺寸对应)  
*   [__运行仿真__](#运行仿真)  

---
## 准备工作

1. 需持有有效的 CarSim 软件许可证。如需获取许可证，请通过[此链接](https://www.carsim.com/forms/additional_information.php)联系 CarSim 团队。
2. 安装适用于 Unreal Engine 4.24 的 VehicleSim Dynamics 插件（2020.0 版）：
   
    __Windows 系统__：
    
    从[虚幻引擎商城](https://www.unrealengine.com/marketplace/en-US/product/carsim-vehicle-dynamics)获取插件。

    __Ubuntu 系统__：
    
    1. 从[CarSim 官网](https://www.carsim.com/users/unreal_plugin/unreal_plugin_2020_0.php)下载插件
    2. 使用[此文件](https://carla-releases.s3.eu-west-3.amazonaws.com/Backup/CarSim.Build.cs)替换原版 `CarSim.Build.cs` 以确保 Ubuntu 兼容性

3. 若从源码构建 CARLA，需在编译时添加 `--carsim` 参数：

```sh
make launch ARGS="--carsim"
```

## 配置 CarSim

### 生成 .sim 文件

.sim 文件定义了 CARLA 与 CarSim 的联合仿真配置，是运行仿真的必要文件。

#### Windows 系统

通过 CarSim GUI 配置参数后，按图示生成 .sim 文件：

![生成 .sim 文件](img/carsim_generate.jpg)

生成的 .sim 文件示例如下：

```
（保持原始代码块内容不变）
```

#### Ubuntu 系统

Ubuntu 需通过以下步骤使用预生成的 .sim 文件：

1. 使用 Windows 生成的 .sim 文件或下方模板
2. 修改 `INPUT`、`INPUTARCHIVE` 等路径指向 Ubuntu 系统实际路径
3. 将 `DLLFILE` 替换为 `SOFILE /opt/carsim_2020.0/lib64/libcarsim.so.2020.0`

```
（保持原始代码块内容不变）
```

### 车辆尺寸对应

CarSim 与 CARLA 的车辆尺寸无直接关联，CARLA 车辆仅作为仿真占位符：

![车辆尺寸对比](img/carsim_vehicle_sizes.jpg)

!!! 重要提示
    CARLA 与 CarSim 的车辆尺寸无对应关系，CARLA 车辆仅作为物理仿真的占位符。

## 运行仿真

通过 Python API 启用 CarSim 集成：

```sh
vehicle.enable_carsim(<sim文件路径>)
```

车辆控制指令将转发至 CarSim 进行物理计算，计算结果返回更新 CARLA 车辆状态。仿真结束后可通过 CarSim 进行数据分析。

![CarSim 数据分析](img/carsim_analysis.jpg)