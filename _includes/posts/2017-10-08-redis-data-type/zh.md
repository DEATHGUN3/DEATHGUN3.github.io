#Redis数据类型

## Redis五大数据类型
![](/img/reids/redis-data-type.png)
> - String（字符串）：<br>
String是redis最基本的类型，可以理解成与Memcached一模一样的类型，一个key对应一个value，一个redis中字符串value最多可以是512M。<br>
String类型是二进制安全的。意思是redis的String可以包含任何数据。比如jpg图片或者序列化的对象。
> - Hash（哈希，类似java里的Map）：<br>
Redis hash是一个键值对集合，类似Java里面的Map<String,Object>。<br>
Redis hash是一个String类型的field和value的映射表，hash特别适合用于存储对象。

