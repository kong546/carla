# 内容创作 - 地图制作

## RoadRunner道路设计

RoadRunner是需要MATLAB的专有软件。部分机构（如大学）可能与MathWorks有合作协议，用户可获取RoadRunner许可。若无预算购买许可，推荐使用开源替代方案[__TrueVision Designer__](https://www.truevision.ai/designer)，该工具具备与RoadRunner相似的功能。

### RoadRunner道路网络创建

1. 新建场景并选择道路规划工具
2. 右键点击工作区放置第一个控制点
3. 拖拽延伸道路网络
4. 使用简单环形交叉路口作为示例（建议参考[RoadRunner官方文档](https://es.mathworks.com/products/roadrunner.html)构建复杂网络）

![道路绘制演示](img/tuto_content_authoring_maps/drawing_roads.gif)

### 导出前检查清单
- 确保地图中心位于(0,0)坐标
- 验证道路定义正确性
- 检查连接处和几何体的拓扑验证

```python
# 示例：OpenDRIVE预览工具调用
world = client.get_world()
opendrive = world.get_map().to_opendrive()
```

### 材质系统规范
- 环境光遮蔽/粗糙度/金属（ORM）纹理通道需正确映射
- 自发光强度参数使用0-1标准化值
- 路面反光特性通过roughness参数控制

## 资产导入流程
1. 将FBX文件拖拽至内容浏览器
2. 右键创建材质实例
3. 配置材质参数：
   - BaseColor（基础色）
   - Metallic（金属度） 
   - Roughness（粗糙度）
   - Emissive（自发光）

![材质实例配置](img/material_customization/master_material.png)

## 进阶功能
- [交通信号系统配置](#交通信号系统)
- [数字孪生植被布置](#数字孪生植被)
- [道路标线绘制工具](#道路标线工具)

[返回文档目录](../README_zhCN.md)