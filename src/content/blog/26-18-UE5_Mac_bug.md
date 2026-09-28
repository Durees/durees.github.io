---
title: UE5启动metal报错
pubDate: 2026-08-12
description: 修复bug
tags: [UE, mac，metal，xcode]
---
# UE5运行在mac启动metal报错
'''
Xcode Metal Compiler error: error: error: cannot execute tool 'metal due to
missing Metal Toolchain; use: xcodebuild -downloadComponent MetalToolchain
'''
一启动就一个弹窗，如上所示。

[【UE5 Mac】Fix Xcode Metal Compiler error — download Metal Toolchain](https://nannyakore.com/en/blog/ue5-metalerror-en/)

# 解决办法
更新Xcode > setting > components: metal toolchain