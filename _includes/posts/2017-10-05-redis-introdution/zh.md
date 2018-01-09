

## Redis是什么
![](/img/redis/redis-intro.png)
Redis:REmote DIctionary Server(远程字典服务器)。<br>
Redis是完全开源免费的，用C语言编写的，遵守BSD协议，是一个高性能的(key/value)分布式内存数据库，基于内存运行并支持持久化的NoSQL数据库，是当前最热门的NoSQL数据库之一，也被人们称为数据结构服务器。<br>
Redis与其他key-value缓存产品有以下是三个特点：
- Redis支持数据的持久化，可以将内存中的数据保持在磁盘中，重启的时候可以再次加载进行使用
- Redis不仅仅支持简单的key-value类型的数据，同时还提供list，set，zset，hash等数据结构的存储
- Redis支持数据的备份，即master-slave模式的数据备份

## Redis能干嘛
1. 内存存储和持久化：redis支持异步将内存中的数据写到硬盘上,同时不影响继续服务
2. 取最新N个数据的操作，如：可以将最新的10条评论的ID放在Redis的List集合里面
3. 模拟类似于HttpSession这种需要设定过期时间的功能
4. 发布、订阅消息系统
5. 定时器、计数器

## Redis中英文参考文档
https://redis.io/<br>
![](/img/redis/redis-en-h.png)
http://www.redis.cn/<br>
![](/img/redis/redis-ch-h.png)

## Redis怎么玩
1. 数据类型、基本操作和配置
2. 持久化和复制，RDB/AOF
3. 事务的控制
4. 复制
5. 其他

