# C++ 参考文档
我们使用Doxygen生成C++代码文档：

[Libcarla/Source](http://carla.org/Doxygen/html/dir_b9166249188ce33115fd7d5eed1849f2.html)<br>
[Unreal/CarlaUnreal/Source](http://carla.org/Doxygen/html/dir_733e9da672a36443d0957f83d26e7dbf.html)<br>
[Unreal/CarlaUnreal/Carla/Plugins](http://carla.org/Doxygen/html/dir_8fc34afb5f07a67966c78bf5319f94ae.html)

生成文档可通过此链接访问：**<http://carla.org/Doxygen/html/index.html>**

!!! note
    文档更新由GitHub自动完成。

### 生成Doxygen文档

!!! important
    需要安装[Doxygen](http://www.doxygen.nl/index.html)生成文档工具
    以及[Graphviz](https://www.graphviz.org/)图形可视化工具集。

1- 使用以下命令安装doxygen和graphviz：

```sh
# linux
> sudo apt-get install doxygen graphviz
```

2- 安装完成后，进入包含_Doxyfile_文件的项目根目录，运行：

```sh
> doxygen
```

该命令将开始构建文档网页。
生成结果位于Doxygen/html/目录

3- 在浏览器中打开_index.html_文件，即可查看本地C++文档。