# CARLA 快速安装指南

本指南介绍如何下载并安装 CARLA 预编译版本。该版本包含 CARLA 服务器和客户端库，支持通过[Windows 编译指南](build_windows_zhCN.md)或[Linux 编译指南](build_linux_zhCN.md)进行深度定制开发。

* __[准备工作](#准备工作)__  
* __[CARLA 安装](#carla-安装)__  
* __[安装客户端库](#安装客户端库)__
* __[运行 CARLA](#运行-carla)__  
* __[更新 CARLA](#更新-carla)__    
* __[后续步骤](#后续步骤)__ 

---
## 准备工作

安装 CARLA 前需满足以下要求：

* __系统要求__：支持 Windows 和 Linux 系统
* __操作系统__：CARLA 虚幻引擎5版本要求最低 **Ubuntu 22.04** 或 **Windows 11**
* __显卡配置__：推荐至少 NVIDIA RTX 3000 系列显卡，显存 **16GB** 以上。建议使用独立显卡处理机器学习任务
* __显卡驱动__：
  - Ubuntu 系统需 **NVIDIA 550 以上版本驱动**
  - Windows 系统需 **NVIDIA 560 以上版本驱动**
* __磁盘空间__：需要 **130GB** 可用空间
* __Python 环境__：CARLA 主要使用 Python 脚本，支持 Python 3.x 版本
* __Pip 版本__：客户端库需要 pip3 20.3 或更高版本，检查命令：

```sh
pip3 -V
```

升级 pip：

```sh
pip3 install --upgrade pip
```

* __网络要求__：默认使用 2000 和 2001 端口，确保防火墙未阻止
* __依赖安装__：根据系统执行以下命令

### Windows

```sh
pip3 install --user pygame numpy
```

### Linux

```sh
pip3 install --user pygame numpy
```

---
## CARLA 安装

从 GitHub 下载所需版本：

- [GitHub 下载页面](#b-package-installation)

版本说明：
- __稳定版__：包含最新修复和功能
- __历史版本__：过往发布版本
- __每日构建版__：开发中版本（最不稳定）

下载名为 __CARLA_版本号__ 的压缩包，解压后包含：
- 预编译的模拟器
- Python API 模块
- 示例脚本

!!! 注意
    本文档适用于虚幻引擎5版本，如需使用虚幻引擎4版本请参考[旧版文档](https://carla.readthedocs.io/en/latest/)。

## 安装客户端库

使用包内提供的 wheel 文件安装：

```sh
cd CARLA根目录/PythonAPI/dist/
pip3 install carla-*.*.*-cp3**-linux_x86_64.whl
```

注意事项：
- 通配符 * 需替换为实际版本号
- 建议在虚拟环境中安装以避免依赖冲突

---
## 运行 CARLA

Linux 系统：

```sh
cd CARLA根目录
./CarlaUnreal.sh
```

Windows 系统：

```sh
cd CARLA根目录
CarlaUnreal.exe
```

启动后会显示 Town 10 的俯瞰视图：

![town_10_default](../img/catalogue/maps/town10/town10default.png)

通过以下命令测试交通生成和车辆控制：

```sh
# 终端A：生成交通
cd PythonAPI/examples
pip3 install -r requirements.txt
python3 generate_traffic.py

# 终端B：手动控制
cd PythonAPI/examples
python3 manual_control.py
```

## 后续步骤

完成安装后建议：
1. 学习[基础概念](foundations_zhCN.md)
2. 熟悉[Python API 参考](python_api_zhCN.md)
3. 参与[CARLA 论坛讨论](https://github.com/carla-simulator/carla/discussions/)
4. 加入[Discord 社区](https://discord.gg/8kqACuC)

接下来可进行：
- [初次使用教程](tuto_first_steps_zhCN.md)
- [核心概念解析](core_concepts_zhCN.md)
- [传感器使用指南](core_sensors_zhCN.md)