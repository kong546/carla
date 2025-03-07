# 如何添加摩擦触发器

*摩擦触发器*是可在运行时添加的盒状触发器，允许用户定义车辆进入该区域时的轮胎摩擦系数。例如，这可用于动态创建地图特定区域的滑溜表面。

要通过PythonAPI生成摩擦触发器，用户需要先获取`static.trigger.friction`蓝图定义，并设置以下必要属性：

- *friction*: 车辆进入触发器区域时的摩擦系数
- *extent_x*: 触发器包围盒X轴范围（厘米）
- *extent_y*: 触发器包围盒Y轴范围（厘米）
- *extent_z*: 触发器包围盒Z轴范围（厘米）

设置完成后，定义包含位置和旋转信息的变换(transform)来生成触发器。

##### 示例

```py
import carla

def main():
    # 连接客户端
    client = carla.Client('127.0.0.1', 2000)
    client.set_timeout(2.0)

    # 获取世界和参与者
    world = client.get_world()
    actors = world.get_actors()

    # 查找摩擦触发器蓝图
    friction_bp = world.get_blueprint_library().find('static.trigger.friction')

    extent = carla.Location(700.0, 700.0, 700.0)

    # 设置蓝图属性
    friction_bp.set_attribute('friction', str(0.0))
    friction_bp.set_attribute('extent_x', str(extent.x))
    friction_bp.set_attribute('extent_y', str(extent.y))
    friction_bp.set_attribute('extent_z', str(extent.z))

    # 生成摩擦触发器
    transform = carla.Transform()
    transform.location = carla.Location(100.0, 0.0, 0.0)
    world.spawn_actor(friction_bp, transform)

    # 可视化触发器（可选）
    world.debug.draw_box(box=carla.BoundingBox(transform.location, extent * 1e-2), 
                        rotation=transform.rotation, 
                        life_time=100, 
                        thickness=0.5, 
                        color=carla.Color(r=255,g=0,b=0))

if __name__ == '__main__':
    main()
```