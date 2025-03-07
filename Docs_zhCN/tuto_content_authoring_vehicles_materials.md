## 内容创作 - 车辆材质

将车辆基础资源（包含网格和蓝图）导入后，现在需要为其添加材质以实现虚幻引擎中的照片级渲染效果，从而获得机器学习训练数据的最佳保真度。

虚幻编辑器提供完整的材质工作流，可创建高度真实的材质效果。但这也带来相当的复杂度。为此，CARLA准备了大量材质原型库供直接使用，无需从零开始。

### 为车辆应用材质

CARLA提供用于复制车辆光泽漆面效果的材质原型，可模拟多种车漆类型和特性。在虚幻编辑器中，通过内容浏览器定位到 `Content > Carla > Static > GenericMaterials > 00_MastersOpt`，找到基础材质*M_CarPaint_Master*。右键该材质选择【创建材质实例】，命名后移至您的新车辆资源目录。

将观察者视角移至地面附近，从内容浏览器将车辆的骨骼网格体拖入场景，车身即会显现。

![add_model](img/tuto_content_authoring_vehicles/add_model.gif)

在右侧细节面板的【材质】区块，将新材质实例拖放至【元素0】位置。此时车体将呈现灰色光泽材质效果。

![apply_material](img/tuto_content_authoring_vehicles/apply_material.gif)

双击内容浏览器中的材质实例即可开始参数调整。以下是影响车漆真实度的关键参数：

__颜色__ - 控制车体主色调：
![change_base_color](img/tuto_content_authoring_vehicles/change_base_color.gif)

__清漆层__ - 控制漆面光泽表现：
- 粗糙度通过纹理添加表面瑕疵，值越高散射越强（哑光效果）
- 推荐使用较低值保持光滑反射效果
- *清漆强度*接近1时呈现高光质感
![change_roughness](img/tuto_content_authoring_vehicles/change_roughness.gif)

__橙皮效应__ - 模拟量产车漆面的细微波纹瑕疵：
![change_orange_peel](img/tuto_content_authoring_vehicles/change_orange_peel.gif)

__金属薄片__ - 添加金属/陶瓷薄片实现珠光效果：
![flakes](img/tuto_content_authoring_vehicles/flakes.gif)

__灰尘层__ - 模拟车体表面附着的灰尘杂质：
![dust](img/tuto_content_authoring_vehicles/dust.gif)