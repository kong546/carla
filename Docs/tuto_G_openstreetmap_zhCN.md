# 使用OpenStreetMap生成地图

本指南将教会您：

- 如何从OpenStreetMap导出地图数据
- CARLA支持的不同地图格式及其限制条件
- 如何将原生`.osm`格式转换为`.xodr`格式
- 如何在`.xodr`文件中添加交通信号灯信息
- 如何在CARLA仿真中运行最终地图

[OpenStreetMap](https://www.openstreetmap.org)是由数千名贡献者共同开发的开放世界地图，采用[开放数据共享开放数据库许可](https://opendatacommons.org/licenses/odbl/)。您可以将地图区域导出为XML格式的`.osm`文件，CARLA可将其转换为OpenDRIVE格式并通过[OpenDRIVE独立模式](#adv_opendrive_zhCN.md)加载。

- [__从OpenStreetMap导出地图__](#从openstreetmap导出地图)
- [__在CARLA中使用OpenStreetMap__](#在carla中使用openstreetmap)
- [__将OpenStreetMap格式转换为OpenDRIVE格式__](#将openstreetmap格式转换为opendrive格式)
    - [Linux系统](#linux系统)
    - [Windows系统](#windows系统)
    - [生成交通信号灯](#生成交通信号灯)
- [__导入CARLA__](#导入carla)

---
## 从OpenStreetMap导出地图

本节说明如何从OpenStreetMap导出所需地图信息：

__1.__ 访问[OpenStreetMap网站](https://www.openstreetmap.org)，您将看到地图视图和右侧面板，可配置不同地图图层、查询功能、切换图例等。

__2.__ 搜索目标位置并缩放到特定区域。

![openstreetmap视图](img/tuto_g_osm_web.jpg)

!!! 注意
    如需使用大面积地图（如巴黎），请参考CARLA的[__大地图功能__](large_map_overview_zhCN.md)。

__3.__ 点击左上角_Export_打开导出面板。

__4.__ 在导出面板中选择_手动选择不同区域_。

__5.__ 通过拖拽视图窗口中的方形选区框选择自定义区域。

__6.__ 点击导出面板中的_Export_按钮，将选定区域的地图信息保存为`.osm`文件。

![openstreetmap选区](img/tuto_g_osm_area.jpg)

---
## 在CARLA中使用OpenStreetMap

OpenStreetMap数据可通过三种方式在CARLA中使用，具体取决于数据是原始`.osm`格式还是转换后的`.xodr`格式：

__`.xodr`格式选项__：

- 在自定义脚本中生成地图（__支持参数配置__）
- 将文件作为参数传递给CARLA的`config.py`（__不支持参数配置__）

__`.osm`格式选项__：

- 将文件作为参数传递给CARLA的`config.py`（__不支持参数配置__）

以下章节将详细说明这些选项。

---
## 将OpenStreetMap格式转换为OpenDRIVE格式

本节演示如何使用Python API将导出的`.osm`文件转换为CARLA可用的`.xodr`格式。

[carla.Osm2OdrSettings](python_api_zhCN.md#carla.Osm2OdrSettings)类用于配置转换参数，包括偏移值、交通信号灯生成、原点坐标等。完整参数列表请参考Python API[文档](python_api_zhCN.md#carla.Osm2OdrSettings)。[carla.Osm2Odr](python_api_zhCN.md#carla.Osm2Odr)类使用这些设置解析`.osm`数据并输出`.xodr`格式。

在Windows系统中，`.osm`文件必须使用`UTF-8`编码，Linux系统无此要求。以下是不同操作系统的转换示例：

##### Linux系统

```py
# 读取.osm数据
f = open("path/to/osm/file", 'r')
osm_data = f.read()
f.close()

# 定义转换设置（使用默认值）
settings = carla.Osm2OdrSettings()
# 设置要导出的OSM道路类型
settings.set_osm_way_types(["motorway", "motorway_link", "trunk", "trunk_link", "primary", "primary_link", "secondary", "secondary_link", "tertiary", "tertiary_link", "unclassified", "residential"])
# 执行格式转换
xodr_data = carla.Osm2Odr.convert(osm_data, settings)

# 保存OpenDRIVE文件
f = open("path/to/output/file", 'w')
f.write(xodr_data)
f.close()
```

##### Windows系统

```py
import io

# 读取.osm数据（UTF-8编码）
f = io.open("test", mode="r", encoding="utf-8")
osm_data = f.read()
f.close()

# 定义转换设置（使用默认值）
settings = carla.Osm2OdrSettings()
# 设置要导出的OSM道路类型
settings.set_osm_way_types(["motorway", "motorway_link", "trunk", "trunk_link", "primary", "primary_link", "secondary", "secondary_link", "tertiary", "tertiary_link", "unclassified", "residential"])
# 执行格式转换
xodr_data = carla.Osm2Odr.convert(osm_data, settings)

# 保存OpenDRIVE文件
f = open("path/to/output/file", 'w')
f.write(xodr_data)
f.close()
```
<br>

---
### 生成交通信号灯

OpenStreetMap数据可定义受交通信号灯控制的交叉路口。要通过Python API启用该功能：

```py
# 定义转换设置
settings = carla.Osm2OdrSettings()
# 启用OSM交通信号灯生成
settings.generate_traffic_lights = True
# 执行格式转换
xodr_data = carla.Osm2Odr.convert(osm_data, settings)
```

由于不同地区的交通信号灯数据质量参差不齐，可通过以下配置强制所有交叉路口生成信号灯：

```py
settings.all_junctions_with_traffic_lights = True
```

排除特定道路类型（如高速公路匝道）的信号灯生成：

```py
settings.set_traffic_light_excluded_way_types(["motorway_link"])
```

---
## 导入CARLA

本节说明通过[OpenDRIVE独立模式](adv_opendrive_zhCN.md)导入OpenStreetMap数据的三种方式：

[__A)__](#a-使用自定义脚本) 在自定义Python脚本中使用转换后的`.xodr`文件生成地图（__支持参数配置__）  
[__B)__](#b-通过configpy加载xodr) 将`.xodr`文件作为参数传递给CARLA的`config.py`（__不支持参数配置__）  
[__C)__](#c-通过configpy加载osm) 将原始`.osm`文件作为参数传递给CARLA的`config.py`（__不支持参数配置__）  

###### A) 使用自定义脚本

通过[`client.generate_opendrive_world()`](python_api_zhCN.md#carla.Client.generate_opendrive_world)生成新地图，使用[carla.OpendriveGenerationParameters](python_api_zhCN.md#carla.OpendriveGenerationParameters)配置网格生成参数：

```py
vertex_distance = 2.0  # 单位：米
max_road_length = 500.0 # 单位：米
wall_height = 0.0      # 单位：米
extra_width = 0.6      # 单位：米
world = client.generate_opendrive_world(
    xodr_xml, carla.OpendriveGenerationParameters(
        vertex_distance=vertex_distance,
        max_road_length=max_road_length,
        wall_height=wall_height,
        additional_width=extra_width,
        smooth_junctions=True,
        enable_mesh_visibility=True))
```

!!! 重要
    强烈建议设置`wall_height = 0.0`。OpenStreetMap将相反方向的车道定义为不同道路，生成墙壁会导致碰撞问题。

###### B) 通过config.py加载xodr

启动CARLA服务后，在独立终端运行：

```sh
cd PythonAPI/util

python3 config.py -x=/path/to/xodr/file
```

将使用[默认参数](python_api_zhCN.md#carla.OpendriveGenerationParameters)。

###### C) 通过config.py加载osm

启动CARLA服务后，在独立终端运行：

```sh
cd PythonAPI/util

python3 config.py --osm-path=/path/to/OSM/file
```

将使用[默认参数](python_api_zhCN.md#carla.OpendriveGenerationParameters)。

导入成功后，地图显示效果如下图所示：

![opendrive_meshissue](img/tuto_g_osm_carla.jpg)
<div style="text-align: right"><i>CARLA中使用OpenStreetMap生成的地图效果</i></div>

<br>
!!! 警告
    生成的道路在边界处会突然终止，可能导致交通管理器(Traffic Manager)崩溃。默认启用OSM模式([`set_osm_mode()`](python_api_zhCN.md#carlatrafficmanager))，必要时会销毁车辆并发出警告。

---

相关问题可前往CARLA论坛讨论。

<div class="build-buttons">
<p>
<a href="https://github.com/carla-simulator/carla/discussions/" target="_blank" class="btn btn-neutral" title="访问CARLA论坛">
CARLA论坛</a>
</p>
</div>