# SAPIEN Open-Source Manipulation Skill (ManiSkill)

## SAPIEN

SAPIEN 模拟器为机器人、刚体和铰接物体提供物理模拟。它通过纯 Python 界面为强化学习和机器人技术提供支持。它还提供多种渲染模式，包括深度图、法线图、光流、主动光和光线追踪。

[Sapien文档](https://sapien-sim.github.io/docs/user_guide/getting_started/installation.html)

## PartNet-Mobility Dataset

PartNet-Mobility 数据集是带有运动注释和渲染材质的 2K 铰接对象的集合。该数据集为通用计算机视觉和操作的研究提供了动力。该数据集是 ShapeNet 和 PartNet 的延续。

https://sapien.ucsd.edu/browse

## ManiSkill

官网：https://www.maniskill.ai/research

### 自定义任务构建

1. 继承自`BaseEnv`
2. 环境注册是通过 `@register_env(env_id, max_episode_steps=...)`

```python
import sapien
from mani_skill.utils import sapien_utils, common
from mani_skill.envs.sapien_env import BaseEnv
from mani_skill.utils.registration import register_env

@register_env("PushCube-v1", max_episode_steps=50)
class PushCubeEnv(BaseEnv):
    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
```

#### Loading

环境调用 `env._reconfigure()` 来加载机器人、资产、关节、照明等，加载完成后环境会被冻结，资产无法添加或者移除，直到下一次 `env._reconfigure()` 被调用，所以通常只会被调用一次，目标是简单地以初始姿势加载对象，以确保它们不会在第一步发生碰撞。

##### 加载机器人

通过 init 函数指定添加的默认机器人。在 PushCube 中，这是通过添加 `SUPPORTED_ROBOTS` 来完成的，以确保用户只能使用选定的机器人运行您的任务。如果您愿意，您可以进一步添加 `agent` 类属性的类型。



##### 加载Actor

使用`builder.build_kinematic`构建运动学actor，并使用`builder.build_static`构建静态actor。

- 能使用静态的就不要使用动态的
- 运动和静态角色固定在适当的位置，但可以阻止物体穿过它们（例如墙壁、厨房柜台）。
- 运动演员可以随时改变姿势。静态演员在通过builder.initial_pose = ...调用build_static之前必须设置初始姿势



### 加载 Actors 和 Articulations

#### 加载Actors

有两种方法，直接从现有的模拟就绪资产数据集加载，或通过较低级别的 ActorBuilder API 加载。

##### 从现有数据集加载





##### 使用 ActorBuilder API





#### 加载关节





### 自定义机器人



### 传感器/相机





### 自定义可重复使用场景



### 域随机化



