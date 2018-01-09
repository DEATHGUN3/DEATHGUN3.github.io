
## Units单位
1. 配置大小单位,开头定义了一些基本的度量单位，只支持bytes，不支持bit
2. 对大小写不敏感

```
# Redis configuration file example.
#
# Note that in order to read the configuration file, Redis must be
# started with the file path as first argument:
#
# ./redis-server /path/to/redis.conf

# Note on units: when memory size is needed, it is possible to specify
# it in the usual form of 1k 5GB 4M and so forth:
#
# 1k => 1000 bytes
# 1kb => 1024 bytes
# 1m => 1000000 bytes
# 1mb => 1024*1024 bytes
# 1g => 1000000000 bytes
# 1gb => 1024*1024*1024 bytes
#
# units are case insensitive so 1GB 1Gb 1gB are all the same.
```

## INCLUDES包含
和Struts2配置文件类似，可以通过includes包含，redis.conf可以作为总闸，包含其他

```
# Include one or more other config files here.  This is useful if you
# have a standard template that goes to all Redis servers but also need
# to customize a few per-server settings.  Include files can include
# other files, so use this wisely.
#
# Notice option "include" won't be rewritten by command "CONFIG REWRITE"
# from admin or Redis Sentinel. Since Redis always uses the last processed
# line as value of a configuration directive, you'd better put includes
# at the beginning of this file to avoid overwriting config change at runtime.
#
# If instead you are interested in using includes to override configuration
# options, it is better to use include as the last line.
#
# include /path/to/local.conf
# include /path/to/other.conf
```<br>

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
- save 秒钟 写操作次数
2. Stop-writes-on-bgsave-error
3. rdbcompression
4. rdbchecksum
5. dbfilename
6. dir