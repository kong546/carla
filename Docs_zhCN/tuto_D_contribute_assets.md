# 如何升级资源内容

我们的资源内容存储在独立的[Git LFS仓库][contentrepolink]。作为构建系统的一部分，我们会生成并上传包含最新资源内容的压缩包，并使用当前日期和提交哈希进行标记。我们会定期更新[CARLA仓库][carlarepolink]中的资源包链接。本文档包含手动更新该链接至最新版本所需的步骤。

1. **复制您要链接的资源包标签**<br>
   该标签可在最新[Jenkins构建][jenkinslink]的制品部分查看包名称获取，例如`20190617_086f97f.tar.gz`。

2. **将标签粘贴至ContentVersions.txt**<br>
   [编辑ContentVersions.txt][cvlink]，将标签添加在文件末尾，例如`Latest: 20190617_086f97f`（不含`.tar.gz`后缀）。

3. **创建Pull Request**<br>
   提交变更并创建新的Pull Request。

[contentrepolink]: https://bitbucket.org/carla-simulator/carla-content
[carlarepolink]: https://github.com/carla-simulator/carla
[jenkinslink]: http://35.181.165.160:8080/blue/organizations/jenkins/carla-content/activity
[cvlink]: https://github.com/carla-simulator/carla/edit/master/Util/ContentVersions.txt