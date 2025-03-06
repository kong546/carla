# OpenDRIVE 独立模式

本功能允许用户直接使用OpenDRIVE文件作为CARLA地图。模拟器会自动生成道路网格供参与者导航。

*   [__概述__](#概述)  
*   [__运行独立地图__](#运行独立地图)  
*   [__网格生成__](#网格生成)  

---
## 概述

该模式仅使用OpenDRIVE文件即可运行完整模拟，无需额外几何体或资产。模拟器通过解析OpenDRIVE文件并程序化生成临时3D网格来实现。

生成的网格以极简方式描述道路定义，所有元素均与OpenDRIVE文件对应。为防止车辆坠落，采取两项措施：

*   在车辆流复杂的路口处适当加宽车道  
*   在道路边界创建可见护栏作为最后防护  

交通信号灯、停车让行标志将实时生成。行人可在人行道和斑马线区域导航。所有元素均基于OpenDRIVE文件定义，因此文件中的问题会直接影响模拟效果，在复杂路口尤为明显。

!!! Important
    特别重要的是要仔细检查OpenDRIVE文件。文件中的任何问题都会在模拟运行时显现。

![opendrive_standalone](img/opendrive_standalone.jpg)

---
## 运行独立地图

通过API调用[`client.generate_opendrive_world()`](python_api.md#carla.Client.generate_opendrive_world)即可加载OpenDRIVE文件。该方法需要两个参数：

*   __`opendrive`__：解析为字符串的OpenDRIVE文件内容  
*   __`parameters`__：包含网格生成设置的[carla.OpendriveGenerationParameters](python_api.md#carla.OpendriveGenerationParameters)对象（可选）  

	*   __`vertex_distance`__ *(默认2.0米)* — 网格顶点间距。数值越大精度越低，但过小会导致网格过重  
	*   __`max_road_length`__ *(默认50.0米)* — 网格分段最大长度。较小分段提升渲染效率，但过多会降低性能  
	*   __`wall_height`__ *(默认1.0米)* — 边界护栏高度  
	*   __`additional_width`__ *(默认0.6米，两侧各0.3米)* — 路口车道额外宽度  
	*   __`smooth_junctions`__ *(默认True)* — 启用时优化路口网格平滑度  
	*   __`enable_mesh_visibility`__ *(默认True)* — 控制网格可见性以节省渲染资源

`PythonAPI/util/`目录下的`config.py`脚本新增`-x`参数用于快速测试：

```sh
python3 config.py -x opendrive/TownBig.xodr
```

!!! Important
    __generate_opendrive_world()__需要OpenDRIVE文件内容字符串，而__config.py__需要文件路径

!!! Note
    若出现`opendrive could not be correctly parsed`错误，请确保对`CarlaUnreal/Content/Carla/Maps/OpenDrive/`目录有写入权限

---
## 网格生成

网格生成是本模式的核心功能。最新优化重点改善了倾斜路口的网格接合问题：

![opendrive_meshissue](img/opendrive_meshissue.jpg)
<div style="text-align: right"><i>启用<smooth_junctions>参数可避免高层车道遮挡底层的问题</i></div>

网格采用分块生成策略，通过动态加载提升渲染效率。当前版本需注意：

*   __路口平滑__：默认启用，可通过`smooth_junctions=False`禁用  
*   __横向坡度__：暂不支持  
*   __人行道高度__：为保证碰撞检测，采用固定值（RoadRunner暂不导出该参数）
---

关于OpenDRIVE独立模式的所有技术细节已介绍完毕，欢迎使用任意OpenDRIVE地图进行测试。

问题建议请访问论坛：

<div class="build-buttons">
<p>
<a href="https://github.com/carla-simulator/carla/discussions/" target="_blank" class="btn btn-neutral" title="前往CARLA论坛">
CARLA论坛</a>
</p>
</div>