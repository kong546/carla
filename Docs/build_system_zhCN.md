# 构建系统

* [__环境搭建__](#setup)  
* [__LibCarla库__](#libcarla)  
* [__CarlaUnreal编辑器与插件__](#carlaue4-and-carla-plugin)  
* [__PythonAPI__](#pythonapi)
    - [0.9.12+版本](#versions-0912)
    - [0.9.12之前版本](#versions-prior-to-0912)

> _本文档仍在完善中，目前主要描述Linux构建系统_。

构建系统的核心挑战在于确保所有依赖项和模块能够兼容：a) 服务端的Unreal Engine b) 客户端的Python环境。

最终目标是通过独立的Python进程调用Unreal Engine的功能。

![模块关系](img/build_modules.jpg)

在Linux系统中，我们使用clang-8.0编译器和C++14标准编译CARLA及其依赖项。针对不同使用场景需要链接不同的C++运行时库——所有需要与Unreal Engine链接的代码必须使用`libc++`编译。

---
## 环境搭建

执行命令：

```sh
make setup
```

该命令将获取并编译以下依赖项：

* llvm-8 (包含libc++和libc++abi)
* rpclib-2.2.1 (分别使用libstdc++和libc++各编译一次)
* boost-1.72.0 (头文件和基于libstdc++的boost_python)
* googletest-1.8.1 (使用libc++编译)

---
## LibCarla

使用CMake编译（要求CMake 3.9+）

执行命令：

```sh
make LibCarla
```

包含两种编译配置：

|  | 服务端 | 客户端 |
| ---------- | ---------- | ---------- |
| **单元测试**        | 启用                   | 禁用       |
| **依赖项**      | rpclib, gtest, boost  | rpclib, boost |
| **标准库**       | LLVM的`libc++`        | 默认的`libstdc++` |
| **输出文件**       | 头文件及测试可执行文件 | `libcarla_client.a`静态库 |
| **使用方**       | Carla插件           | PythonAPI      |

---
## CarlaUnreal编辑器与插件

使用Unreal Engine的构建工具同时编译CarlaUnreal项目和插件，需要预先设置`UE4_ROOT`环境变量。

编译命令：

```sh
make CarlaUnrealEditor
```

启动Unreal Editor：

```sh
make launch
```

---
## PythonAPI
### 0.9.12+版本

使用Python的`setuptools`工具（"setup.py"）编译，需要提前安装：Python、libpython-dev、libboost-python-dev、pip>=20.3、wheel和auditwheel。

编译命令：

```sh
make PythonAPI
```

生成两种格式的客户端库文件：

>__A. .whl包__

>>安装命令：

>>      pip install <wheel文件>.whl

>>无需像旧版本或.egg文件那样手动添加库路径，直接使用`import carla`即可。

>__B. .egg包__

>>参考[__0.9.12之前版本__](#versions-prior-to-0912)的说明文档。

### 0.9.12之前版本

编译要求：Python、libpython-dev和libboost-python-dev。

编译命令：

```sh
make PythonAPI
```

生成两种Python版本的egg包：

* `PythonAPI/dist/carla-X.X.X-py2.7-linux-x86_64.egg`
* `PythonAPI/dist/carla-X.X.X-py3.7-linux-x86_64.egg`

在Python脚本中可通过添加路径直接导入：

```python
#!/usr/bin/env python
import sys

sys.path.append(
    'PythonAPI/dist/carla-X.X.X-py%d.%d-linux-x86_64.egg' % (sys.version_info.major,
                                                             sys.version_info.minor))

import carla
# ...
```

或使用`easy_install`安装：

```sh
easy_install2 --user --no-deps PythonAPI/dist/carla-X.X.X-py2.7-linux-x86_64.egg
easy_install3 --user --no-deps PythonAPI/dist/carla-X.X.X-py3.7-linux-x86_64.egg
```