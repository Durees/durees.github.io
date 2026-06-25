---
title: MuJoCo 学习笔记
pubDate: 2025-12-28
description: MuJoCo 安装与入门、重力补偿原理、执行器类型与控制信号匹配问题
tags: [MuJoCo, 仿真, 强化学习, 机器人]
---

# MuJoCo 安装与入门

MuJoCo（Multi-Joint dynamics with Contact）是一个用于物理仿真的高性能引擎，广泛应用于机器人学和强化学习领域。以下是安装和入门的基本步骤：

- 访问 [MuJoCo 官方网站](https://mujoco.org/) 下载最新版本的 MuJoCo。
- 解压下载的文件，并将其放置在合适的目录中（例如 `~/.mujoco/mujoco210`）。
- 设置环境变量 `MUJOCO_PY_MUJOCO_PATH` 指向 MuJoCo 的安装路径。
- 安装 Python 绑定 `mujoco-py`，可以使用 pip：
  ```bash
  pip install mujoco
  ```

# gemini给出的示例代码解析

```
import mujoco
import mujoco.viewer
import time
import numpy as np

## 1. 加载模型 (请将 path_to_your_arm.xml 替换为你实际的路径)
model = mujoco.MjModel.from_xml_path("franka_emika_panda/scene.xml")
data = mujoco.MjData(model)

# 2. 启动可视化交互窗口
with mujoco.viewer.launch_passive(model, data) as viewer:
    # 保持仿真运行
    while viewer.is_running():
        step_start = time.time()

        # --- 这里是你的控制逻辑区 ---
        
        # 核心功能：重力补偿 (Gravity Compensation)
        # qfrc_bias 存储了为了抵消重力、科氏力等所需的力矩
        # 我们把这个力直接赋值给执行器 (ctrl)，机器人就能"浮"在空中
        data.ctrl[:] = data.qfrc_bias[:len(data.ctrl)]
        
        # -----------------------

        # 3. 物理步进
        mujoco.mj_step(model, data)

        # 4. 同步渲染频率
        viewer.sync()
        
        # 尽量维持实时仿真速度
        time_until_next_step = model.opt.timestep - (time.time() - step_start)
        if time_until_next_step > 0:
            time.sleep(time_until_next_step)
```

### 现象：机械表疯狂转动不停止

手臂"疯狂转动"通常是因为控制信号（ctrl）与执行器（actuator）的类型不匹配导致的。
在 MuJoCo 中，不同类型的执行器（如位置控制、速度控制、力控制）需要不同的控制信号。如果你将一个执行器设置为位置控制，但提供了力控制信号，机械臂可能会表现出异常行为。

#### 常见的执行器类型包括但不限于：

- 位控制（Position Control）：控制信号表示期望的位置。
- 力控制（Force Control）：控制信号表示施加的力或力矩。
- 速度控制（Velocity Control）：控制信号表示期望的速度。

#### 其他一些执行器类型还包括：

- 力矩控制（Torque Control）：控制信号表示施加的力矩。
- 混合控制（Hybrid Control）：结合多种控制方式。
- 弹簧阻尼控制（Spring-Damper Control）：模拟弹簧和阻尼器的行为。
- 肌肉模型控制（Muscle Model Control）：模拟生物肌肉的行为。
- 自定义控制（Custom Control）：用户定义的控制方式。
- 环境反馈控制（Environment Feedback Control）：基于环境状态的控制。
- 学习型控制（Learning-based Control）：通过机器学习方法实现的控制。
- 自适应控制（Adaptive Control）：根据系统变化自动调整控制参数。
- 鲁棒控制（Robust Control）：在不确定性条件下保持性能的控制。
- 分布式控制（Distributed Control）：多个控制器协同工作。
- 非线性控制（Nonlinear Control）：处理非线性系统的控制方法。
- 最优控制（Optimal Control）：通过优化方法实现的控制策略。
- 预测控制（Predictive Control）：基于未来状态预测的控制方法。
- 多变量控制（Multivariable Control）：同时控制多个变量的系统。
- 分层控制（Hierarchical Control）：多层次的控制结构。
- 事件驱动控制（Event-driven Control）：基于事件触发的控制机制。
- 混沌控制（Chaotic Control）：利用混沌理论实现的控制方法。
- 量子控制（Quantum Control）：基于量子力学原理的控制方法。
- 生物启发控制（Bio-inspired Control）：模仿生物系统的控制策略。
- 智能控制（Intelligent Control）：结合人工智能技术的控制方法。
- 模糊控制（Fuzzy Control）：基于模糊逻辑的控制方法。
- 神经网络控制（Neural Network Control）：利用神经网络实现的控制策略。
- 遗传算法控制（Genetic Algorithm Control）：通过遗传算法优化的控制方法。
- 粒子群优化控制（Particle Swarm Optimization Control）：利用粒子群优化算法实现的控制策略。
- 强化学习控制（Reinforcement Learning Control）：通过强化学习方法实现的控制。
- 模仿学习控制（Imitation Learning Control）：通过模仿专家行为实现的控制策略。
- 分布式智能控制（Distributed Intelligent Control）：结合分布式系统和智能控制方法。
- 协同控制（Cooperative Control）：多个系统协同工作的控制策略。

确保你的控制信号类型与执行器类型匹配。例如，如果你的执行器是位置控制类型，你应该提供期望的位置值，而不是力矩值。

ur5e.xml 中的执行器类型是位置控制（position），而代码中提供的是力矩值（data.qfrc_bias），这就导致了机械臂的异常行为。要解决这个问题，你需要根据执行器的类型调整控制信号的赋值方式。

```
# 在循环开始前，记录初始姿势
initial_qpos = np.array(data.qpos[:model.nu])

# 在 while 循环中
while viewer.is_running():
    # 发送位置指令，而不是力矩指令
    data.ctrl[:] = initial_qpos  
    
    mujoco.mj_step(model, data)
    viewer.sync()
```

改为位控后保持静止。

### 怎么用鼠标拽动机械臂

按住ctrl键，然后用鼠标左键点击并拖动机械臂的某个部分，就可以直接操纵机械臂的位置和姿态。释放鼠标按钮后，机械臂会根据新的位置继续进行仿真。
