---
title: SuGaR 安装错误记录
pubDate: 2026-08-12
description: SuGaR安装和使用的过程
tags: [SuGar, Tech，robot，env]
---
# SuGaR在5090的安装记录
## CUDA版本
由于nvidia的sm90和sm120不兼容，对于5090显卡需要cuda12.8以上的驱动及相关文件。

torch的版本需要查询 [torch&python](https://pytorch.org/get-started/previous-versions/)

## pip install
当安装过程中提示没有torch，这个时候并不是没有装torch。
'''
pythom
import torch
print(torch.__version__)
'''
只要出现torch就表明没有问题，然后就是继续提示没有torch

'''
pip install --no-build-isolation -e .
'''
启动不隔离安装。


## pytorch3d
这个安装过程中会卡死。
第二次安装依然卡死。
目前继续测试中，后续补。

目前不支持cuda13，so sad.

# 结果
测试完
'''
python train_full_pipeline.py -s <path to COLMAP dataset> -r <"dn_consistency", "density" or "sdf"> --high_poly True --export_obj True
'''
可以进入渲染流程，但是会报错提示，ai解释是pytorch3d太低，让我安装高版本。