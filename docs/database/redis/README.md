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



## 1.2 List

+ 字符串集合
+ 存储一批数据、消息
+ 消息队列

#### 指令

+ ```redis
  LPUSH   RPUSH (LPUSH key value1 value2...)
  ```

+ ```
  LPOP  RPOP  LREM(LREM key count value,  从左到右移除 count 个等于value的元素，0表示全部移除)
  ```

+ ```
  DEL (DEL key)   UNLINK(异步删除)
  ```

+ ```
  LLEN (LLEN key，key的长度)   LRANGE(LRANGE key start stop)
  ```

  

#### 编码方式

主要为三种：

ZIPLIST

LINKERLIST

QUICKLIST

------



##### ZIPLIST

+ 所有对象长度都小于**64字节**
+ 元素数量小于**512个**
+ 节约链表指针开销，小数据量遍历性能好

![image-20250107115524334](C:\Users\smc.shang\AppData\Roaming\Typora\typora-user-images\image-20250107115524334.png)

​		zlbytes：  包含本身，一共占了多少个字节

​		zltail：	  偏移的字节数，zl+zltail定位到尾节点；zl是开始节点指针

​		zlen：	   表示有多少个数据节点

​		zlend：	结束标志位

​		

​		数据中，entry由 prevlen  encoding  entry-data构成

​		prevlen：上一个节点的数据长度，可以p-prevlen定位到上一个节点的开头位置，所以可以从后往前遍历

​		如果前一节点的长度，也就是前一个ENTRY的大小，**小于254字节**， 那么**previen属性需要用1字节**长的空间来保存这个长度值，255是特殊字符，被zlend使		用，如果前一节点的**长度大于等于254字节**，**那么prevlen属性需要用5字节长的空间来保存这个长度值**，注意5个字节中的第一个字节为11111110，也就是		254，标志这是个5字节的prevlen信息，剩下4字节来表示大小。

​		encoding：可以知道当前节点的长度

​		由于ZIPLIST的header定义了记录节点数量的字段zllen，所以**通常是可以在O(1)时间复杂度直接返回**的，为什么说通常呢?是因为zlen是2个字节的，**当zllen大		于65535时，zlen就存不下了**，所以真实的节点数量需要**遍历**来得到。
​		这样设计的原因是Redis中应用ZIPLIST都是为了节点个数少的场景，所以将zllen设计得较小，节约内存空间。

​		在ZIPLIST中查询指定数据的节点，需要遍历这个压缩列表，平均时间复杂度是0(N).



​		更新数据是O(n)，但是容易引起连锁反应，即prevlen 1字节和5字节的变换导致后面的变化的连锁反应。



​		通个ListPack解决，即不记录prevlen ，记录本身长度

##### LINKEDLIST

节点是指针相连

![image-20250107115534910](C:\Users\smc.shang\AppData\Roaming\Typora\typora-user-images\image-20250107115534910.png)

##### QUICKLIST

![image-20250107115548574](C:\Users\smc.shang\AppData\Roaming\Typora\typora-user-images\image-20250107115548574.png)3

单个节点是ziplist

LINKEDLIST编码下，查询节点个数的时间复杂度是多少   O(1)
