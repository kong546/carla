# 从源代码构建 CARLA

!!! note
    本构建文档适用于构建基于虚幻引擎5（Unreal Engine 5）的CARLA版本。

用户可以通过源代码构建CARLA进行开发。如果您想为CARLA添加额外功能或使用虚幻编辑器创建资产/操作地图，推荐使用此方法。

构建指南适用于Linux和Windows平台，也支持在Docker容器中构建以部署到AWS、Azure或Google云服务。请访问[__CARLA GitHub仓库__](https://github.com/carla-simulator/carla)并克隆项目。

虚幻引擎5版的CARLA最低需要：
- Windows 10
- Ubuntu 22.04

* [__Linux构建指南__](build_linux_ue5.md)  
* [__Windows构建指南__](build_windows_ue5.md)
 
## 准备工作
1. 确保系统满足最低配置要求（16GB内存，20GB可用磁盘空间）
2. 安装对应平台的开发工具链：
   - Windows: Visual Studio 2019+
   - Linux: gcc 10+
3. 保持稳定网络连接（需下载约30GB依赖项）

## 构建完成
通过以下命令启动虚幻编辑器进行场景编辑：
```sh
make launch
```

> 警告：首次构建可能需要2-4小时，具体取决于网络速度和硬件配置。