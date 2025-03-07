# 通过API实时修改纹理

CARLA API支持在运行时修改资产纹理。本教程将演示如何选择资产并通过Python API动态更新其纹理。

## 在虚幻引擎中选择资产

首先需要启动包含Carla地图的虚幻引擎编辑器：
1. 根据Linux/Windows的源码编译指南构建CARLA
2. 载入Town 10地图（默认城镇）
3. 在场景中选择目标建筑（示例使用BP_Apartment04_v5_Opt）

![选中建筑](../img/tuto_G_texture_streaming/building_selected.png)

__重要提示__：在世界大纲视图中悬停资产名称，以工具提示显示的内部名称为准（本例实际为BP_Apartment04_v5_Opt_2）。

![工具提示](../img/tuto_G_texture_streaming/tooltip.png)

## 导出原始纹理

1. 在细节面板打开静态网格体属性
2. 点击放大镜图标定位纹理资产（示例使用T_Apartment04_D_Opt）
3. 右键选择"导出"并保存为TGA格式

![纹理导出](../img/tuto_G_texture_streaming/texture_export.png)

## 修改并应用纹理

使用图像编辑软件修改纹理后，通过以下Python代码更新运行时纹理：

```py
import carla
from PIL import Image

# 连接CARLA服务端
client = carla.Client('127.0.0.1', 2000)
client.set_timeout(2.0)
world = client.get_world()

# 载入修改后的纹理
image = Image.open('BP_Apartment04_v05_modified.tga')
texture = carla.TextureColor(image.width, image.height)
for x in range(image.width):
    for y in range(image.height):
        r, g, b = image.getpixel((x, y))[:3]
        texture.set(x, y, carla.Color(r, g, b, 255))

# 应用纹理到建筑资产
world.apply_color_texture_to_object(
    'BP_Apartment04_v05_Opt_2', 
    carla.MaterialParameter.Diffuse, 
    texture
)
```

![纹理变化](../img/tuto_G_texture_streaming/texture_change.gif)

## 通过API查询对象名称

使用以下代码动态查询包含特定关键词的对象：
```py
# 过滤名称包含'Apartment'的对象
apartment_objects = list(filter(
    lambda k: 'Apartment' in k, 
    world.get_names_of_all_objects()
))
```