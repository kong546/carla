# 行人骨骼控制

本教程介绍如何通过CARLA Python API手动控制和动画化行人的骨骼。所有可用类与方法的参考详见[Python API参考文档](python_api_zhCN.md)。

*   [__行人骨骼结构__](#行人骨骼结构)  
*   [__手动控制行人骨骼__](#手动控制行人骨骼)  
	*   [连接模拟器](#连接模拟器)  
	*   [生成行人](#生成行人)  
	*   [控制骨骼系统](#控制骨骼系统)  

!!! note
    **本文档假设使用者已熟悉Python API**。<br>
    建议先阅读入门教程了解基础概念：[核心概念](core_concepts_zhCN.md)。

---
## 行人骨骼结构

所有行人共享相同的骨骼层级结构和骨骼命名。以下是骨骼层级示意图：

```
crl_root
└── crl_hips__C
    ├── crl_spine__C
    │   └── crl_spine01__C
    │       ├── ctrl_shoulder__L
    │       │   └── crl_arm__L
    │       │       └── crl_foreArm__L
    │       │           └── crl_hand__L
    │       │               ├── crl_handThumb__L
    │       │               │   └── crl_handThumb01__L
    │       │               │       └── crl_handThumb02__L
    │       │               │           └── crl_handThumbEnd__L
    │       │               ├── crl_handIndex__L
    │       │               │   └── crl_handIndex01__L
    │       │               │       └── crl_handIndex02__L
    │       │               │           └── crl_handIndexEnd__L
    │       │               ├── crl_handMiddle_L
    │       │               │   └── crl_handMiddle01__L
    │       │               │       └── crl_handMiddle02__L
    │       │               │           └── crl_handMiddleEnd__L
    │       │               ├── crl_handRing_L
    │       │               │   └── crl_handRing01__L
    │       │               │       └── crl_handRing02__L
    │       │               │           └── crl_handRingEnd__L
    │       │               └── crl_handPinky_L
    │       │                   └── crl_handPinky01__L
    │       │                       └── crl_handPinky02__L
    │       │                           └── crl_handPinkyEnd__L
    │       ├── crl_neck__C
    │       │   └── crl_Head__C
    │       │       ├── crl_eye__L
    │       │       └── crl_eye__R
    │       └── crl_shoulder__R
    │           └── crl_arm__R
    │               └── crl_foreArm__R
    │                   └── crl_hand__R
    │                       ├── crl_handThumb__R
    │                       │   └── crl_handThumb01__R
    │                       │       └── crl_handThumb02__R
    │                       │           └── crl_handThumbEnd__R
    │                       ├── crl_handIndex__R
    │                       │   └── crl_handIndex01__R
    │                       │       └── crl_handIndex02__R
    │                       │           └── crl_handIndexEnd__R
    │                       ├── crl_handMiddle_R
    │                       │   └── crl_handMiddle01__R
    │                       │       └── crl_handMiddle02__R
    │                       │           └── crl_handMiddleEnd__R
    │                       ├── crl_handRing_R
    │                       │   └── crl_handRing01__R
    │                       │       └── crl_handRing02__R
    │                       │           └── crl_handRingEnd__R
    │                       └── crl_handPinky_R
    │                           └── crl_handPinky01__R
    │                               └── crl_handPinky02__R
    │                                   └── crl_handPinkyEnd__R
    ├── crl_thigh__L
    │   └── crl_leg__L
    │       └── crl_foot__L
    │           └── crl_toe__L
    │               └── crl_toeEnd__L
    └── crl_thigh__R
        └── crl_leg__R
            └── crl_foot__R
                └── crl_toe__R
                    └── crl_toeEnd__R
```

---
## 手动控制行人骨骼

以下是使用CARLA Python API修改行人骨骼变换的详细步骤：

### 连接模拟器

导入必要库：

```py
import carla
import random
```

初始化客户端：

```py
client = carla.Client('127.0.0.1', 2000)
client.set_timeout(2.0)
```

### 生成行人

在地图生成点随机生成行人：

```py
world = client.get_world()
blueprint = random.choice(world.get_blueprint_library().filter('walker.*'))
spawn_points = world.get_map().get_spawn_points()
spawn_point = random.choice(spawn_points) if spawn_points else carla.Transform()
world.try_spawn_actor(blueprint, spawn_point)
```

### 控制骨骼系统

通过WalkerBoneControl类修改骨骼变换参数：

```py
control = carla.WalkerBoneControl()
first_tuple = ('crl_hand__R', carla.Transform(rotation=carla.Rotation(roll=90)))
second_tuple = ('crl_hand__L', carla.Transform(rotation=carla.Rotation(roll=90)))
control.bone_transforms = [first_tuple, second_tuple]
world.player.apply_control(control)
```

关键说明：
1. `bone_transforms`参数接收包含（骨骼名称，变换参数）的元组列表
2. 骨骼变换采用相对父骨骼的坐标系
3. 可在每帧调用apply_control实现骨骼动画
4. 修改父骨骼参数会级联影响子骨骼