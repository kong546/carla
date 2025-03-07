# 为CARLA创建大型地图

大型地图（如11号、12号城镇）在CARLA中的运行机制与标准地图（如10号城镇）不同。地图被划分为多个区块（Tile），通常每个区块1-2平方公里。区块机制通过按需加载图形内存中的地图部分来实现高效渲染，邻近玩家车辆的区块会被加载，该行为可通过启动参数调整。

# 在RoadRunner中创建大型地图

RoadRunner是创建CARLA大型地图的推荐工具。本指南将说明如何使用RoadRunner创建大型地图，并将其导入Unreal引擎编辑器。

- [__在RoadRunner构建大型地图__](#在roadrunner构建大型地图)
- [__从RoadRunner导出大型地图__](#从roadrunner导出大型地图)
- [__将大型地图导入CARLA__](#将大型地图导入carla)
    - [文件与目录结构](#文件与目录结构)
    - [创建JSON描述文件（可选）](#创建json描述文件可选)
    - [执行导入操作](#执行导入操作)
- [__在Unreal编辑器中处理大型地图__](#在unreal编辑器中处理大型地图)
- [__打包大型地图__](#打包大型地图)
---

## 在RoadRunner构建大型地图

构建复杂地图的具体方法超出本指南范围，但[RoadRunner文档][rr_tutorials]提供视频教程。大型地图的构建流程与标准地图相似，主要区别在于导出方式。

![roadrunner_draw](img/tuto_content_authoring_maps/large_map_roadrunner.png)

我们创建了一个约1.2平方公里的大型地图，导出时将选择700米的区块尺寸，最终会分割为4个区块。

!!! 注意
    创建带地形起伏的大型地图时，建议最大尺寸不超过20×20平方公里，更大尺寸可能导致RoadRunner导出时崩溃。

[rr_tutorials]: https://www.mathworks.com/support/search.html?fq=asset_type_name:video%20category:roadrunner/index&page=1&s_tid=CRUX_topnav

---

## 从RoadRunner导出大型地图

以下是RoadRunner导出大型地图的基本流程：

[exportlink]: https://www.mathworks.com/help/roadrunner/ug/Exporting-to-CARLA.html

1. 通过[世界设置工具](https://www.mathworks.com/help/roadrunner/ref/worldsettingstool.html)选择完整导出区域，调整蓝色边界框覆盖目标区域后点击"应用世界更改"。

![roadrunner_workspace](img/tuto_content_authoring_maps/roadrunner_workspace.png)

2. 使用场景导出预览工具查看区块划分效果，在"区块选项"菜单中调整*区块尺寸*参数，点击"刷新场景"查看调整效果。

![roadrunner_scene_preview](img/tuto_content_authoring_maps/rr_scene_export_preview.png)

!!! 重要
    __区块尺寸__：需根据地图复杂度合理选择。高密度3D资产（建筑、植被）的地图建议使用较小区块（1公里左右），但会增加制作复杂度。CARLA引擎支持最大区块尺寸为2公里。

3. 导出`.fbx`几何文件：
   - 主工具栏选择 `文件` -> `导出` -> `FBX(.fbx)`
   - 在导出窗口中勾选：
     - _按语义分割_：按语义分割网格，改善行人导航
     - _二次方纹理尺寸_：提升性能
     - _嵌入纹理_：确保纹理嵌入网格
     - _导出为区块_：设置尺寸（最大2000×2000）
     - _导出独立区块_：生成CARLA流式加载所需的独立区块文件

>>>>>>![export_large_map_fbx](img/tuto_content_authoring_maps/rr_export.png)

4. 导出`.xodr`OpenDRIVE文件：
   - 主工具栏选择 `文件` -> `导出` -> `OpenDRIVE(.xodr)`

导出目录将包含一个.xodr文件和多个.fbx文件：

![export_large_map_fbx](img/tuto_content_authoring_maps/large_map_export.png)

!!! 警告
    确保.fbx和.xodr文件具有相同的基础文件名。

---

# 将大型地图导入CARLA

## 文件与目录结构

所有导入文件应放置在CARLA根目录的`Import`文件夹，包含：
- 由多个.fbx区块文件组成的地图网格
- 单个.xodrOpenDRIVE定义文件

!!! 警告
    不能同时导入大型地图和标准地图。

区块文件命名规范：
```
<地图名称>_Tile_<X坐标>_<Y坐标>.fbx
```

RoadRunner默认遵循此规范，但建议导入前二次确认。区块布局示例如下：

>>>>>><img src="../img/tuto_content_authoring_maps/large_map_tiles.png" width="70%">

标准目录结构示例：
```sh
Import
│
└── Package01
  ├── Package01.json
  ├── LargeMap_Tile_0_0.fbx
  ├── LargeMap_Tile_0_1.fbx
  ├── LargeMap_Tile_1_0.fbx
  ├── LargeMap_Tile_1_1.fbx
  └── LargeMap.xodr
```

!!! 注意
    `package.json`非必需文件，缺失时会自动生成。

---

## 创建JSON描述文件（可选）

JSON文件应包含以下地图参数：
```json
{
  "maps": [
    {
      "name": "LargeMap",
      "xodr": "./LargeMap.xodr",
      "use_carla_materials": true,
      "tile_size": 700,
      "tiles": [
        "./LargeMap_Tile_0_0.fbx",
        "./LargeMap_Tile_0_1.fbx",
        "./LargeMap_Tile_1_0.fbx",
        "./LargeMap_Tile_1_1.fbx"
      ]
    }
  ],
  "props": []
}
```

---

## 执行导入操作

在CARLA根目录执行：
```sh
make import
```

内存受限时可分批次导入：
```sh
make import ARGS="--batch-size=200"
```

可选参数：
- `--package=<包名称>` 指定唯一包名（默认map_package）
- `--no-carla-materials` 使用RoadRunner材质（需配合自定义JSON文件）

导入完成后，地图资源将保存在`Unreal/CarlaUnreal/Content`，包含基础区块（天空、天气等全局元素）和流式加载区块。

!!! 注意
    当前不建议对大型地图使用标准地图定制工具（道路绘制、程序化建筑等）。

---

## 在Unreal编辑器中处理大型地图

在内容浏览器中打开对应地图包（默认map_package），通过双击单个区块文件（如LargeMap_Tile_0_0）进行细节编辑。首次加载建议：
1. 依次加载所有区块文件
2. 将视图模式从"Lit"切换为"Unlit"
3. 参照标准地图流程添加资产

![large_map_unreal](img/tuto_content_authoring_maps/large_map_unreal.png)

---

## 打包大型地图

打包命令：
```sh
make package ARGS="--packages=<地图包名称>"
```

打包文件将生成在`Dist`（Linux）或`/Build/UE4Carla/`（Windows）目录，可用于独立版CARLA。

---

更多问题请访问[CARLA论坛](https://github.com/carla-simulator/carla/discussions)。

<div class="build-buttons">
<p>
<a href="https://github.com/carla-simulator/carla/discussions" target="_blank" class="btn btn-neutral" title="访问CARLA论坛">
CARLA论坛</a>
</p>
</div>