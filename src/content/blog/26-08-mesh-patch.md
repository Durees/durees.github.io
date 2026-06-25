---
title: mesh补面
pubDate: 2026-03-02
description: MeshLab 补面修复、Data Transfer 修改器搬运颜色、Cycles 贴图烘焙防黑
tags: [Blender, 3D, 建模, 贴图]
---

# 接补面

原文件存在多个漏洞和一个大破面，通过meshlab直接补面。小面可以补，但是破洞面积过大难以补全。

# 包裹补面

原文件是两层，相当于一个"布"。
通过裹缩重建mesh

# 贴图

## data transfer

1. 关键：点击"生成数据层" (Generate Data Layers)

这是最容易漏掉的一步。修改器如果不点这个按钮，它就像一个没有插电的抽水机，不会开始搬运数据。

    在 Data Transfer 修改器面板中。

    勾选 Face Corner Data（面拐角数据）。

    展开下方的 Colors（颜色） 选项。

    重点： 点击那个写着 Generate Data Layers（生成数据层） 的小图标（通常在最右侧）。

    此时，你会发现新模型的"颜色属性"列表里多了一项。

2. 正确的映射设置 (Mapping)

由于 GS 模型点云非常密集，建议在修改器里把映射模式改一下，效果会更准：

    Face Corner Data 选勾后。

    Mapping（映射） 建议选择：Nearest Corner and Best Matching Normal（最近角点和最佳匹配法线）。这能确保颜色不会拉伸得太难看。

3. 应用修改器 (Apply)

当你看到模型在"物体数据属性"里出现了颜色层之后：

    鼠标悬停在 Data Transfer 修改器上，按 Ctrl + A（或者点击修改器右上角的箭头选择 Apply）。

    注意： 应用后，修改器会消失，但数据会永久留在你的新网格上。

## bake

1. 检查"选中顺序"（非常重要）

在点击烘焙前，你需要同时选中两个物体。顺序决定了谁是"源"，谁是"目标"：

    先点击 原始的高斯泼溅模型（Source）。

    按住 Shift，再点击 补好面的模型/Cube（Active）。

    此时 Cube 应该在场景层级里显示为浅橙色（活动物体），而原模型是深橙色。

    确保在 Shader Editor 里，那个 Blank 图像节点是被选中的（白框）。

2. 设置烘焙参数（解决变黑问题）

    将渲染引擎切换为 Cycles（烘焙功能仅在 Cycles 下有效）。

    找到 Bake（烘焙） 面板。

    Bake Type（烘焙类型）： 选为 **Diffuse**（漫射）。*必须选择这个*

    Influence（影响）： 只勾选 Color（取消勾选 Direct 和 Indirect）。这一步最关键，它能确保你拿到的就是原始贴图的颜色，而不会受灯光影响变黑。

    勾选 Selected to Active（从选中的物体到活动的物体）。

    Extrusion（挤出）： 设置为 0.01 或 0.1（防止面重叠导致黑块）。

3. 源模型的材质必须是"自发光"

    检查原模型材质： 1. 选中原模型，看它的 Shader 节点。
    1. 它的颜色属性（Color Attribute）应该是连在 Principled BSDF 的 Emission（发射） 或者 Base Color 上。
    2. 如果连在 Base Color 上，请临时把它连到 Emission（或者直接把烘焙模式改成 Diffuse 并在 Influence 里只勾选 Color）。
