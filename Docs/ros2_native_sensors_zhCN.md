# ROS 桥接传感器

---

CARLA 服务器负责基于仿真时间发布时钟(/clock)。时钟将在每帧仿真时更新。

## 可用传感器

###### RGB 摄像头

| 主题 | 类型 |
|-------|------|
| `/carla/[<父角色名称>]/<传感器角色名称>/image` | [传感器消息/图像](https://docs.ros.org/zh_CN/api/sensor_msgs/html/msg/Image.html) |
| `/carla/[<父角色名称>]/<传感器角色名称>/camera_info` | [传感器消息/相机信息](https://docs.ros.org/zh_CN/api/sensor_msgs/html/msg/CameraInfo.html) |

###### 深度摄像头

| 主题 | 类型 |
|-------|------|
| `/carla/[<父角色名称>]/<传感器角色名称>/image` | [传感器消息/图像](https://docs.ros.org/zh_CN/api/sensor_msgs/html/msg/Image.html) |
| `/carla/[<父角色名称>]/<传感器角色名称>/camera_info` | [传感器消息/相机信息](https://docs.ros.org/zh_CN/api/sensor_msgs/html/msg/CameraInfo.html) |

###### 语义分割摄像头

| 主题 | 类型 |
|-------|------|
| `/carla/[<父角色名称>]/<传感器角色名称>/image` | [传感器消息/图像](https://docs.ros.org/zh_CN/api/sensor_msgs/html/msg/Image.html) |
| `/carla/[<父角色名称>]/<传感器角色名称>/camera_info` | [传感器消息/相机信息](http://docs.ros.org/zh_CN/api/sensor_msgs/html/msg/CameraInfo.html) |

###### 实例分割摄像头

| 主题 | 类型 |
|-------|------|
| `/carla/[<父角色名称>]/<传感器角色名称>/image` | [传感器消息/图像](https://docs.ros.org/zh_CN/api/sensor_msgs/html/msg/Image.html) |
| `/carla/[<父角色名称>]/<传感器角色名称>/camera_info` | [传感器消息/相机信息](http://docs.ros.org/zh_CN/api/sensor_msgs/html/msg/CameraInfo.html) |

###### 法线摄像头

| 主题 | 类型 |
|-------|------|
| `/carla/[<父角色名称>]/<传感器角色名称>/image` | [传感器消息/图像](https://docs.ros.org/zh_CN/api/sensor_msgs/html/msg/Image.html) |
| `/carla/[<父角色名称>]/<传感器角色名称>/camera_info` | [传感器消息/相机信息](http://docs.ros.org/zh_CN/api/sensor_msgs/html/msg/CameraInfo.html) |

###### 光流摄像头

| 主题 | 类型 |
|-------|------|
| `/carla/[<父角色名称>]/<传感器角色名称>/image` | [传感器消息/图像](https://docs.ros.org/zh_CN/api/sensor_msgs/html/msg/Image.html) |
| `/carla/[<父角色名称>]/<传感器角色名称>/camera_info` | [传感器消息/相机信息](http://docs.ros.org/zh_CN/api/sensor_msgs/html/msg/CameraInfo.html) |

###### DVS 事件摄像头

| 主题 | 类型 |
|-------|------|
| `/carla/[<父角色名称>]/<传感器角色名称>/events` | [传感器消息/点云2](https://docs.ros.org/zh_CN/api/sensor_msgs/html/msg/PointCloud2.html) |
| `/carla/[<父角色名称>]/<传感器角色名称>/image` | [传感器消息/图像](https://docs.ros.org/zh_CN/api/sensor_msgs/html/msg/Image.html) |
| `/carla/[<父角色名称>]/<传感器角色名称>/camera_info` | [传感器消息/相机信息](https://docs.ros.org/zh_CN/api/sensor_msgs/html/msg/CameraInfo.html) |

###### 激光雷达

| 主题 | 类型 |
|-------|------|
| `/carla/[<父角色名称>]/<传感器角色名称>` | [传感器消息/点云2](https://docs.ros.org/zh_CN/api/sensor_msgs/html/msg/PointCloud2.html) |

###### 语义激光雷达

| 主题 | 类型 |
|-------|------|
| `/carla/[<父角色名称>]/<传感器角色名称>` | [传感器消息/点云2](https://docs.ros.org/zh_CN/api/sensor_msgs/html/msg/PointCloud2.html) |

###### 毫米波雷达

| 主题 | 类型 |
|-------|------|
| `/carla/[<父角色名称>]/<传感器角色名称>` | [传感器消息/点云2](https://docs.ros.org/zh_CN/api/sensor_msgs/html/msg/PointCloud2.html) |

###### 惯性测量单元(IMU)

| 主题 | 类型 |
|-------|------|
| `/carla/[<父角色名称>]/<传感器角色名称>` | [传感器消息/IMU](https://docs.ros.org/zh_CN/api/sensor_msgs/html/msg/Imu.html) |

###### 全球导航卫星系统(GNSS)

| 主题 | 类型 |
|-------|------|
| `/carla/[<父角色名称>]/<传感器角色名称>` | [传感器消息/导航卫星定位](https://docs.ros.org/zh_CN/api/sensor_msgs/html/msg/NavSatFix.html) |

###### 车道入侵检测传感器

| 主题 | 类型 |
|-------|------|
| `/carla/[<父角色名称>]/<传感器角色名称>` | [carla消息/车道入侵事件](https://github.com/carla-simulator/ros-carla-msgs/blob/master/msg/CarlaLaneInvasionEvent.msg) |