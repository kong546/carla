# Chrono 物理引擎集成

本文档介绍如何在 CARLA 中集成 Chrono 物理引擎，包含使用方法及功能限制说明。

- [__Chrono 项目__](#project-chrono)
- [__在 CARLA 中使用 Chrono__](#using-chrono-on-carla)
    - [服务器配置](#configuring-the-server)
    - [启用 Chrono 物理引擎](#enabling-chrono-physics)
- [__功能限制__](#limitations)

---

## Chrono 项目

[Chrono 项目](https://projectchrono.org/) 是一个开源多物理场仿真引擎，通过模板化方法提供高精度车辆动力学模拟。CARLA 集成版本允许用户使用 Chrono 模板进行车辆动力学仿真。

---

## 在 CARLA 中使用 Chrono

使用 Chrono 集成功能需要先配置服务器启动参数，然后通过 PythonAPI 在生成的车辆上启用。

### 服务器配置

启动 CARLA 服务器时必须使用 Chrono 编译标签。

__在源码编译的 CARLA 版本中__，使用以下命令启动服务器：

```sh
make launch ARGS="--chrono"
```

---

### 启用 Chrono 物理引擎

通过 [Actor 类](python_api_zhCN.md#carlaactor) 的 `enable_chrono_physics` 方法启用物理引擎。除子步数和子步时间间隔参数外，需要指定三个模板文件及其基础路径：

- __`base_path`:__ 模板文件所在目录路径，用于定位模板引用的辅助文件
- __`vehicle_json`:__ 车辆模板文件相对路径
- __`tire_json`:__ 轮胎模板文件相对路径
- __`powertrain_json`:__ 动力总成模板文件相对路径

!!! 重要
    请仔细检查文件路径，错误路径可能导致虚幻引擎崩溃。

模板文件示例位于 `Build/chrono-install/share/chrono/data/vehicle` 目录。参考 Chrono [官方文档](https://api.projectchrono.org/manual_vehicle.html) 了解模板创建方法。

启用 Chrono 物理引擎的代码示例：

```python
    # 生成车辆
    vehicle = world.spawn_actor(bp, spawn_point)

    # 设置基础路径
    base_path = "/path/to/carla/Build/chrono-install/share/chrono/data/vehicle/"

    # 指定模板文件
    vehicle_json = "sedan/vehicle/Sedan_Vehicle.json"
    powertrain_json = "sedan/powertrain/Sedan_SimpleMapPowertrain.json"
    tire_json = "sedan/tire/Sedan_TMeasyTire.json"

    # 启用 Chrono 物理引擎
    vehicle.enable_chrono_physics(5000, 0.002, vehicle_json, powertrain_json, tire_json, base_path)
```

可通过 `PythonAPI/examples` 目录下的 `manual_control_chrono.py` 脚本体验 Chrono 物理效果，运行后按 `Ctrl + o` 启用。

---

### 功能限制

当前集成版本不支持碰撞检测。__发生碰撞时，物理仿真将自动切换回 CARLA 默认物理引擎。__