# 录制器

本功能允许记录并重现场景仿真。所有发生的事件都存储在[录制文件](ref_recorder_binary_file_format.md)中，并提供高级查询功能用于追踪和研究这些事件。

* [__录制功能__](#录制功能)
* [__仿真回放__](#仿真回放)
  * [设置时间系数](#设置时间系数)
* [__录制文件__](#录制文件)
* [__数据查询__](#数据查询)
  * [碰撞检测](#碰撞检测)
  * [受阻参与者](#受阻参与者)
* [__Python脚本示例__](#python脚本示例)

---
## 录制功能

所有数据以二进制格式存储在服务端。录制功能通过[carla.Client](python_api.md#carla.Client)接口管理。

每帧根据录制文件数据更新参与者状态。当前仿真中存在于录制的参与者会被移动或重新生成，未涉及的参与者将保持原有行为。

!!! 重要
    回放结束时，车辆将切换为自动驾驶模式，但__行人会停止__。

录制文件包含以下元素信息：

* __参与者__ — 创建/销毁、边界框/触发框
* __交通信号灯__ — 状态变更与时间设置
* __车辆__ — 位置/朝向、线速度/角速度、灯光状态、物理控制参数
* __行人__ — 位置/朝向、线速度/角速度
* __灯光__ — 建筑/街道/车辆的灯光状态

开始录制只需指定文件名。使用`\`、`/`或`:`字符表示绝对路径，否则文件将保存在`CarlaUnreal/Saved`目录。

```py
client.start_recorder("/home/carla/recording01.log")
```

默认仅记录必要回放数据。如需完整记录，需设置`additional_data`参数：

```py
client.start_recorder("/home/carla/recording01.log", True)
```

!!! 注意
    附加数据包含：车辆/行人线速度与角速度、交通灯时间设置、执行时间、参与者触发框/边界框、车辆物理控制参数。

停止录制命令：

```py
client.stop_recorder()
```

!!! 注意
    估算数据量：包含50个交通灯和100辆车的1小时录制约占用200MB空间。

---
## 仿真回放

可在仿真过程中随时开始回放，需指定日志文件路径及以下参数：

```py
client.replay_file("recording01.log", start, duration, camera)
```

| 参数        | 说明                                                                 | 备注                                                                 |
|-----------|--------------------------------------------------------------------|--------------------------------------------------------------------|
| start     | 开始时间（秒）                                                          | 正值从开始计算，负值从末尾倒推                                              |
| duration  | 回放时长（0表示全部）                                                      | 回放结束车辆切为自动驾驶，行人停止                                            |
| camera    | 摄像机聚焦的参与者ID                                                      | 设为0允许自由视角                                                      |

### 设置时间系数

时间系数控制回放速度，可实时调整：

```py
client.set_replayer_time_factor(2.0)
```

| 参数        | 默认值    | 快进模式    | 慢放模式    |
|-----------|---------|---------|---------|
| time_factor | **1.0** | **>1.0** | **<1.0** |

!!! 重要
    当`time_factor>2.0`时，参与者位置插值功能关闭，行人动画不受时间系数影响。

20倍速时可清晰观察交通流：
![flow](img/RecorderFlow2.gif)

---
## 录制文件

通过API调用可获取录制详细信息。默认显示关键事件帧，设置`show_all`参数可查看完整帧数据。文件格式详见[录制器参考文档](ref_recorder_binary_file_format.md)。

```py
# 显示关键帧信息
print(client.show_recorder_file_info("recording01.log"))
```

输出包含：
* __起始信息__ — 地图名称、录制时间
* __帧信息__ — 参与者生成/销毁等事件
* __结束信息__ — 总帧数与时长

```
版本: 1
地图: Town05
日期: 2019/02/21 10:46:20

第1帧 0秒
 创建2190: spectator (0) 位置(-260, -200, 382.001)
 创建2191: traffic.traffic_light (3) 位置(4255, 10020, 0)
...

第2350帧 60.2805秒
 销毁2276

总帧数: 2354
总时长: 60.3753秒
```

---
## 数据查询

### 碰撞检测

需为车辆附加[碰撞传感器](ref_sensors.md#collision-detector)。查询支持按参与者类型过滤：

* h = 主控车辆
* v = 车辆
* w = 行人
* t = 交通灯
* o = 其他
* a = 全部

示例：查询车辆与其他对象的碰撞

```py
print(client.show_recorder_collisions("recording01.log", "v", "a"))
```

输出格式：
```
版本: 1
地图: Town05
日期: 2019/02/19 15:36:08

    时间  类型     ID 主体1                                ID 主体2
      16   v v     122 vehicle.yamaha.yzf             118 vehicle.dodge_charger.police

总帧数: 790
总时长: 46秒
```

### 受阻参与者

检测长时间停滞的车辆，停滞条件由用户定义：

```py
print(client.show_recorder_actors_blocked("recording01.log", min_time, min_distance))
```

| 参数         | 说明                        | 默认值     |
|------------|---------------------------|---------|
| min_time   | 最小停滞时间（秒）               | 30秒     |
| min_distance | 最小移动距离（厘米）             | 10厘米    |

示例：查询60秒内移动不足1米的车辆

```py
client.show_recorder_actors_blocked("col3.log", 60, 100)
```

输出按停滞时长排序：
```
    时间    ID 参与者                         停滞时长
      36    173 vehicle.nissan.patrol         336秒
```

---
## Python脚本示例

`PythonAPI/examples`包含实用脚本：

* __start_recording.py__ — 启动录制
  * `-f` 文件名
  * `-n` 生成车辆数（默认10）
  * `-t` 录制时长

* __start_replaying.py__ — 开始回放
  * `-f` 文件名
  * `-s` 起始时间
  * `-d` 回放时长
  * `-c` 跟随的参与者ID

* __show_recorder_collisions.py__ — 碰撞查询
  * `-t` 参与者类型组合（如vv表示车车碰撞）

* __show_recorder_actors_blocked.py__ — 受阻车辆查询
  * `-t` 停滞时间阈值
  * `-d` 移动距离阈值

<div class="build-buttons">
<p>
<a href="https://github.com/carla-simulator/carla/discussions/" target="_blank" class="btn btn-neutral">
CARLA论坛</a>
</p>
</div>