# REDIS

**一种内存存储，Key-Value存储；key只能是string对象，value支持多种对象，主要掌握以下六种：**

1. **string**
2. **list**
3. **set**
4. **hash**
5. **sorted set**
6. **stream**

**常用于缓存、消息中转、数据流引擎**

## 1. 对象类型

### 1.1 String

**缓存存 Json 字符串信息**

**可以用于计数场景（INCR，DECR）**

DECR 会将指定 key 的值减 1，如果该 key 不存在，Redis 会初始化为 0，再进行减 1 操作。

数器在一段时间后过期，可以使用 `EXPIRE` 命令。

```
SET counter 100
EXPIRE counter 3600
INCR counter
```

#### 指令

1. SET SETNX（SET key value）
   1. EX PX      NX XX
2. GET  MGET (GET key)
3. . DEL (DEL key)

#### 底层实现

实现主要分为三种

- INT -----> 整数的时候使用
- EMBSTR -----> 非整数 or 长度大于long
- RAW ----->  非整数 and 长度大于EMBSTR

EMBSTR和RAW都是由**redisObiect和SDS**两个结构组成。

它们的差异在于，**EMBSTR下redisObject和SDS是连续的内存，RAW编码下redisObject和SDS的内存是分开的。**EMBSTR优点是redisObiect和SDS两个结构可以一次性分配空间，缺点在于如果重新分配空间，整体都需要再分配，所以**EMBSTR设计为只读，任何写操作之后EMBSTR都会变成RAW**，理念是发生过修改的字符串通常会认为是易变的。

![image-20250705224634494](C:\Users\laxsm\AppData\Roaming\Typora\typora-user-images\image-20250705224634494.png)

![image-20250705224714442](C:\Users\laxsm\AppData\Roaming\Typora\typora-user-images\image-20250705224714442.png)



SDS可以避免：
1.每次计算字符串长度的复杂度为O(N);
2.对字符串进行追加，需要重新分配内存;
3.非二进制安全。

即：

1.增加长度字段len，快速返回长度

2.增加空余空间(alloc-len)，为后续追加数据留余地;

3.不再以0'作为判断标准，二进制安全。



这里可能会想，SDS可以预留空间，那么预留空间有多大呢，规则如下len小于1M的情况下，alloc=2倍*len，即预留len大小的空间:len大于1M的情况下，alloc是1M+len，即预留1M大小的空间。简单来说，预留空间为**min(len，1M)**
