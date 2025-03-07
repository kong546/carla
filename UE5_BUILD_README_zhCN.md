> [!重要]
> 这是正在开发中的版本！当前CARLA版本尚未稳定。未来几个月内可能会有重大修改导致现有功能变更，建议将此分支视为实验性版本。

# 在Linux系统使用虚幻引擎5.3构建CARLA

> [!注意]
> 本构建流程已在Ubuntu 22.04测试通过，Ubuntu 20.04将不再支持CARLA UE5。

## 环境配置

本指南详细说明如何在Linux系统使用虚幻引擎5.3从源码构建CARLA。

克隆CARLA的ue5-dev分支到本地：

```sh
git clone -b ue5-dev https://github.com/carla-simulator/carla.git CarlaUE5
```

运行配置脚本并输入GitHub凭证：
> [!注意]
> * 本版本CARLA需要使用**CARLA定制版的虚幻引擎5.3**。需将GitHub账户关联至Epic Games账号以获取仓库克隆权限，如未完成账户关联请参考[此指南](https://www.unrealengine.com/en-US/ue4-on-github)

```sh
cd CarlaUE5
env GIT_LOCAL_CREDENTIALS=your_user@your_token bash -x CarlaSetup.sh
```

Setup.sh脚本将安装所有依赖项，包括Cmake、debian软件包、Python包和虚幻引擎5.3，并下载CARLA资源进行构建。整个过程可能需要较长时间。

脚本默认使用apt安装Python3。如需使用现有Python环境，请添加`--python-root=PYTHON路径`参数指定安装路径。可在目标环境中使用`whereis python3`命令查询路径后去除`/python3`后缀。

> [!注意]
> * 必须使用**CARLA定制版的虚幻引擎5.3**
> * 若已安装过旧版CARLA虚幻引擎，请确保设置`CARLA_UNREAL_ENGINE_PATH`环境变量指向引擎安装路径。未设置时将自动下载安装（需额外1小时构建时间及225GB磁盘空间）
> * CARLA无法在外置磁盘构建，Ubuntu系统对外置磁盘的读写权限不足

## 构建与运行CARLA UE5

修改代码后重新构建时使用以下命令：

* 配置：
```sh
cmake -G Ninja -S . -B Build --toolchain=$PWD/CMake/Toolchain.cmake \
-DLAUNCH_ARGS="-prefernvidia" -DCMAKE_BUILD_TYPE=Release -DENABLE_ROS2=ON \
-DBUILD_CARLA_UNREAL=ON -DCARLA_UNREAL_ENGINE_PATH=$CARLA_UNREAL_ENGINE_PATH
```

> [!注意]
> 使用自定义Python环境时需添加参数：`-DPython_ROOT_DIR=路径`和`-DPython3_ROOT_DIR=路径`

* 构建CARLA：
```sh
cmake --build Build
```

* 安装Python API：
```sh
cmake --build Build --target carla-python-api-install
```

* 启动编辑器：
```sh
cmake --build Build --target launch
```

## 构建CARLA UE5发行包
```sh
cmake --build Build --target package
```

生成路径：`$CARLA_PATH/Build/Package`

## 运行发行包
```sh
./CarlaUnreal.sh
```

启用ROS2接口：
```sh
./CarlaUnreal.sh --ros2
```

安装对应Python API：
```sh
pip3 install PythonAPI/carla/dist/carla-*.whl
```

# 在Windows系统使用虚幻引擎5.3构建CARLA

## 环境配置

本指南详细说明如何在Windows系统使用虚幻引擎5.3从源码构建CARLA。

克隆CARLA的ue5-dev分支到本地：
```sh
git clone -b ue5-dev https://github.com/carla-simulator/carla.git CarlaUE5
```

运行配置脚本：
```sh
cd CarlaUE5
CarlaSetup.bat
```

Setup.bat脚本将安装Visual Studio 2022、Cmake、Python包和虚幻引擎5，并构建CARLA。整个过程可能需要较长时间。

> [!注意]
> * 必须使用**CARLA定制版的虚幻引擎5.3**
> * 设置`CARLA_UNREAL_ENGINE_PATH`环境变量指向已有引擎安装路径可避免重复下载
> * 确保Python环境变量PATH配置正确
> * 需开启[Windows开发者模式](https://learn.microsoft.com/en-us/gaming/game-bar/guide/developer-mode)
> * 无法在外置磁盘构建

## 构建与运行CARLA UE5

* 配置（在VS 2022 x64命令行执行）：
```sh
cmake -G Ninja -S . -B Build -DCMAKE_BUILD_TYPE=Release -DBUILD_CARLA_UNREAL=ON -DCARLA_UNREAL_ENGINE_PATH=%CARLA_UNREAL_ENGINE_PATH%
```

* 构建：
```sh
cmake --build Build
```

* 安装Python API：
```sh
cmake --build Build --target carla-python-api-install
```

* 启动编辑器：
```sh
cmake --build Build --target launch
```

## 构建发行包（Windows暂未完整测试）
```sh
cmake --build Build --target package
```

生成路径：`Build/Package`