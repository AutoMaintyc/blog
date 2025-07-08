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
`${SCE_ROOT_DIR}`/ORBIS/Tools/CMake/Extensions/CMakeExtensionsForPS4-XXXX.msi  
PS5运行  
`${SCE_ROOT_DIR}`/Prospero/Tools/CMake/Extensions/CMakeExtensionsForPS5-XXXX.msi  

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
### xLua Call C#
静态方法标记[LuaCallCSharp]特性
CS.Namespace名.Class名.方法名（）
### C# Call Lua
``` C#
LuaEnv luaEnv = new LuaEnv();
luaEnv.DoString("print('Hello from C#')");
//清理
luaEnv.Tick();
//销毁
luaEnv.Dispose();
//获取变量
luaEnv.Global.Get<int>("变量名"); 
luaEnv.Global.Get<string>("变量名"); 
```
XLua源： https://github.com/Tencent/xLua

### 变量
Lua会直接拷贝一份 被调用的C#变量，尽量不要直接把C#中的较大的数据变量直接用

## 模块和包
### 模块
遵循特定结构的 .lua 文件  
模块文件必须通过 return 语句返回一个值  
模块需要require后调用

```Lua
-- autoMain.lua （模块）
local AutoMain = {}
-- 公共接口
function AutoMain.add(a, b) return a + b end
-- 私有变量、函数，用local限定
local function exampleLocalFuction() end
-- 显式返回表
return AutoMain
```

### 包
模块的集合​​，通常以​​目录结构​​组织  
入口文件 init.lua  
在包目录下创建 init.lua，定义包入口  
当执行 require("autoMainPackage") 时，Lua 会优先查找autoMainPackage/init.lua 文件作为包的入口

```Lua
-- autoMainPackage/init.lua
local autoMainPackage = {}
--顶级代码，会直接调用
require("autoMainPackage." .. moduleName)
return autoMainPackage
```

### 使用
#### 执行require时顺序
1、package.loaded 检查缓存  
2、package.preload 检查预加载  
3、在以下路径搜索  
./?.lua  
./?/init.lua  
/usr/local/share/lua/版本号/?.lua  
/usr/local/lib/lua/版本号/?.lua  
​​4、package.cpath 检查C扩展  
5、子模块回退机制​（尝试加载父模块）    

## 协程
```Lua
coroutine.create()
coroutine.wrap()
coroutine.resume()
coroutine.yield()
```

## 面向对象
## 内存管理（垃圾收集）
## 闭包
## 反射
## 序列化

## pairs和ipairs
避免遍历时修改表​
### pairs
遍历所有键值对  
无序
### ipairrs
遍历连续数字索引,当索引不连续时中断  
有序

## table和metaTable
### table
Lua唯一的数据结构​  
table的键值分为数组和hash两部分  
#### 数组部分​​：  
键值​​不连续时，当哈希处理  
#### 哈希部分：  
开放寻址法解决哈希冲突  
无空闲节点时会Rehash（将旧表中的所有键值对重新计算哈希值并迁移到新表）
Rehash ≠ 扩容
### metaTable
metaTable​​ 是一个普通表，通过 setmetatable(t, mt) 关联到目标表 t，用于拦截其特定操作  
示例代码：  
``` Lua
local t = { a = 1 }
local mt = {
    __index = function(_, key) 
        return "Key '" .. key .. "' not found!" 
    end
}
setmetatable(t, mt)
print(t.b) -- 输出：Key 'b' not found! [1,4](@ref)
```
详情： https://www.runoob.com/lua/lua-metatables.html

## pcall和xpcall
安全调用函数,避免崩溃  
### pcall：
#### 性能:  
pcall 会创建独立调用栈，高频调用可能导致性能下降  
#### 错误信息:  
错误信息为字符串，需手动解析
​​无法捕获语法错误​，仅处理运行时错误
### xpcall：
与pcall相比：  
提供了errhandler，且会在栈展开前执行  
性能开销更多  
更详细的Log信息  