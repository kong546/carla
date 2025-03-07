# 内容创作 - 车辆篇

CARLA 提供开箱即用的完整车辆蓝图库，同时支持用户扩展自定义车辆以实现最大灵活性。

## 建模规范

### 车辆部件
车辆应分为以下独立建模部件：
- **车身框架**：包含底盘、外壳及内饰的统一模型
- **独立车轮**：四个车轮需建模为独立对象
- **玻璃组件**：车窗、车灯玻璃等透明部件集合
- **灯光系统**：头灯、转向灯、刹车灯等发光元件

#### 命名规范
- __车身部件__：使用M_部件_车型（例：M_Bodywork_Mustang）
- __玻璃部件__：
  - Glass_Ext（外部可见玻璃层）
  - Glass_Int（内部可见玻璃层）
- __车牌__：29x12厘米平面，使用[官方FBX模板](https://carla-assets.s3.eu-west-3.amazonaws.com/fbx/LicensePlate.rar)

## 骨骼绑定流程

### 骨骼配置
1. 创建Vehicle_Base主骨骼
2. 添加四轮子骨骼：
  - Wheel_Front_Left
  - Wheel_Front_Right
  - Wheel_Back_Left
  - Wheel_Back_Right

![Maya绑定示例](img/tuto_content_authoring_vehicles/import_model_maya.png)

### 蒙皮绑定
```maya
# 绑定车轮示例
select -r Wheel_Front_Left;
skinCluster -toSelectedBones -bindMethod 0;
```

## 虚幻引擎导入规范

### 初始配置
```sh
cmake --build Build --target launch
```

### 物理资产
1. 碰撞体生成：
   - 单凸面体（Single Convex Hull）
   - 多凸面体（Multi Convex Hull）
2. 手动碰撞体导入：
   - 右键Vehicle_Base选择「从静态网格体复制碰撞」

![物理资产配置](img/tuto_content_authoring_vehicles/single_convex_hull.png)

### 动画蓝图
1. 创建VehicleAnimationInstance实例
2. 复制标准车辆动画节点

![动画节点配置](img/tuto_content_authoring_vehicles/animation_nodes.png)

## 材质系统

### 车漆参数
- **基础色**：RGB 0.05-0.1区间
- **清漆层**：粗糙度0.3，高光0.8
- **金属质感**：使用Flakes贴图增强细节

## 质量验证标准
1. 转向系统：最大转向角50度
2. 物理模拟：60fps稳定运行
3. 碰撞检测：5cm精度阈值

[返回顶层](#内容创作---车辆篇)