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


