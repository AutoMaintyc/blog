---
layout: post
title:  "UI优化"
date:   2025-07-06 12:40:39 +0800
permalink: /UI优化/
parent: UI
---

UI优化的方向，分为使用时和技术向的优化
# 开发日常使用
# 技术向优化
## 内存占用
纹理压缩​​  
读写关闭​​：纹理导入设置Read/Write Enabled = false，避免内存冗余拷贝  
对象池化​​  
## 合批
静态合批​  
​​动态合批​  
​​GPU Instancing​
## CPU占用
异步加载  
线程优化：将布局等计算从主线程移除  
预初始化​​、加载  