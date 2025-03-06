# 性能基准测试

我们提供基准测试脚本，方便用户分析CARLA在本地环境中的性能表现。该脚本可配置多种场景组合，包含不同地图、传感器和天气条件，并输出指定场景下的平均帧率及标准差。

本节详细说明运行基准测试的要求、脚本位置、参数配置及使用示例。同时包含我们在特定环境下进行的独立测试结果，展示不同车辆数量、物理模拟开关、交通管理器(Traffic Manager)开关等组合的性能表现，结果均标注使用的CARLA版本和测试环境。

- [__基准测试脚本__](#基准测试脚本)
    - [__准备工作__](#准备工作)
    - [__参数说明__](#参数说明)
        - [__命令行参数__](#命令行参数)
- [__CARLA性能报告__](#carla性能报告)

---
## 基准测试脚本

脚本位于`PythonAPI/util`目录，支持以下参数配置测试场景：

### 准备工作

运行前需安装依赖：
```python
python -m pip install -U py-cpuinfo==5.0.0 psutil python-tr gpuinfo GPUtil
```

### 参数说明

`python3` [`performance_benchmark.py`](https://github.com/carla-simulator/carla/blob/master/PythonAPI/util/performance_benchmark.py) [`[--host 主机]`](#-host-ip地址) [`[--port 端口]`](#-port-端口) [`[--file 文件]`](#-file-文件名md) [`[--tm]`](#-tm)
[`[--ticks 帧数]`](#-ticks) [`[--sync]`](#-sync) [`[--async]`](#-async))
[`[--fixed_dt 固定时间步]`](#-fixed_dt) [`[--render_mode]`](#-render_mode))
[`[--no_render_mode]`](#-no_render_mode) [`[--show_scenarios]`](#-show_scenarios))
[`[--sensors 传感器 [传感器 ...]]`](#-sensors-整数))
[`[--maps 地图 [地图 ...]]`](#-maps-城镇名称))
[`[--weather 天气 [天气 ...]]`](#-weather-整数)

#### 命令行参数

* `--host`: __IP地址__  
__默认__: 本地主机  
设置服务器主机地址

* `--port`: __端口__  
__默认__: 2000  
设置TCP监听端口

* `--file`: __文件名.md__  
__默认__: benchmark.md  
以Markdown表格格式输出结果

* `--tm`  
切换至交通管理器基准测试

* `--ticks`  
__默认__: 100  
设置每个场景运行的帧数

* `--sync`  
__默认模式__  
同步模式运行测试

* `--async`  
异步模式运行测试

* `--fixed_dt`  
__默认__: 0.05  
同步模式下的固定时间步长

* `--render_mode`  
开启渲染模式

* `--no_render_mode`  
__默认模式__  
关闭渲染模式

* `--show_scenarios`  
单独使用时显示所有可用场景参数，配合其他参数时预览将运行的场景

* `--sensors`: __整数__  
>> __默认__: 全部  
选择使用的传感器类型：
>> * __0__: 300x200摄像头
>> * __1__: 800x600摄像头
>> * __2__: 1900x1080摄像头
>> * __3__: 双300x200摄像头
>> * __4__: 10万点激光雷达
>> * __5__: 50万点激光雷达
>> * __6__: 100万点激光雷达

* `--maps`: __城镇名称__  
__默认__: 全部地图  
支持所有[CARLA地图][carla_maps]

[carla_maps]: https://carla.readthedocs.io/zh_CN/latest/core_map/#carla地图

* `--weather`: __整数__  
__默认__: 全部天气  
天气条件选项：
* __0__: 晴朗正午
* __1__: 多云正午
* __2__: 微雨日落

## 运行基准测试

1. 启动CARLA：
```shell
# Linux:
./CarlaUnreal.sh
# Windows:
CarlaUnreal.exe
# 源码编译:
cmake --build Build --target launch
```

2. 在另一个终端中进入`PythonAPI/util`目录：

>> * 查看所有场景参数：
```shell
python3 performance_benchmark.py --show_scenarios
```

>> * 预览指定配置的场景：
```shell
python3 performance_benchmark.py --sensors 2 5 --maps Town03 Town05 --weather 0 1 --show_scenarios
```

>> * 执行指定场景的测试：
```shell
python3 performance_benchmark.py --sensors 2 5 --maps Town03 Town05 --weather 0 1
```

>> * 异步渲染模式测试：
```shell
python3 performance_benchmark.py --async --render_mode
```

---
<div class="build-buttons">
<p>
<a href="https://github.com/carla-simulator/carla/discussions/" target="_blank" class="btn btn-neutral" title="访问CARLA论坛">
CARLA论坛</a>
</p>
</div>