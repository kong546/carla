# ROS 桥接

__完整文档请访问[此处](https://carla.readthedocs.io/projects/ros-bridge/en/latest/)。__

---

ROS 桥接器实现了 ROS 与 CARLA 之间的双向通信。CARLA 服务器的信息会被转换为 ROS 主题，同时 ROS 节点间传递的消息也会被转换为在 CARLA 中执行的指令。

该桥接器兼容 ROS 1 和 ROS 2 版本。

主要功能特性包括：

- 提供激光雷达（LIDAR）、语义激光雷达（Semantic LIDAR）、摄像头（深度/语义分割/RGB/DVS）、GNSS、雷达和IMU的传感器数据
- 提供物体变换数据、交通灯状态、可视化标记、碰撞检测和车道入侵检测
- 通过转向、油门和刹车控制自动驾驶代理
- 控制CARLA仿真参数如同步模式、暂停/继续仿真等核心功能