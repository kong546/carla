# Inverted AI 人工智能交通仿真

![inverted_ai_logo](img/logos/inverted_ai_logo.png)

<iframe width="100%" height="400px" src="https://ecosystem.carla.org/video/iai-pipeline.mp4" title="Inverted AI 交通仿真" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<br>

Inverted AI 提供基于深度生成模型的非玩家角色（NPC）仿真引擎，能够生成具有高度真实性、反应能力和行为多样性的交通参与者。该技术通过Web API与CARLA仿真器无缝集成，显著提升了CARLA在自动驾驶开发和测试中的环境真实度。

### 核心产品

#### [DRIVE](https://www.inverted.ai/apis#DRIVE)
智能驾驶行为引擎：
- 生成人类驾驶风格的多样化NPC行为
- 支持复杂交通场景的真实交互
- 提供接近真实路况的驾驶策略分布

#### [INITIALIZE](https://www.inverted.ai/apis#INITIALIZE)
场景初始化优化方案：
- 生成符合真实分布的交通参与者初始状态（密度、类型、速度、位置、朝向）
- 消除传统仿真中冗长的预运行（burn-in）过程

#### [SCENARIO](https://www.inverted.ai/apis#SCENARIO) 
全场景生成系统（目前内测阶段）：
- 支持行人、自行车、汽车、公交等多类型交通参与者
- 提供基于日志修改的多样化场景生成
- 集成交通信号灯状态控制

#### [BLAME](https://www.inverted.ai/apis#BLAME)
事故责任判定工具（目前内测阶段）：
- 自动分析碰撞事件的根本原因
- 快速定位测试中的责任主体
- 支持大规模仿真结果过滤

访问[Inverted AI官网](https://www.inverted.ai/home)获取最新信息，或查看[Python API文档](https://docs.inverted.ai/en/latest/pythonapi/)了解技术细节。