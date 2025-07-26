---
layout: post
title:  "Shader变体"
date:   2025-07-06 09:40:39 +0800
permalink: /Shader变体/
parent: Shader
---

# Shader变体
通过#pragma multi_compile或#pragma shader_feature指令定义关键字（如SHADOWS_ON），Unity在编译时根据关键字组合生成独立变体  
运行时动态选择​​Unity根据当前材质的关键字、硬件层级（如UNITY_HARDWARE_TIER1）和图形API，动态匹配最优变体提交给GPU执行  

# 问题
关键字组合呈指数增长  
占包体，编译后很大  
首次使用时需重新编译，可能卡顿
