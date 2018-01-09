
## Units单位
1. 配置大小单位,开头定义了一些基本的度量单位，只支持bytes，不支持bit
2. 对大小写不敏感
![](/img/redis-conf/redis-units.png)

## INCLUDES包含
和Struts2配置文件类似，可以通过includes包含，redis.conf可以作为总闸，包含其他
![](/img/redis-conf/redis-includes.png)

## GENERAL通用
1. Daemonize
2. Pidfile
3. Port
4. Tcp-backlog
- 设置tcp的backlog，backlog其实是一个连接队列，backlog队列总和=未完成三次握手队列 + 已经完成三次握手队列。<br>
- 在高并发环境下你需要一个高backlog值来避免慢客户端连接问题。注意Linux内核会将这个值减小到/proc/sys/net/core/somaxconn的值，所以需要确认增大somaxconn和tcp_max_syn_backlog两个值来达到想要的效果
5. Timeout
6. Bind
7. Tcp-keepalive
- 单位为秒，如果设置为0，则不会进行Keepalive检测，建议设置成60
8. Loglevel
9. Logfile
10. Syslog-enabled
- 是否把日志输出到syslog中
11. Syslog-ident
- 指定syslog里的日志标志
12. Syslog-facility
- 指定syslog设备，值可以是USER或LOCAL0-LOCAL7
13. Databases

## SNAPSHOTTING快照
1. Save
![](/img/redis-conf/redis-snapshotting.png)
- save 秒钟 写操作次数
- 禁用
RDB是整个内存的压缩过的Snapshot，RDB的数据结构，可以配置复合的快照触发条件，默认**是1分钟内改了1万次，或5分钟内改了10次， 或15分钟内改了1次**<br>
如果想禁用RDB持久化的策略，只要不设置任何save指令，或者给save传入一个空字符串参数也可以
2. Stop-writes-on-bgsave-error
3. rdbcompression
4. rdbchecksum
5. dbfilename
6. dir