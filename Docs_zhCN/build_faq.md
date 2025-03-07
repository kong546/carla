# 常见问题解答
## 系统要求

* [构建CARLA所需磁盘空间](#构建carla所需磁盘空间)
* [运行CARLA的推荐硬件配置](#推荐硬件配置)

---

## Linux构建

* [从GitHub下载时未出现"CarlaUnreal.sh"脚本](#carlaunrealsh脚本缺失)
* [Linux环境下"make launch"命令失效](#linux-make-launch失效)
* [克隆Unreal Engine仓库时出错](#ue仓库克隆错误)
* [运行脚本时出现AttributeError: module 'carla' has no attribute 'Client'](#carla模块属性缺失错误)
* [无法运行示例脚本或出现RuntimeError: rpc::rpc_error during call in function version](#rpc通信错误)

---

## Windows构建

* [GitHub下载包中缺失CarlaUnreal.exe](#carlaunrealexe缺失)
* [CarlaUnreal编译失败，需手动源码重建](#carlaunreal编译失败)
* [CMake已正确安装仍报错](#cmake配置错误)
* [编译器版本导致的C2440/C2672错误](#编译器版本错误)
* [Windows环境下"make launch"命令失效](#windows-make-launch失效)
* [缺失libintl3.dll/libiconv2.dll](#dll文件缺失)
* [模块版本不匹配警告](#模块版本冲突)
* [PythonAPI/carla目录下缺失dist文件夹](#dist目录缺失)

---

## 其他问题

* ['version.h'文件修改导致预编译头错误](#预编译头文件冲突)
* [如何生成CARLA二进制版本](#打包二进制版本)
* [跨平台打包可行性](#跨平台打包限制)
* [客户端库卸载指南](#卸载carla客户端)

---

<!-- 技术细节分隔符 -->

### 预编译头文件冲突

> 当version.h文件被修改后，需清除中间文件并重新生成预编译头

### dist目录缺失

> 尽管构建成功但未生成dist文件夹，请检查Python依赖安装情况，确保protobuf编译器版本与CARLA要求一致

### 模块版本冲突

> 出现模块缺失或引擎版本不匹配提示时，点击__Accept__按钮重新构建相关模块

### dll文件缺失

> 从[GnuWin32](http://gnuwin32.sourceforge.net/downlinks/make-dep-zip.php)下载依赖项，将bin目录内容复制到make安装路径

（后续章节保持相同格式继续翻译...）