# 生成详细碰撞体

本教程说明如何为车辆创建更精确的碰撞边界（相对于物体的原始形状）。这些碰撞体可用作物理碰撞体兼容碰撞检测，或作为次级碰撞体用于激光雷达等射线投射传感器以获取更精确数据。新碰撞体可集成到CARLA中供社区使用，更多贡献指南请参见[此处](cont_contribution_guidelines_zhCN.md)。

有两种创建碰撞体的方法，但效果并不完全相同：

* __射线投射碰撞体__ — 需要基础的3D建模技能，为车辆添加次级碰撞体以提高激光雷达等射线投射传感器的检测精度。
* __物理碰撞体__ — 基于贡献者[yankagan](https://github.com/yankagan)的[教程](https://bitbucket.org/yankagan/carla-content/wiki/Home)，无需手动建模即可创建碰撞网格，作为车辆的主要物理碰撞体和传感器检测依据。

---

* [__射线投射碰撞体__](#射线投射碰撞体)
	* [1-导出车辆FBX](#1-导出车辆fbx)
	* [2-生成低密度网格](#2-生成低密度网格)
	* [3-导入UE引擎](#3-导入ue引擎)
	* [4-添加碰撞体组件](#4-添加碰撞体组件)

---

* [__物理碰撞体__](#物理碰撞体)
	* [0-前提条件](#0-前提条件)
	* [1-在虚幻编辑器中定义车轮碰撞](#1-在虚幻编辑器中定义车轮碰撞)
	* [2-导出车辆FBX](#2-导出车辆fbx)
	* [3-4 导入Blender创建自定义边界](#3-4-导入blender创建自定义边界)
	* [5-从Blender导出FBX](#5-从blender导出fbx)
	* [6-8 导入碰撞体并定义物理属性](#6-8-导入碰撞体并定义物理属性)

---

## 射线投射碰撞体

### 1-导出车辆FBX

首先需要获取车辆的原始网格作为参考：

__1.1__ 在UE中打开CARLA，进入`Content/Carla/Static/Vehicles/4Wheeled/<车辆型号>`

__1.2__ 右键点击`SM_<车辆型号>`将车辆网格导出为FBX

### 2-生成低密度网格

__2.1__ 使用3D建模软件，参考原始网格创建忠实原型的低密度网格

![手动生成网格](img/tuto_D_colliders_mesh.jpg)

__2.2__ 将新网格保存为FBX，命名格式为`sm_sc_<车辆型号>.fbx`（例如`sm_sc_audiTT.fbx`）

!!! 注意
	车轮、车顶、挡泥板等部件的网格需要精确匹配几何形状，简单的立方体无法满足要求

### 3-导入UE引擎

__3.1__ 在UE中打开CARLA，进入`Content/Carla/Static/Vehicles/4Wheeled/<车辆型号>`

__3.2__ 右键导入新网格`SM_sc_<车辆型号>.fbx`

### 4-添加碰撞体组件

__4.1__ 进入`Content/Carla/Blueprints/Vehicles/<车辆型号>`，打开名为`BP_<车辆型号>`的车辆蓝图

__4.2__ 选择`CustomCollision`组件，在静态网格属性中添加`SM_sc_<车辆型号>.fbx`

![自定义碰撞组件](img/tuto_D_colliders_final.jpg)

__4.3__ 点击工具栏上的`Compile`并保存修改

!!! 注意
	摩托车、自行车等车辆也使用相同的`CustomCollision`组件修改碰撞网格

---

## 物理碰撞体

!!! 重要
	本教程基于[yankagan](https://github.com/yankagan)的[贡献](https://bitbucket.org/yankagan/carla-content/wiki/Home)，特别感谢Francisco E的[UE碰撞体导入教程](https://www.youtube.com/watch?v=SEH4f0HrCDM)

[效果演示视频](https://www.youtube.com/watch?v=CXK2M2cNQ4Y)

### 0-前提条件

* 从源码[编译CARLA](build_linux_zhCN.md)或[Windows编译](build_windows_zhCN.md)
* 从[官网](https://www.blender.org/download/)下载Blender 2.80+
* 安装[Blender VHACD插件](https://github.com/andyp123/blender_vhacd)，该插件可自动创建凸面体近似网格

!!! 推荐
	建议通过[Blender入门系列](https://www.youtube.com/watch?v=ppASl6yaguU)和[Udemy课程](https://www.udemy.com/course/blender-3d-from-zero-to-hero/)学习基础操作

### 1-在虚幻编辑器中定义车轮碰撞

__步骤1__ (UE操作) — 为车轮添加碰撞边界，详细步骤参考视频教程：

[![步骤1演示](img/tuto_D_colliders_01.jpg)](https://www.youtube.com/watch?v=bECnsTw6ehI)

### 2-导出车辆FBX

__步骤2__ (UE操作) — 导出车辆骨骼网格：

__2.1__ 进入`Content/Carla/Static/Vehicles/4Wheeled/<车辆型号>`

__2.2__ 右键点击`SM_<车辆型号>`导出为FBX

### 3-4 导入Blender创建自定义边界

__步骤3__ (Blender操作) — 导入FBX文件

__步骤4__ (Blender操作) — 使用VHACD工具创建凸面体碰撞边界：

__4.1__ 切除车轮底部、后视镜和车顶部分，分阶段创建碰撞边界

[![步骤3演示](img/tuto_D_colliders_03.jpg)](https://www.youtube.com/watch?v=oROkK3OCuOA)

__4.2__ 为后视镜单独创建碰撞边界

[![步骤4演示](img/tuto_D_colliders_04.jpg)](https://www.youtube.com/watch?v=L3upzdC602s)

!!! 警告
	所有碰撞体命名必须以`UCX_`开头，后续名称必须与原始网格完全一致

### 5-从Blender导出FBX

__步骤5__ (Blender操作) — 导出碰撞边界：

__5.1__ 同时选择原始车辆和新建的碰撞物体

__5.2__ 在导出设置中勾选`仅选中物体`并选择"Mesh"类型

[![导出设置](img/tuto_D_colliders_05.jpg)](https://youtu.be/aJPyskYjzWo)

### 6-8 导入碰撞体并定义物理属性

__步骤6__ (UE操作) — 将FBX导入为静态网格

__步骤7__ (UE操作) — 将碰撞体添加到车辆的物理资产

__步骤8__ (UE操作) — 创建关节约束并定义物理参数

[![物理设置](img/tuto_D_colliders_0608.jpg)](https://www.youtube.com/watch?v=aqFNwAyj2CA)

---

完成车辆碰撞体修改后，可在CARLA中进行测试。如有疑问请访问论坛：

<div class="build-buttons">
<p>
<a href="https://github.com/carla-simulator/carla/discussions/" target="_blank" class="btn btn-neutral" title="访问CARLA论坛">
CARLA论坛</a>
</p>
</div>