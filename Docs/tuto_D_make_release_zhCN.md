# 如何发布新版本

> _本文档适用于需要发布新版本的开发者。_

1. **确保内容最新**<br>
   参见[资源更新指南](tuto_D_contribute_assets_zhCN.md)。

2. **更新CARLA版本号**<br>
   需修改以下文件中的版本号：_DefaultGame.ini_、_Carla.uplugin_、
   _setup.py_、_ContentVersions.txt_。使用grep命令检查当前版本号的所有引用以避免遗漏。

3. **整理变更日志**<br>
   确保CHANGELOG.md更新至最新状态，必要时重新措辞并调整结构，
   需重点突出对用户影响较大的变更项。

4. **提交变更并添加新标签**<br>
   完成所有修改后，使用`git tag -a X.X.X`命令添加新标签
   （将X.X.X替换为最新版本号），并将该版本的变更日志作为标签描述信息。

5. **标记内容仓库**<br>
   在内容仓库中添加与_ContentVersions.txt_中提交哈希完全一致的相同标签。

6. **推送变更**<br>
   将改动推送到两个仓库，推送标签可能需要使用`git push --tags`命令。
   如有必要需创建Pull Request。

7. **编辑GitHub发布页**<br>
   前往[GitHub发布页](https://github.com/carla-simulator/carla/releases)
   基于新建标签创建发布版本。待Jenkins完成最新版本构建后，
   将下载链接添加到新创建的发布页中。