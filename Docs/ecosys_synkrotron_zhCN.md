<img src="../img/synkrotron.jpg" alt="synkrotron_logo" style="display: block; margin-left: auto; margin-right: auto; width: 70%;">

# Synkrotron 仿真解决方案

Synkrotron 基于 CARLA 提供先进的自动驾驶仿真解决方案。其产品套件 OASIS 支持场景生成、传感器建模、交通仿真和数据管理等广泛应用。基于灵活架构设计，OASIS 既可部署在云端实现规模化运行，也支持在开发者本地环境进行原型验证。

---

<img src="../img/oasis_logo.png" alt="oasis_logo" style="display: block; margin-left: auto; margin-right: auto; width: 70%;">

<div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
    <iframe src="https://www.youtube.com/embed/YRI67aar3S0" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
</div>

<br>

## [__OASIS 仿真平台__](https://www.synkrotron.ai/sim.html)

OASIS Sim 是以 CARLA 为核心的全功能可扩展仿真平台，支持自动驾驶仿真的全生命周期管理：

- 通过图形界面进行场景导入与编辑
- 传感器配置管理
- 分布式任务调度
- 基于丰富仿真数据与日志的诊断分析

通过容器化封装同时支持云端和本地部署，并开放完整 API 支持 DevOps 集成。您可通过[申请试用](https://synkrotron.ai/contact.html)获取服务。

## [__OASIS 数据平台__](https://www.synkrotron.ai/data.html)

OASIS Data 是面向自动驾驶研发数据管理的综合平台，通过以下功能实现数据驱动的系统开发：

* 数据采集与匿名化处理
* 基于结构化数据（CAN 总线信号、主动安全触发等）与非结构化数据（传感器读数）的多级筛选
* 采用激光雷达和/或纯视觉策略的环境信息映射与重建
* 基于预训练感知模型的自动标注
* 符合 OpenX 格式的场景标记与重建

平台处理后的数据可支持场景重仿真、感知模型再训练和运营车队管理等下游应用。

## __Synkrotron 开发工具与服务__

除完整解决方案外，Synkrotron 还提供以下开发者工具与服务：

| 产品/服务       | 描述 |
| -----------| ------ |
| __传感器模型__：鱼眼摄像头 | 支持可配置畸变参数的鱼眼摄像头 |
| __传感器模型__：激光雷达 | 具备可调快门模式与材质反射特性的先进激光雷达模型 |
| __SOTIF__ 场景生成工具 | 根据用户定义的 ODD 描述，通过本体论方法结合 Carla 评估迭代优化，生成关键场景以发现自动驾驶系统未知的不安全域 |
| __高精地图创建__ | 基于激光雷达扫描数据（可提供路测数据采集设备与服务），生成 OpenDrive 格式高精地图支持规控测试与日志仿真 |
| __传感器模型__：相机物理模型 | 支持 CMOS 传感器仿真，输出 12bit RAW 数据，适用于 ISP 算法开发或需要原始数据的 ECU 系统 |
| __静态场景构建__ | 整合高精地图、3D 资产开发与程序化建模技术，创建数字孪生静态场景 |
| __动态场景重建__ | 在静态场景基础上，通过路测数据检测跟踪交通参与者，将运动轨迹转换为 OpenScenario 1.0 文件供 CARLA 重仿真 |