# 同步性与时间步长

本节涉及CARLA中两个基本概念。它们的配置决定了仿真中的时间流逝方式以及服务器推进仿真的机制。

* [__仿真时间步长__](#仿真时间步长)
	* [可变时间步长](#可变时间步长)
	* [固定时间步长](#固定时间步长)
	* [录制仿真的注意事项](#录制仿真的注意事项)
	* [物理子步进](#物理子步进)
* [__客户端-服务器同步__](#客户端-服务器同步)
	* [设置同步模式](#设置同步模式)
	* [使用同步模式](#使用同步模式)
* [__可能的配置组合__](#可能的配置组合)
* [__物理确定性__](#物理确定性)

---
## 仿真时间步长

真实时间与仿真时间存在差异。仿真世界拥有由服务器控制的独立时钟。计算两个仿真步骤需要消耗真实时间，而这两个仿真时刻之间的时间跨度即为时间步长。

举例来说，服务器可能需要几毫秒真实时间来计算两个仿真步骤，但这两个步骤之间的仿真时间步长可配置为固定值（例如1秒）。

时间步长可根据用户需求设置为固定或可变模式。

!!! 注意
    时间步长与同步性概念紧密相关，建议完整阅读本节和同步模式章节以获得全面理解。

### 可变时间步长

CARLA默认模式。仿真步间时间等于服务器计算所需真实时间。

```py
settings = world.get_settings()
settings.fixed_delta_seconds = None # 设置为可变时间步长
world.apply_settings(settings)
```
通过`PythonAPI/util/config.py`脚本设置时，参数0表示可变时间步长：
```sh
cd PythonAPI/util && python3 config.py --delta-seconds 0
```

### 固定时间步长

步间时间保持恒定。设置为0.05秒时，每秒将进行20个仿真步骤。固定时间步长有利于数据采集，确保物理和传感器数据与明确的仿真时刻对应。若服务器性能足够，可在更短真实时间内模拟更长时间跨度。

```py
settings = world.get_settings()
settings.fixed_delta_seconds = 0.05
world.apply_settings(settings)
```
等效脚本命令：
```sh
cd PythonAPI/util && python3 config.py --delta-seconds 0.05
```

### 录制仿真的注意事项

使用CARLA的[录制功能](adv_recorder.md)时需注意：

* __固定时间步长__：重放时设置相同步长即可精确复现
* __可变时间步长__：
	* 服务器使用可变步长时，原始步长与重放步长不同，数据需插值处理
	* 强制复现原始步长会导致真实时间波动，可能产生异常时间表现
	* 浮点运算误差随步数累积，影响仿真精确性

### 物理子步进

为保证物理计算精度，CARLA默认启用物理子步进机制（最大10子步，每步最长0.01秒）：

```py
settings = world.get_settings()
settings.substepping = True
settings.max_substep_delta_time = 0.01
settings.max_substeps = 10
world.apply_settings(settings)
```

同步模式下需满足约束条件：
```py
fixed_delta_seconds <= max_substep_delta_time * max_substeps
```

建议子步长时间≤0.01秒以获得最佳物理精度。图示说明不同配置下的速度收敛情况：

>>>>>![固定物理步长时间的速度收敛](../img/physics_convergence_fixed_pdt.png)

>>>>>![变化物理步长时间的速度偏差](../img/physics_convergence_fixed_dt.png)

>>>>>![Z轴加速度收敛情况](../img/physics_convergence_z_acceleration.png)

---
## 客户端-服务器同步

CARLA采用客户端-服务器架构，默认运行在__异步模式__。__同步模式__下服务器等待客户端tick信号后才推进仿真。

!!! 注意
    多客户端架构中应仅有一个客户端发送tick信号，避免状态不一致。

### 设置同步模式

```py
settings = world.get_settings()
settings.synchronous_mode = True # 启用同步模式
world.apply_settings(settings)
```

!!! 警告
    启用同步模式时，需同时设置Traffic Manager为同步模式（参考[文档](adv_traffic_manager.md#同步模式)）。

禁用命令：
```sh
cd PythonAPI/util && python3 config.py --no-sync
```

### 同步模式应用

适用于客户端处理较慢或需要传感器数据同步的场景。示例相机同步代码：

```py
settings = world.get_settings()
settings.synchronous_mode = True
world.apply_settings(settings)

camera = world.spawn_actor(blueprint, transform)
image_queue = queue.Queue()
camera.listen(image_queue.put)

while True:
    world.tick()
    image = image_queue.get()
```

---
## 可能的配置组合

| 组合模式 | 固定时间步长 | 可变时间步长 |
|---------|-------------|-------------|
| 同步模式 | 完全控制仿真流程 | 物理仿真不可靠 |
| 异步模式 | 高效时间参考 | 仿真不可复现 |

* __同步+可变步长__：物理计算易出错，不推荐
* __异步+可变步长__：默认模式，存在浮点误差
* __异步+固定步长__：高效仿真长时间跨度
* __同步+固定步长__：精确控制，数据同步最佳实践

!!! 重要警告
    __同步模式必须配合固定时间步长使用__，可变步长会导致物理计算不稳定。

---
## 物理确定性

实现确定性物理需满足：
1. 启用同步模式和固定时间步长
2. 加载世界前启用同步设置
3. 每次重复实验前重新加载世界
4. 使用批量命令`apply_batch_sync`

示例配置：
```py
client = carla.Client(HOST, PORT)
client.set_timeout(10.0)
world = client.get_world()

# 加载地图并设置同步参数
client.load_world("Town10HD_Opt")
new_settings = world.get_settings()
new_settings.synchronous_mode = True
new_settings.fixed_delta_seconds = 0.05
world.apply_settings(new_settings)

# 配置交通管理器
client.reload_world(False)
traffic_manager = client.get_trafficmanager(TM_PORT)
traffic_manager.set_synchronous_mode(True)
traffic_manager.set_random_device_seed(SEED)

# 仿真循环
while True:
    world.tick()
```

完整执行流程可确保物理仿真的完全确定性。

<div class="build-buttons">
<p>
<a href="https://github.com/carla-simulator/carla/discussions/" target="_blank" class="btn btn-neutral" title="访问CARLA论坛">
CARLA论坛</a>
</p>
</div>