---
layout: post
title:  "使用Lua遇到过的问题"
date:
permalink: /使用Lua遇到过的问题/
nav_order: 3
---

# 使用Lua遇到过的问题
以下提到的lua绝大多数都为XLua

## 如何Debug
LuaIDE、EmmyLua

## 版本
使用前注意项目的lua版本！！！

## 如何在Playstation平台运行
### 以Xlua为例
#### 编译准备：
CMake，PS的对应工具软件SDK
#### 操作步骤：
安装SDK的CMake扩展：
PS4运行
${SCE_ROOT_DIR}/ORBIS/Tools/CMake/Extensions/CMakeExtensionsForPS4-XXXX.msi
PS5运行
${SCE_ROOT_DIR}/Prospero/Tools/CMake/Extensions/CMakeExtensionsForPS5-XXXX.msi

结束后运行cmake
选好source code（Xlua源码）文件夹和build文件夹
点击 Configure

platform 填写 ORBIS(PS4的)或Prospero(PS5的)
选择 Specify toolchain file for cross-compiling
点击 NEXT

选择路径
PS4的: ${SCE_ROOT_DIR}/ORBIS/Tools/CMake/PS4.cmake
PS5的: ${SCE_ROOT_DIR}/Prospero/Tools/CMake/PS5.cmake
点击 Finish

等待 直到出现 Configuring done
点击Generate
等待 直到出现 Generating done
结束

## 与C#交互
### Lua Call C#
### C# Call Lua
### 变量
Lua会直接拷贝一份 被调用的C#变量，尽量不要直接把C#中的较大的数据变量直接用

## 模块和包
## 协程
## 序列化
## 面向对象
## 内存管理（垃圾收集）
## 闭包
## 反射
## pairs和ipairs
## table和metaTable
## 错误处理、pcall