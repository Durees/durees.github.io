---
title: blender制作texture贴图流程
pubDate: 2026-01-18
description: Blender 展 UV → 贴图绘制 → 导出贴图 → URDF 使用，含 Phobos 插件与环境配置踩坑
tags: [Blender, 贴图, URDF, 3D]
---

# 制作texture贴图

参考别人的方式，我现在摸索的pipeline很基础：Blender展uv -> 插件绘制贴图 -->  导出贴图 -> 在urdf中使用贴图。

## Blender展UV

使用smart UV project快速展UV。
大批量可以使用脚本自动化处理，但是这里就有个大坑**环境配置**，我的常用机器是macOS（谁家SB用mac做3D啊！！！）然后适配linux需要统一环境。

本质还是我的代码太菜哦。。。

创建conda环境，调用pip安装blender包

## 贴图绘制

然后使用插件Texture Paint Plus进行贴图绘制。

**但是这个方式不正确！！！**
正确的方式应该是使用Substance Painter进行贴图绘制，Blender的贴图绘制功能太弱鸡了，尤其是对于PBR贴图的支持。

## 导出贴图

blender有插件在支持**PBR材质编辑器**导出贴图。

# urdf插件

phobos插件支持blender导出urdf模型。

## 替换mesh

在导出urdf后，支持直接替换obj文件为带贴图的obj文件。**但是必须确保贴图和obj使用同一uv**。
