---
title: Base Knowledge
pubDate: 2025-12-28
description: 仿真基础概念：ODE/PDE、IMU、仿真的三大支柱与 MuJoCo XML 框架入门
tags: [仿真, MuJoCo, 机器人]
---

# 什么是仿真（Simulation）？

对现实物理世界（的现象）进行**建模** --> **预测**未来状态

- 推广到最大：world model （世界模型
- all models are wrong, but some are useful. -- George E. P. Box

## 建模的本质：求解物理现象的**Equations**
    - function
    - static relation
    - differential equation (动态系统)

## ODE or PDE
- Ordinary Differential Equation (ODE) 常微分方程
- Partial Differential Equation (PDE) 偏微分方程

## IMU (Inertial Measurement Unit) 惯性测量单元

## three pillars of a simulation (仿真的三大支柱)
1. Constucting the differential equations/models(构建微分方程) (ease of use, diversity of environments robot)
2. Solving differential equations （解微分方程） (speed and accuracy)
3. visualization of the simulation results（可视化仿真结果） (*好看不是重点)

*urdf是user与机器人交互的interface(UI)*

matlab --> black background

# MuJoCo
## XML *框架和原则，细节看xml reference*
- <body> 机器人本体 **body frame**
  - <name> 机器人的名称 *option*
  - <pos> 位置
  - <euler> 欧拉角 / <quat> 四元数 / <axisangle> 轴角
- <geom> 几何体
- <joint> 关节
- <inertial> 惯性
