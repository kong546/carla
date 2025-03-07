# 添加新道具

道具是场景中除地图和车辆外的其他资产，包括路灯、建筑物、树木等。模拟器可以随时通过简单流程导入新道具，这对创建定制化地图环境非常有用。

* [__准备资源包__](#准备资源包)  
	*   [创建文件夹结构](#创建文件夹结构)  
	*   [创建JSON描述文件](#创建json描述文件)  
*   [__导入CARLA预编译包__](#导入carla预编译包)  
*   [__导入源码编译版本__](#导入源码编译版本)  

---
## 准备资源包

### 创建文件夹结构

__1. 在`carla/Import`目录下创建新文件夹__，文件夹名称可自定义。

__2. 创建子文件夹__：需包含一个统一存放道具的"Props"文件夹，其下为每个道具单独创建子文件夹。

__3. 将道具文件移至对应子文件夹__：每个道具文件夹应包含`.fbx`模型文件及所需的纹理文件。

示例目录结构：

```sh
Import
│
├── Package01
│   ├── Package01.json
│   └── Props
│       ├── Prop01
│       │   ├── Prop01_Diff.png
│       │   ├── Prop01_Norm.png
│       │   ├── Prop01_Spec.png
│       │   └── Prop01.fbx
│       └── Prop02
│           └── Prop02.fbx
└── Package02
    ├── Packag02.json
    └── Props
        └── Prop03
            └── Prop03.fbx
```

### 创建JSON描述文件

在资源包根目录创建与包同名的`.json`文件，内容需包含以下参数：

* __名称(name)__：必须与.fbx文件名一致
* __路径(source)__：.fbx文件的相对路径
* __尺寸(size)__：可选值 tiny/small/medium/big/huge
* __语义标签(tag)__：有效的语义分割标签（详见下方列表）

可用语义标签：
```
Bridge（桥梁）
Building（建筑）
Dynamic（动态物体）
Fence（围栏）
Ground（地面）
GuardRail（护栏）
Other（其他）
Pedestrian（行人）
Pole（立柱）
RailTrack（轨道）
Road（道路）
RoadLine（道路标线）
SideWalk（人行道）
Sky（天空）
Static（静态物体）
Terrain（地形）
TrafficLight（交通灯）
TrafficSign（交通标志）
Unlabeled（未标记）
Vegetation（植被）
Vehicles（车辆）
Wall（墙壁）
Water（水体）
```

完整JSON示例：
```json
{
  "maps": [],
  "props": [
    {
      "name": "MyProp01",
      "size": "medium",
      "source": "./Props/Prop01/Prop01.fbx",
      "tag": "Building"
    },
    {
      "name": "MyProp02",
      "size": "small",
      "source": "./Props/Prop02/Prop02.fbx",
      "tag": "Vegetation"
    }
  ]
}
```

!!! 警告
    资源包名称必须唯一，重复名称会导致导入失败。

---
## 导入CARLA预编译包

适用于官方发布的CARLA版本（如0.9.8）：

1. __构建Unreal引擎Docker镜像__：参照[Docker构建指南](https://github.com/carla-simulator/carla/tree/master/Util/Docker)

2. __执行资源烹饪脚本__：
```sh
python3 docker_tools.py --input ~/资源路径 --output ~/输出路径 --packages Package01
```

3. __获取资源包__：生成的`Package01.tar.gz`位于输出目录

4. __导入CARLA__：
   * Windows：解压至`WindowsNoEditor`目录
   * Linux：将包移至`Import`目录后执行：
```sh
cd Util
./ImportAssets.sh
```

---
## 导入源码编译版本

适用于自行编译的CARLA版本：

1. 将资源包放入`Import`目录
2. 执行导入命令：
```sh
make import
```

!!! 重要
    导入前请确保：
    - 资源包结构符合规范
    - JSON文件语法正确
    - 没有同名的已存在资源包

---

更多问题请访问[CARLA论坛](https://github.com/carla-simulator/carla/discussions/)。

<div class="build-buttons">
<p>
<a href="tuto_M_custom_map_overview_zhCN.md" target="_blank" class="btn btn-neutral">
查看地图创建教程</a>
</p>
</div>