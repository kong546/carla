# 创建独立资源包

在CARLA中，使用独立资源包管理资产是常见做法。这种方式可以有效控制构建包体积，并支持随时将资产包导入CARLA项目。该方法也为有序分发资产提供了便利。

- [__通过源码构建导出资源包__](#导出源码构建的资源包)  
- [__使用Docker导出资源包__](#使用docker导出资源包)
- [__将资产导入CARLA项目__](#将资产导入carla项目)  

---
## 导出源码构建的资源包

当资产成功导入Unreal引擎后，用户可生成__独立资源包__。这些资源包可用于向CARLA项目（如0.9.8版本）分发内容。

运行以下命令导出资源包：

```sh
make package ARGS="--packages=Package1,Package2"
```

此命令将为每个列出的资源包生成压缩的`.tar.gz`文件。在Linux系统中文件保存在`Dist`目录，Windows系统则保存在`/Build/UE4Carla/`目录。

---

## 使用Docker导出资源包

通过Docker镜像可以构建Unreal引擎和CARLA，并用于创建资源包或准备项目所需资产。

构建Docker镜像请参考[此教程](build_docker_unreal.md)。

镜像准备就绪后：

1. 进入`Util/Docker`目录
2. 执行以下命令之一来创建CARLA资源包或准备项目资产：

```sh
# 创建独立资源包
./docker_tools.py --output /输出路径

# 准备项目所需资产
./docker_tools.py --input /待导入资产路径 --output /输出路径 --packages 资源包1,资源包2
```

---
## 将资产导入CARLA项目

独立资源包以`.tar.gz`格式存储，不同平台解压方式不同：

*   __Windows系统__：将压缩文件解压至CARLA根目录  
*   __Linux系统__：将压缩文件移至`Import`目录后运行脚本  

```sh
cd Import
./ImportAssets.sh
```

!!! 注意
    独立资源包不能直接导入CARLA源码构建。请参考相关教程导入[道具](tuto_A_add_props.md)、[地图](tuto_M_custom_map_overview.md)或[车辆](tuto_A_add_vehicle.md)。

---

以上是CARLA中创建和使用独立资源包的完整说明。如遇意外问题，欢迎在论坛交流讨论。

<div class="build-buttons">
<p>
<a href="https://github.com/carla-simulator/carla/discussions/" target="_blank" class="btn btn-neutral" title="前往CARLA论坛">
CARLA论坛</a>
</p>
</div>