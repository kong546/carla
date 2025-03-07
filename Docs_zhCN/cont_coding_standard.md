# 编码标准

*   [__通用规则__](#通用规则)  
*   [__Python__](#python)  
*   [__C++__](#c++)  

---
## 通用规则

  * 使用空格而非制表符
  * 避免行尾空格，因其会造成版本差异干扰

---
## Python

  * 注释不超过80列，代码不超过120列
  * 所有代码需兼容Python 2.7和3.7
  * [Pylint][pylintlink]不应报错或警告（部分外部类如`numpy`和`pygame`例外，参见项目.pylintrc）
  * Python代码遵循[PEP8风格指南][pep8link]（尽可能使用`autopep8`自动格式化）

[pylintlink]: https://www.pylint.org/
[pep8link]: https://www.python.org/dev/peps/pep-0008/

---
## C++

  * 注释不超过80列，特殊情况下代码可略微超限以保持清晰
  * 编译不应出现错误或警告（使用`clang++-8 -Wall -Wextra -std=C++14 -Wno-missing-braces`）
  * 禁止使用`throw`，应使用`carla::throw_exception`
  * Unreal C++代码（CarlaUnreal及插件）遵循[虚幻引擎编码标准][ue4link]，但使用空格替代制表符
  * LibCarla采用改进版[Google风格指南][googlelink]
  * 服务端代码中的`try-catch`块需用`#ifndef LIBCARLA_NO_EXCEPTIONS`包裹

[ue4link]: https://docs.unrealengine.com/latest/INT/Programming/Development/CodingStandard/
[googlelink]: https://google.github.io/styleguide/cppguide.html