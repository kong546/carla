# 更新CARLA

*   [__更新命令摘要__](#更新命令摘要)  
*   [__获取最新二进制版本__](#获取最新二进制版本)  
*   [__更新Linux和Windows构建__](#更新linux和windows构建)  
	*   [清理构建](#清理构建)  
	*   [从源仓库拉取](#从源仓库拉取)  
	*   [下载资源文件](#下载资源文件)  
	*   [启动服务器](#启动服务器)  
*   [__获取开发资产__](#获取开发资产)  

如遇意外问题、疑问或建议，请登录CARLA论坛交流。

<div class="build-buttons">
<p>
<a href="https://github.com/carla-simulator/carla/discussions/" target="_blank" class="btn btn-neutral" title="前往最新CARLA版本">
CARLA论坛</a>
</p>
</div>

---
## 更新命令摘要

<details>
<summary> 显示更新CARLA的命令行</summary>

```sh
# 更新打包版CARLA
#   1. 删除当前版本
#   2. 按照快速安装指南获取所需版本


# 更新Linux构建
git checkout master
make clean
git pull origin master
./Update.sh


# 更新Windows构建
git checkout master
make clean
git pull origin master
#   清空`Unreal\CarlaUnreal\Content\Carla`目录
#   查看`\Util\ContentVersions.txt`
#   下载最新资源内容
#   将新内容解压至`Unreal\CarlaUnreal\Content\Carla`


# 获取开发资产
#   删除包含旧资产的`/Carla`文件夹
#   进入carla主目录
git clone https://bitbucket.org/carla-simulator/carla-content Unreal/CarlaUnreal/Content/Carla

```
</details>

---
## 获取最新二进制版本

二进制版本是预打包的，因此与特定CARLA版本绑定。要获取最新版本，请删除旧版本并按照[快速安装指南](start_quickstart.md)获取所需版本。

版本列表位于CARLA仓库的__开发__板块。另有包含最新开发状态的实验性__每日构建(Nightly build)__。

<div class="build-buttons">
<p>
<a href="https://github.com/carla-simulator/carla/blob/master/Docs/download.md" target="_blank" class="btn btn-neutral" title="查看CARLA版本列表">
<span class="icon icon-github"></span> 获取发行版</a>
</p>

<p>
<a href="http://carla-releases.s3.amazonaws.com/Linux/Dev/CARLA_Latest.tar.gz" target="_blank" class="btn btn-neutral" title="获取Linux每日构建">
<span class="icon fa-cloud-download"></span> 获取Linux每日构建</a>
</p>

<p>
<a href="http://carla-releases.s3.amazonaws.com/Linux/Dev/AdditionalMaps_Latest.tar.gz" target="_blank" class="btn btn-neutral" title="获取Linux附加地图包">
<span class="icon fa-cloud-download"></span> 获取Linux附加地图</a>
</p>

<p>
<a href="http://carla-releases.s3.amazonaws.com/Windows/Dev/CARLA_Latest.zip" target="_blank" class="btn btn-neutral" title="获取Windows每日构建">
<span class="icon fa-cloud-download"></span> 获取Windows每日构建</a>
</p>

<p>
<a href="http://carla-releases.s3.amazonaws.com/Windows/Dev/AdditionalMaps_Latest.zip" target="_blank" class="btn btn-neutral" title="获取Windows附加地图包">
<span class="icon fa-cloud-download"></span> 获取Windows附加地图</a>
</p>

</div>

---
## 更新Linux和Windows构建

更新前请确保位于本地`master`分支，之后可将变更合并或衍合到其他分支并解决可能的冲突。

```sh 
git checkout master
```

### 清理构建

进入CARLA主目录，删除先前构建生成的二进制文件和临时文件。
```sh 
make clean
```

### 从源仓库拉取

从CARLA仓库的`master`分支获取最新版本。
```sh
git pull origin master
```

### 下载资源文件

__Linux系统__
```sh
./Update.sh
```

__Windows系统__  

__1.__ 清空`Unreal\CarlaUnreal\Content\Carla`目录  
__2.__ 查看`\Util\ContentVersions.txt`  
__3.__ 下载`latest`对应内容  
__4.__ 将新内容解压至`Unreal\CarlaUnreal\Content\Carla`  

!!! 注意
    如需使用开发中的内容，请参考下方__获取开发资产__章节。

### 启动服务器

以观察者模式运行服务器以验证更新成功。

```sh
make launch
```

---
## 获取开发资产

CARLA团队使用的开发中资产存储在[公开git仓库][contentrepolink]，这些资源尚未完成，建议开发者谨慎使用。  

建议安装[git-lfs][gitlfslink]管理大文件，可显著提升二进制文件处理效率。  

__进入CARLA主目录__执行以下命令克隆仓库：  

```sh
git clone https://bitbucket.org/carla-simulator/carla-content Unreal/CarlaUnreal/Content/Carla
```

!!! 警告
    克隆前必须删除原有的`/Carla`资产文件夹，否则会导致错误。

[contentrepolink]: https://bitbucket.org/carla-simulator/carla-content
[gitlfslink]: https://github.com/git-lfs/git-lfs/wiki/Installation