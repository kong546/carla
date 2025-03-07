# CARLA 贡献指南

CARLA 团队欢迎所有开发者参与项目贡献。根据贡献者能力的不同，我们提供多种贡献方式。团队将全力协助贡献内容顺利集成到项目中。

请选择适合您的贡献方式：

* [__报告错误__](#报告错误)  
* [__功能请求__](#功能请求)  
* [__代码贡献__](#代码贡献)  
	* [学习Unreal Engine](#学习unreal-engine)  
	* [准备工作](#准备工作)  
	* [编码规范](#编码规范)  
	* [提交流程](#提交流程)  
	* [检查清单](#检查清单)  
* [__美术资源贡献__](#美术资源贡献)  
* [__文档贡献__](#文档贡献)  

---
## 报告错误

问题请提交至GitHub的[问题板块][issueslink]。在报告新问题前请先完成以下检查：

__1. 确认问题是否已被报告__ 在GitHub问题板块搜索类似问题

__2. 查阅文档__ 确认问题确实属于程序错误，而非功能误解。请仔细阅读[文档][docslink]相关章节并查看[常见问题][faqlink]。

[issueslink]: https://github.com/carla-simulator/carla/issues
[docslink]: http://carla.readthedocs.io
[faqlink]: build_faq.md

---
## 功能请求

改进用户体验的新功能建议可通过GitHub[功能请求专区][frlink]提交。

[frlink]: https://github.com/carla-simulator/carla/issues?q=is%3Aissue+is%3Aopen+label%3A%22feature+request%22+sort%3Acomments-desc

---
## 代码贡献

开始编码前请查阅[问题看板][issueboard]了解团队当前工作重点，避免重复劳动。如有疑问请联系团队成员（或发送邮件至<carla.simulator@gmail.com>）。

__准备工作__：
1. [Fork项目仓库](https://docs.github.com/en/enterprise/2.13/user/articles/fork-a-repo)
2. 克隆fork仓库到本地
3. 定期[同步fork仓库](https://docs.github.com/en/enterprise/2.13/user/articles/syncing-a-fork)

[issueboard]: https://github.com/carla-simulator/carla/issues

### 学习Unreal Engine

建议学习资源：
- Unreal官方[C++教程][ue4tutorials]
- [Udemy Unreal C++课程][ue4course]（付费）

[ue4tutorials]: https://docs.unrealengine.com/latest/INT/Programming/Tutorials/
[ue4course]: https://www.udemy.com/unrealcourse/

### 编码规范

请严格遵守[编码规范标准](cont_coding_standard.md)。

### 提交流程

采用[Gitflow工作流](https://nvie.com/posts/a-successful-git-branching-model/)：
1. 从最新的`dev`分支创建特性分支
2. 开发完成后向`dev`分支提交Pull Request
3. CI系统自动运行检查，需全部通过
4. 合并到`dev`分支等待版本发布

### 检查清单
* [ ] 分支已同步最新dev代码
* [ ] 更新相关文档
* [ ] 通过全部编译检查
* [ ] 通过`make check`测试

---
## 美术资源贡献

包括车辆、行人、地图等3D资产：
1. 创建Bitbucket账号
2. 通过[Discord服务器](https://discord.com/invite/8kqACuC)联系美术团队获取仓库权限
3. 使用`contributors/用户名`分支提交资源
4. 按[构建指南](build_linux.md)编译CARLA后测试资源
5. 提交Pull Request等待审核

---
## 文档贡献

文档采用Markdown+HTML混合编写：
```sh
# 本地预览文档
mkdocs serve
```
提交流程：
1. 从dev分支创建文档分支
2. 按[文档标准](cont_doc_standard.md)编写
3. 提交Pull Request并添加技术文档审核者

!!! 重要提示
	文档更新需保持与dev分支同步，建议定期执行：
	```
	git pull origin dev
	```