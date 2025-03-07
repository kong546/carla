# 文档标准

本文档将作为贡献文档时需要遵循规范的指南和示例。

*   [__文档结构__](#文档结构)  
*   [__规范细则__](#规范细则)  
*   [__例外情况__](#例外情况)  

---
## 文档结构

我们使用Markdown与HTML标签混合编写文档，并通过[`extra.css`](https://github.com/carla-simulator/carla/tree/master/Docs/extra.css)文件进行样式定制。
更新Python API文档时，请勿直接修改Markdown文件，应编辑[`carla/PythonAPI/docs/`][fileslink]目录下的YAML文件，并运行[`doc_gen.py`][scriptlink]或执行`make PythonAPI.docs`命令。

该操作会在`carla/Docs/`目录重新生成对应的Markdown文件，供mkdocs使用。

[fileslink]: https://github.com/carla-simulator/carla/tree/master/PythonAPI/docs
[scriptlink]: https://github.com/carla-simulator/carla/blob/master/PythonAPI/docs/doc_gen.py

---
## 规范细则

*   章节之间和文档末尾必须保留空行
*   文本行宽不得超过`100`列，HTML内容、Markdown表格、代码片段和引用链接除外
*   行内链接若超出行宽限制，应使用`[名称][引用链接]`标记语法（`[引用链接]: https://`）而非`[名称](https://)`
*   使用`<br>`进行行内换行，而非在行尾添加两个空格
*   新页面起始使用`<h1>标题</h1>`作为页面标题，或使用`<hx>子标题</hx>`创建不显示在导航栏的标题
*   使用`------`下划线或`#`层级标记创建显示在导航栏的标题

---
## 例外情况

  * 通过Python脚本自动生成的文档（如PythonAPI参考文档）

实用Markdown[速查表][cheatlink]。

[cheatlink]: https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet