

## Redis五大数据类型
![](/img/reids/redis-data-type.png)
- String（字符串）<br>
String是redis最基本的类型，可以理解成与Memcached一模一样的类型，一个key对应一个value，一个redis中字符串value最多可以是512M。<br>
String类型是二进制安全的。意思是redis的String可以包含任何数据。比如jpg图片或者序列化的对象。
- Hash（哈希，类似java里的Map）<br>
Redis hash是一个键值对集合，类似Java里面的Map<String,Object>。<br>
Redis hash是一个String类型的field和value的映射表，hash特别适合用于存储对象。
- List(列表)<br>
Redis列表是最简单的字符串列表，按照插入顺序排序。可以添加一个元素到列表的头部(左边)或者尾部(右边)，它的底层实际是个链表。
- Set(集合)<br>
Redis的Set是String类型的无序集合。它是通过HashTable实现的。
- Zset(sorted set:有序集合)<br>
Redis zset和set一样也是String类型元素的集合，且不允许重复的成员。**不同的是每个元素都会关联一个double类型的分数。**Redis正是通过分数来为集合中的成员进行从小到大的排序。**zet的成员是唯一的，但分数(score)却可以重复。**

## Redis常见数据操作命令
![](/img/reids/redis-command-reference.png)

## Redis键(key)
常用命令参考
![](/img/reids/redis-key.png)
案例
> - keys *
> - exists key的名字，判断某个key是否存在
> - move key db   --->当前库就没有了，被移除了
> - expire key 秒钟：为给定的key设置过期时间
> - ttl key 查看还有多少秒过期，-1表示永不过期，-2表示已过期
> - type key 查看你的key是什么类型

## Redis字符串(String)

## Redis列表(List)

## Redis集合(Set)

## Redis哈希(Hash)

## Redis有序集合Zset(sorted 




