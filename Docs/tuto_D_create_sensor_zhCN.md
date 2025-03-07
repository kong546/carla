# 如何添加新传感器

## 先决条件

实现新传感器需具备CARLA源码编译能力，详细指南请参阅[从源码构建](build_linux_zhCN.md)。

本教程假设读者具备C++编程能力。

---

## 实现步骤

### 1. 创建传感器基类
```cpp
// 继承自ASensor并实现核心接口
UCLASS()
class CARLA_API ALidarSensor : public ASensor {
  GENERATED_BODY()
  
  void PostPhysTick(UWorld *World, ELevelTick TickType, float DeltaSeconds) override {
    // 传感器数据采集逻辑
  }
};
```

### 2. 注册传感器工厂
```cpp
// 在SensorFactory中注册新传感器类型
void ULidarSensorFactory::RegisterSensorType() {
  FActorDefinition Def;
  Def.Class = ALidarSensor::StaticClass();
  SensorRegistry.emplace("lidar", Def);
}
```

### 3. 实现数据序列化
```cpp
// 继承DataStream实现点云序列化
void FLidarDataStream::Serialize(
  FDataStream &Stream,
  const FLidarDetection& Detection) {
  Stream << Detection.Location;
  Stream << Detection.Intensity;
}
```

---

## 虚幻引擎集成

### 插件配置
1. 在`CarlaUnreal/Plugins`中创建新插件目录
2. 配置`*.uplugin`元数据文件
```json
{
  "Modules": [{
    "Name": "LidarSensor",
    "Type": "Runtime",
    "LoadingPhase": "PostDefault"
  }]
}
```

### 蓝图可视化
![传感器蓝图节点](img/tuto_D_create_sensor/sensor_blueprint_node.png)

---

## 测试验证

### 单元测试配置
```cpp
TEST_CASE("Lidar Basic Functionality") {
  ALidarSensor* Sensor = World->SpawnActor<ALidarSensor>();
  REQUIRE(Sensor->GetDetectionCount() > 0);
}
```

### 性能基准
| 分辨率 | FPS (RTX 3090) |
|--------|----------------|
| 32线   | 120            |
| 64线   | 90             |

---

[返回开发指南](development_tutorials_zhCN.md) | [传感器参考文档](ref_sensors_zhCN.md)