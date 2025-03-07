# 在Linux系统使用虚幻引擎5.5构建CARLA

!!! note
        虚幻引擎5版本的CARLA要求Ubuntu系统最低版本为22.04，不支持在更旧的Ubuntu版本上构建。

## 环境配置

本指南详细说明如何在Linux系统使用虚幻引擎5.5从源码构建CARLA。

克隆CARLA的`ue5-dev`分支到本地机器：

```sh
git clone -b ue5-dev https://github.com/carla-simulator/carla.git CarlaUE5
```

运行安装脚本：

```sh
cd CarlaUE5
bash -x  CarlaSetup.sh
```

Setup.sh脚本将安装所有必需的软件包，包括Cmake、debian包、Python包和虚幻引擎5.5，并设置必要的环境变量。该脚本还会下载CARLA资源、构建CARLA并启动编辑器。

完成后，脚本将启动CARLA虚幻引擎5编辑器。**注意：此过程可能需要较长时间。**

安装脚本只需在首次构建时运行。后续重新构建时，请使用以下章节中的命令。

!!! note
        * 此版本CARLA需要**CARLA定制的虚幻引擎5.5分支**。您需要将GitHub账户与Epic Games关联以获得克隆UE仓库的权限。若尚未关联账户，请参照[此指南](https://www.unrealengine.com/en-US/ue4-on-github)
        * 若使用先前构建的CARLA虚幻引擎5，**请确保已定义CARLA_UNREAL_ENGINE_PATH环境变量**并指向CARLA虚幻引擎5.5的绝对路径。若未定义该变量，Setup.sh脚本将自动下载并构建CARLA虚幻引擎5，**此过程将额外消耗1小时以上的构建时间及225GB磁盘空间**
        * Setup.sh脚本会检查PATH变量首位的Python安装情况，若无则自动安装Python。**若要使用自定义Python版本，请确保在运行脚本前正确设置PATH变量**
        * CARLA无法在外部磁盘构建，Ubuntu系统对构建所需读写执行权限的限制会导致构建失败

## 构建并运行CARLA UE5

安装脚本会自动执行以下命令，修改代码后如需重新构建请使用：

* 配置项目：

```sh
cmake -G Ninja -S . -B Build --toolchain=$PWD/CMake/LinuxToolchain.cmake \
-DLAUNCH_ARGS="-prefernvidia" -DCMAKE_BUILD_TYPE=Release -DENABLE_ROS2=ON
```

命令行参数说明：

**G** - 指定构建系统
**S** - CARLA仓库源码路径
**B** - 构建输出目录

* 构建CARLA：

```sh
cmake --build Build
```

* 构建并安装Python API：

```sh
cmake --build Build --target carla-python-api-install
```

* 启动编辑器：

```sh
cmake --build Build --target launch
```

## 生成CARLA UE5安装包

```sh
cmake --build Build --target package
```

安装包将生成在`$CARLA_PATH/Build/Package`目录

如需生成开发调试包，请使用`package-development`目标，该包会包含调试日志输出功能。

## 运行安装包

在安装包根目录执行以下命令：

```sh
./CarlaUnreal.sh
```

如需启用原生ROS2接口，添加`--ros2`参数：

```sh
./CarlaUnreal.sh --ros2
```

如需安装对应版本的Python API：

```sh
pip3 install PythonAPI/dist/carla-*.whl
```

## 预设配置

如需使用多种配置构建，推荐使用预设系统。创建预设配置：

```sh
cmake --preset Linux-Development
```

这将在构建目录中创建`Linux-Development`子目录。该配置的所有构建产物都将存放在此目录，例如启动编辑器时运行：

```sh
cmake --build Build/Linux-Development/ --target launch
```