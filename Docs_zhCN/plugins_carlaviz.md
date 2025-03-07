# carlaviz

CARLA可视化插件用于在网页浏览器中实时呈现仿真场景。该插件将创建一个包含基础场景元素的窗口，支持动态更新参与者状态、获取传感器数据，并可在场景中绘制附加文本、线条和多段线。

* [__基础信息__](#基础信息)  
	* [支持版本](#支持版本)  
* [__获取carlaviz__](#获取carlaviz)  
	* [先决条件](#先决条件)  
	* [下载插件](#下载插件)  
* [__功能使用__](#功能使用)  

---
## 基础信息

* __贡献者__ — 徐敏俊（GitHub ID [wx9698](https://github.com/wx9698))  
* __许可证__ — [MIT](https://en.wikipedia.org/wiki/MIT_License)  

### 支持版本

* __Linux__ — CARLA 0.9.6, 0.9.7, 0.9.8, 0.9.9, 0.9.10  
* __Windows__ — CARLA 0.9.9, 0.9.10  
* __源码编译版__ — 最新更新  

---
## 获取carlaviz

### 先决条件

* __Docker__ — 访问[官方文档](https://docs.docker.com/get-docker/)安装Docker  
* __操作系统__ — 支持运行CARLA的任何系统  
* __Websocket客户端__ — 执行`pip3 install websocket_client`。若系统未安装pip，请先[安装pip](https://pip.pypa.io/en/stable/installing/)  

### 下载插件

根据使用的CARLA版本，在终端拉取对应的Docker镜像：

```bash
# 拉取与CARLA版本匹配的镜像
docker pull mjxu96/carlaviz:0.9.6
docker pull mjxu96/carlaviz:0.9.7
docker pull mjxu96/carlaviz:0.9.8
docker pull mjxu96/carlaviz:0.9.9
docker pull mjxu96/carlaviz:0.9.10

# 源码编译版使用此镜像
docker pull mjxu96/carlaviz:latest
```

!!! Important
    Windows系统目前仅支持0.9.9和0.9.10版本

CARLA 0.9.9及更早版本采用单数据流模式。后续版本实现了传感器的多数据流模式：

* __单数据流模式__下，传感器只能被一个客户端监听。当传感器已被其他客户端（如`manual_control.py`）占用时，carlaviz需要复制传感器实例，可能影响性能
* __多数据流模式__下，传感器可被多个客户端同时监听，无需复制实例

!!! Note
    Linux用户可参考[构建文档](https://github.com/carla-simulator/carlaviz/blob/master/docs/build.md)自行编译，但建议使用Docker镜像更便捷

---
## 运行carlaviz

__1. 启动CARLA__

* __a) 发行版__ — 进入CARLA目录，执行`CarlaUnreal.exe`（Windows）或`./CarlaUnreal.sh`（Linux）
* __b) 源码版__ — 进入CARLA目录，执行`make launch`启动UE编辑器并点击`Play`

__2. 启动carlaviz__ 在新终端执行对应Docker镜像命令（替换<镜像名称>为实际拉取的镜像名）：

```sh
# Linux系统
docker run -it --network="host" -e CARLAVIZ_HOST_IP=localhost -e CARLA_SERVER_IP=localhost -e CARLA_SERVER_PORT=2000 <镜像名称>

# Windows/MacOS系统
docker run -it -e CARLAVIZ_HOST_IP=localhost -e CARLA_SERVER_IP=host.docker.internal -e CARLA_SERVER_PORT=2000 -p 8080-8081:8080-8081 -p 8089:8089 <镜像名称>
```

成功启动后终端将显示类似提示：
![carlaviz_run](img/plugins_carlaviz_run.jpg)

!!! Warning
    请确保使用正确的Docker镜像名称

__3. 访问网页端__ 浏览器打开`http://127.0.0.1:8080/`，默认界面如下：
![carlaviz_empty](img/plugins_carlaviz_empty.jpg)

---
## 功能使用

插件运行后可实时可视化仿真场景、参与者状态及传感器数据。界面右侧为3D可视化窗口，左侧为功能面板：

* __视角模式__ — 切换观察视角
	* `俯视` — 上帝视角
	* `透视` — 自由视角
	* `驾驶` — 第一人称视角

* __/vehicle__ — 展示主控车辆属性
	* `/velocity` — 车速仪表
	* `/acceleration` — 加速度仪表

* __/drawing__ — 显示[CarlaPainter](https://github.com/wx9698/carlaviz/blob/master/examples/carla_painter.py)绘制的元素
	* `/texts` — 文本标注
	* `/points` — 点元素
	* `/polylines` — 折线元素

* __/objects__ — 场景参与者管理
	* `/walkers` — 行人更新
	* `/vehicles` — 车辆更新

* __/game__ — 仿真元数据
	* `/time` — 仿真时间和帧数

* __/lidar__ — 激光雷达数据
	* `/points` — 点云可视化

* __/radar__ — 毫米波雷达数据
	* `/points` — 探测点显示

* __/traffic__ — 交通要素
	* `/traffic_light` — 交通信号灯
	* `/stop_sign` — 停止标志

生成交通流测试：
```sh
cd PythonAPI/examples
python3 generate_traffic.py -n 10 -w 5
```
![carlaviz_full](img/plugins_carlaviz_full.jpg)

主控车辆测试：
```sh
cd PythonAPI/examples
python3 manual_control.py
```
![carlaviz_data](img/plugins_carlaviz_data.jpg)

使用[CarlaPainter](https://github.com/wx9698/carlaviz/blob/master/examples/carla_painter.py)可绘制自定义元素，参考[示例代码](https://github.com/carla-simulator/carlaviz/blob/master/examples/example.py)实现激光雷达数据可视化：
![carlaviz_demo](img/plugins_carlaviz_demo.jpg)

---

本教程涵盖carlaviz核心功能，更多疑问欢迎访问论坛讨论：

<div class="build-buttons">
<p>
<a href="https://github.com/carla-simulator/carla/discussions/" target="_blank" class="btn btn-neutral" title="访问CARLA论坛">
CARLA论坛</a>
</p>
</div>