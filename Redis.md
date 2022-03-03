# Redis

redis自身是一map，其中所有的数据都采用`key:value`的形式存储，数据类型指定是存储的数据的类型，也就是value部分的类型，key部分永远都是字符串。

## Redis常用数据类型

1. **通用指令**

   - 删除指定key

   ```markdown
   del key
   ```

   - 获取key是否存在

   ```markdown
   exists key
   ```

   - 获取key的类型

   ```markdown
   type key
   ```

   - 为指定key设置有效期

   ```markdown
   expire key seconds 
   ```

   - 获取key的有效时间

   ```markdown
   ttl key
   ```

   - 切换key从时效性转换为永久性

   ```markdown
   persist key
   ```

   - 为key改名

   ```markdown
   rename key newKey
   ```

   - 查询key

   ```markdown
   keys pattern
   ```

   > 查询规则
   >
   > \* 匹配任意数量的任意符号 ? 配合一个任意符号 [] 匹配一个指定符号

   

   > **keys** * 
   >
   > 查询所有
   >
   > **keys** it* 
   >
   > 查询所有以it开头
   >
   > **keys** *heima 
   >
   > 查询所有以heima结尾
   >
   > **keys** ??heima 
   >
   > 查询所有前面两个字符任意，后面以heima结尾
   >
   > **keys** user:? 
   >
   > 查询所有以user:开头，最后一个字符任意
   >
   > **keys** u[st]er:1 
   >
   > 查询所有以u开头，以er:1结尾，中间包含一个字母，s或t

1. **String**

   - 存储的是单个数据，一个存储空间保存一个数据

   基本操作：

   - 添加/修改数据

   ```markdown
   set key value
   ```

   - 获取数据

   ```markdown
   get key 
   ```

   - 删除数据

   ```markdown
   del key
   ```

   - 添加或修改多个数据

   ```markdown
   mset key1 value1 key2 value2 ...
   ```

   - 获取多个数据

   ```markdown
   mget key1 key2 ...
   ```

   - 获取数据字符个数(字符串长度)

   ```markdown
   strlen key 
   ```

   - 追加信息到原始信息后部

   ```markdown
   applend key value
   ```

   - 设置数据数值增加指定范围的值

   ```markdown
   incr key
   incrby key increment 
   incrybyfloat key increment
   ```

   - 设置数值数据减少指定范围的值

   ```markdown
   decr key 
   decrby key increment
   ```

   - 设置数据具有指定的生命周期

   ```markdown
   Redis Setex 命令为指定的 key 设置值及其过期时间。如果 key 已经存在， SETEX 命令将会替换旧的值
   setex key seconds value 
   psetex key milliseconds value
   ```

3. **hash**

   一个存储空间保存多个键值对数据，底层使用哈希表结构实现数据存储

   基本操作：

   - 添加/修改数据

   ```markdown
   hset key field value 
   ```

   - 获取数据

   ```markdown
   hget key field
   hgetall key
   ```

   - 删除数据

   ```markdown
   hdel key field1 [field2]
   ```

   - 添加或修改多个数据

   ```markdown
   hmset key field1 value1 field2 value2 ...
   ```

   - 获取多个数据

   ```markdown
   hmget key field1 field2 ...
   ```

   - 获取哈希表中字段的数量

   ```markdown
   hlen key 
   ```

   - 获取哈希表中是否存在指定的字段

   ```markdown
   hexists key field 
   ```

   - 获取哈希表中所有的字段名或字段值

   ```markdown
   hkeys key
   hvals key
   ```

   - 当指定字段不存在时，设置它的值

   ```markdown
   hsetnx key field value
   ```

   - 设置只当字段的数据数据增加指定范围的值

   ```markdown
   hincrby key field increment
   ```

1. **list**

   保存多个数据，底层使用双向链表存储结构实现

   基本操作

   - 添加/修改数据

   ```
   lpush key value1 [value2] ...
   rpush key value1 [value2] ...
   ```

   - 获取数据

   ```
   lrange key start stop
   lindex key index 
   llen key 
   ```

   - 获取并移除数据

   ```
   lpop key 
   rpop key 
   ```

   - 规定时间内获取并移除数据

   ```
   brpop key1 [key2] timeout
   blpop key1 [key2] timeout
   ```

   - 移除指定数据

   ```
   lrem key count value
   ```

2. **set**

   与hash存储结构完全相同，但是仅存储键，不存储值，并且值是不允许重复的

   - 添加数据

   ```
   sadd key member1 [member2]
   ```

   - 获取全部数据

   ```
   smembers key
   ```

   - 删除数据

   ```
   srem key member1 [member2]
   ```

   - 获取集合数据总量

   ```
   scard key
   ```

   - 判断集合中是否包含指定数据

   ```
   sismember key member
   ```

   - 随机获取集合中指定数量的数据

   ```
   srandmember key [count]
   ```

   - 随机获取集合中的某个数据并将该数据移除集合

   ```
   spop key [count]
   ```

   - 求两个集合的交、并、差集

   ```
   sinter key1 [key2] 
   sunion key1 [key2] 
   sdiff key1 [key2]
   ```

   - 求两个集合的交、并、差集并存储到指定集合中

   ```
   sinterstore destination key1 [key2] 
   sunionstore destination key1 [key2] 
   sdiffstore destination key1 [key2]
   ```

   - 将指定数据从原始集合中移动到目标集合中

   ```markdown
   smove source destination member
   ```

3. **sorted_set**

   在set基础上添加可排序字段

   - 添加数据

   ```
   zadd key score1 member1 [score2 member2]
   ```

   - 获取全部数据

   ```
   zrange key start stop 
   zrevrange key start stop
   ```

   - 删除数据

   ```
   zrem key member [member]... 
   ```

   - 按条件获取数据

   ```
   zrangebyscore key min max
   zrevrangebyscore key max min
   ```

   - 按条件删除数据

   ```
   zremrangebyrank key start stop
   zremrangebyscore key min max
   ```

   - 获取集合数据总量

   ```
   zcard key
   zcount key min max
   ```

   - 集合交、并操作

   ```
   zinterstore destination numkeys key [key ...]
   zunionstore destination numkeys key [key ...]
   ```

   - 获取数据对应的索引

   ```
   zrank key member
   zrevarank key member
   ```

   - score值获取与修改

   ```markdown
   zscore key member
   zincrby key increment member
   ```

   



