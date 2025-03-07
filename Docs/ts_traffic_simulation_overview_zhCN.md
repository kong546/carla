# CARLA 中的交通模拟

交通模拟是自动驾驶系统训练和测试中不可或缺的组成部分。CARLA 提供多种不同的交通模拟方案来满足各类场景需求。本节将概述可用的选项，帮助您选择最适合的方案。

- [__交通管理器__](#交通管理器)
- [__场景执行器与OpenScenario__](#场景执行器与openscenario)
- [__Scenic__](#scenic)

---

## 交通管理器

[__交通管理器__](adv_traffic_manager_zhCN.md)是 CARLA 内置的客户端模块，通过 [`carla.Vehicle.set_autopilot`](https://carla.readthedocs.io/zh_CN/latest/python_api/#carla.Vehicle.set_autopilot) 方法或 [`command.SetAutopilot`](https://carla.readthedocs.io/zh_CN/latest/python_api/#commandsetautopilot) 类控制指定车辆。每个车辆的控制流程分为[多个独立阶段](adv_traffic_manager_zhCN.md#阶段说明)，分别运行于不同线程。

__适用场景：__

- 创建逼真的城市交通环境
- [定制交通行为](tuto_G_traffic_manager_zhCN.md)以设置特定训练条件
- 开发阶段功能原型设计与数据结构优化

<div class="build-buttons">
<p>
<a href="https://carla.readthedocs.io/zh_CN/docs-preview/adv_traffic_manager/" target="_blank" class="btn btn-neutral" title="前往交通管理器">
前往交通管理器</a>
</p>
</div>

---

## 场景执行器与OpenScenario

场景执行器提供[预置交通场景](https://carla-scenariorunner.readthedocs.io/zh_CN/latest/list_of_scenarios/)，支持通过Python或[OpenSCENARIO 1.0标准](https://releases.asam.net/OpenSCENARIO/1.0.0/ASAM_OpenSCENARIO_BS-1-2_User-Guide_V1-0-0.html#_foreword) [自定义场景](https://carla-scenariorunner.readthedocs.io/zh_CN/latest/creating_new_scenario/)。

OpenSCENARIO 主要用于描述涉及多车辆的复杂机动动作，其支持的[功能特性](https://carla-scenariorunner.readthedocs.io/zh_CN/latest/openscenario_support/)包括机动动作、触发条件、故事线编排等。需[单独安装](https://github.com/carla-simulator/scenario_runner)该组件。

__适用场景：__

- 构建复杂交通场景用于[CARLA排行榜](https://leaderboard.carla.org/)评估
- 定义定制化[评估指标](https://carla-scenariorunner.readthedocs.io/zh_CN/latest/metrics_module/)，支持场景回放分析

<div class="build-buttons">
<p>
<a href="https://carla-scenariorunner.readthedocs.io/zh_CN" target="_blank" class="btn btn-neutral" title="前往场景执行器">
前往场景执行器</a>
</p>
</div>

---

## Scenic

[Scenic](https://scenic-lang.readthedocs.io) 是面向机器人及自动驾驶的场景建模专用概率编程语言，提供[CARLA专用扩展](https://scenic-lang.readthedocs.io/zh_CN/latest/modules/scenic.simulators.carla.html)。通过[基础教程](tuto_G_scenic_zhCN.md)可快速掌握场景定义方法。

__适用场景：__

- 单场景定义生成多样化衍生场景
- 为动态智能体定义基于环境状态响应的概率策略

<div class="build-buttons">
<p>
<a href="https://carla.readthedocs.io/zh_CN/latest/tuto_G_scenic/" target="_blank" class="btn btn-neutral" title="Scenic教程">
前往Scenic教程</a>
</p>
</div>

如有关于CARLA交通模拟方案的疑问，欢迎访问[论坛](https://github.com/carla-simulator/carla/discussions/)或[Discord](https://discord.gg/8kqACuC)交流。

<div class="build-buttons">
<p>
<a href="https://github.com/carla-simulator/carla/discussions/" target="_blank" class="btn btn-neutral" title="CARLA论坛">
前往CARLA论坛</a>
</p>
</div>