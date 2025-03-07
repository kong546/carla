!!! warning
        本文档为进行中版本！当前CARLA版本尚未达到稳定发布状态。未来数月可能进行重大修改，这些修改可能导致您所做的任何适配失效。建议将此分支视为实验性版本。

# 在Windows平台使用Unreal Engine 5.5构建CARLA

## 环境配置

本指南详细说明如何在Windows系统上从源码构建基于Unreal Engine 5.5的CARLA。

克隆CARLA的`ue5-dev`分支到本地：

```sh
git clone -b ue5-dev https://github.com/carla-simulator/carla.git CarlaUE5
```

运行安装脚本：

```sh
cd CarlaUE5
CarlaSetup.bat
```

CarlaSetup.bat脚本将自动安装所有依赖组件，包括Visual Studio 2022、CMake、Python 3.8环境包以及Unreal Engine 5.5。该脚本还会下载CARLA内容资源并执行构建。因此，整个安装过程可能耗时较长。

也可使用Python 3.9或3.10版本。

!!! note
        * 本版CARLA要求使用**CARLA定制版的Unreal Engine 5.5**。需将GitHub账户与Epic Games账号关联以获得UE仓库克隆权限。若未完成账户关联，请参考[此指南](https://www.unrealengine.com/en-US/ue4-on-github)
        * 使用旧版CARLA Unreal Engine 5构建时，需确保环境变量CARLA_UNREAL_ENGINE_PATH指向CARLA Unreal Engine 5的绝对路径。若未设置该变量，Setup.bat脚本将自动下载并构建CARLA Unreal Engine 5，**此过程需额外1小时以上构建时间及225GB磁盘空间**
        * Setup.bat脚本会检查PATH环境变量中是否存在Python版本，若未找到将自动安装Python。**若要使用自定义Python版本，请确保在运行脚本前正确配置PATH变量**
        * **需启用Windows开发者模式**，否则构建将失败。激活方法请参考[微软官方指南](https://learn.microsoft.com/zh-cn/gaming/game-bar/guide/developer-mode)
        * **CARLA无法在外置磁盘上构建**，Windows系统对外置磁盘的读写执行权限不足。


## 构建与运行CARLA UE5

Setup.bat文件自动执行以下命令，修改代码后如需重新构建，请手动执行：

!!! warning
       请确保环境变量CARLA_UNREAL_ENGINE_PATH指向CARLA Unreal Engine 5.5的绝对路径。若通过其他方式安装依赖，该变量可能未正确设置。

* **配置工程**。在CarlaUE5目录打开x64 Native Tools Command Prompt for VS 2022，执行：

```sh
cmake -G Ninja -S . -B Build -DCMAKE_BUILD_TYPE=Release
```

命令行参数说明：

**G** - 指定构建系统
**S** - CARLA仓库源码路径
**B** - 构建输出目录

CMake预设 - 将多个命令聚合为单个预设

* **构建CARLA**。在CarlaUE5目录打开x64 Native Tools Command Prompt for VS 2022，执行：

```sh
cmake --build Build
```

* **构建并安装Python API**。在CarlaUE5目录打开x64 Native Tools Command Prompt for VS 2022，执行：

```sh
cmake --build Build --target carla-python-api-install
```

* **启动编辑器**。在CarlaUE5目录打开x64 Native Tools Command Prompt for VS 2022，执行：

```sh
cmake --build Build --target launch
```

## 构建CARLA UE5发布包

!!! warning
        Windows平台的CARLA UE5发布包构建尚未完成全面测试。

在CarlaUE5目录打开x64 Native Tools Command Prompt for VS 2022，执行以下命令创建正式发布包：

```sh
cmake --build Build --target package
```

如需构建包含调试日志的开发版发布包：

```sh
cmake --build Build --target package-development
```

构建产物将生成于`Build/Package`目录

## 运行发布包

Windows平台的发布包运行测试尚未完成

## 预设配置

如需使用多种配置构建，建议使用预设系统。创建预设配置：

```sh
cmake --preset Linux-Development
```

该命令将在构建目录下创建名为`Linux-Development`的文件夹。该配置的所有构建产物都将存储于此目录，例如启动编辑器命令变为：

```sh
cmake --build Build/Linux-Development/ --target launch
```