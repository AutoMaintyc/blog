---
title: "字典Hash"  # 覆盖默认分组标题
parent: C#
---

[源码](https://github.com/dotnet/runtime/blob/5535e31a712343a63f5d7d796cd874e563e5ac14/src/libraries/System.Private.CoreLib/src/System/Collections/Generic/Dictionary.cs)
# 字典Hash

默认字典非线程安全，多线程场景用 ConcurrentDictionary<TKey, TValue>

## 扩容
默认阈值 ​​0.72​​。当 条目数 / 桶数 > 0.72时触发扩容，新容量取大于当前容量两倍的​​素数  
需重新分配内存并​​重新哈希​​所有条目，建议初始化时预估容量以减少扩容  
## Hash冲突
链地址法：头插法  
质数容量优化：质数，减少哈希取模后的分布不均匀性，降低冲突概率  
空闲条目复用​​：删除条目时，字典不会立即释放空间  