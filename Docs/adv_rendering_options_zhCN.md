# 渲染选项

本文档详细说明CARLA的渲染选项，包含画质等级、无渲染模式与离屏渲染模式，并解释0.9.12版本与此前版本的差异。

- [__图形质量__](#图形质量)  
	- [Vulkan图形接口](#vulkan图形接口)  
	- [质量等级](#质量等级)
- [__离屏模式__](#离屏模式)  
	- [离屏模式与无渲染模式对比](#离屏模式与无渲染模式对比)

!!! Important
    部分命令行选项在CARLA打包版本中不可用，请查阅[命令行选项](start_quickstart.md#command-line-options)章节了解详情。

---
## 图形质量

### 质量等级

CARLA提供两种图形质量等级：__Ep
IC__ 是默认的最高画质模式，__Low__ 模式将禁用所有后处理效果和阴影，绘制距离缩短至50米。

__Low__ 模式能显著提升仿真运行速度，适用于硬件性能受限、精度要求不高或需要近距离元素训练的场景。

以下对比图展示了两种模式的效果（Windows/Linux使用相同参数）。构建版本用户可通过UE编辑器调整画质：前往`设置/引擎可扩展性设置`进行自定义配置。

#### Epic模式
`./CarlaUnreal.sh -quality-level=Epic`

![Epic模式截图](img/epic_rendering.png)
*Epic画质模式效果*

#### Low模式
`./CarlaUnreal.sh -quality-level=Low`

![Low模式截图](img/low_quality_rendering.png)
*Low画质模式效果*

<br>

---

## 离屏渲染模式

离屏渲染模式允许CARLA在无观察者窗口的情况下运行，适用于云虚拟机等无界面环境。传感器数据仍会正常渲染输出，但不会在本地显示器呈现。

### 启用离屏模式

通过以下命令启动离屏模式：

```sh
./CarlaUnreal.sh -RenderOffScreen
```

---