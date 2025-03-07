# 创建语义标签

了解如何定义用于语义分割的自定义标签。这些标签还可添加到[carla.CityObjectLabel](python_api_zhCN.md#carla.CityObjectLabel)来过滤[carla.World](python_api_zhCN.md#carla.World)检索的边界框。

* [__创建新语义标签__](#创建新语义标签)
	* [1. 创建标签ID](#1-创建标签id)
	* [2. 创建UE资源文件夹](#2-创建ue资源文件夹)
	* [3. 建立UE与代码标签双向对应](#3-建立ue与代码标签双向对应)
	* [4. 定义颜色代码](#4-定义颜色代码)
	* [5. 添加标签元素](#5-添加标签元素)
* [__将标签添加至CityObjectLabel__](#将标签添加至carlacityobjectlabel)

---

## 创建新语义标签

### 1. 创建标签ID

__打开`LibCarla/source/carla/rpc/ObjectLabel.h`__，在枚举末尾按相同格式添加新标签。

![object_label_h](img/tuto_D_create_semantic_tags/01_objectlabel_tag.jpg)

!!! 注意
    标签无需严格排序，但建议保持有序。

### 2. 创建UE资源文件夹

__打开虚幻引擎编辑器__，进入`Carla/Static`目录，创建与标签同名的新文件夹。

![ue_folder](img/tuto_D_create_semantic_tags/02_ue_folder.jpg)

!!! 注意
    UE文件夹与标签名称不必完全相同，但建议保持一致。

### 3. 建立UE与代码标签双向对应

__3.1 打开`Unreal/CarlaUnreal/Plugins/Carla/Source/Carla/Game/Tagger.cpp`__，在__`GetLabelByFolderName`__函数末尾添加新标签。比较字符串必须与[步骤2](#2-创建ue资源文件夹)中UE文件夹名称完全一致。

![tagger_cpp](img/tuto_D_create_semantic_tags/03_tagger_cpp.jpg)

__3.2 在同一文件的__`GetTagAsString`__函数中，在switch语句末尾添加新标签。

![tagger_cpp_02](img/tuto_D_create_semantic_tags/04_tagger_cpp_02.jpg)

### 4. 定义颜色代码

__打开`LibCarla/source/carla/image/CityScapesPalette.h`__，在数组末尾添加新标签的颜色代码。

![city_scapes_palette_h](img/tuto_D_create_semantic_tags/05_city_scapes_palette_h.jpg)

!!! 警告
    数组位置必须与标签ID对应，本例中应为`23u`。

### 5. 添加标签网格体

新语义标签已准备就绪。只有存储在对应UE文件夹中的网格体会被标记。将相关网格体移动或导入新文件夹即可完成标记。

---

## 将标签添加至[carla.CityObjectLabel](python_api_zhCN.md#carla.CityObjectLabel)

此步骤与语义分割无直接关联，但这些标签可用于过滤[carla.World](python_api_zhCN.md#carla.World)中的边界框查询。需将标签添加至PythonAPI中的枚举。

__打开`carla/PythonAPI/carla/source/libcarla/World.cpp`__，在枚举末尾添加新标签。

![city_object_label](img/tuto_D_create_semantic_tags/06_city_object_label.jpg)

---

遇到问题请查阅[常见问题](build_faq_zhCN.md)或在[CARLA论坛](https://github.com/carla-simulator/carla/discussions)提问。

<p style="font-size: 20px">下一步学习</p>

<div class="build-buttons">

<p>
<a href="../ref_sensors_zhCN" target="_blank" class="btn btn-neutral" title="CARLA传感器完整参考">
传感器参考</a>
</p>

<p>
<a href="../tuto_A_add_props_zhCN" target="_blank" class="btn btn-neutral" title="学习如何导入自定义资源">
添加新道具</a>
</p>

</div>