
## 是什么
1. 官网介绍
![](/img/redis-replication/redis-replication-intro.png)
行话：也就是我们所说的主从复制，主机数据更新后根据配置和策略，
自动同步到备机的master/slaver机制，Master以写为主，Slave以读为主
2. 能干嘛
- 读写分离
- 容灾恢复
3. 怎么玩
- 配从(库)不配主(库)
- 从库配置：slaveof 主库IP 主库端口
> - 每次与master断开之后，都需要重新连接，除非你配置进redis.conf文件
> - Info replication
- 修改配置文件细节操作<br>
> - 拷贝多个redis.conf文件 如:cp redis.conf /myredis/redis6379.conf
> - 开启daemonize yes
> - Pid文件名字 如：pidfile /var/run/redis6379.pid
> - 指定端口  如：port 6379 
> - Log文件名字  如：logfile "log6379.log"
> - Dump.rdb名字 如：dbfilename dump6379.rdb
- 常用3招<br>
一主多仆
> - Init<br>
> - 一个Master多个Slave<br>
> - 日志查看<br>
> - 主从问题演示<br>
1 切入点问题？slave1、slave2是从头开始复制还是从切入点开始复制?比如从k4进来，那之前的123是否也可以复制<br>
2 从机是否可以写？set可否？<br>
3 主机shutdown后情况如何？<br>
4 主机又回来了后，主机新增记录，从机还能否顺利复制？<br>
5 其中一台从机down后情况如何？依照原有它能跟上大部队吗？
薪火相传
> - 上一个Slave可以是下一个slave的Master，Slave同样可以接收其他
slaves的连接和同步请求，那么该slave作为了链条中下一个的master,
可以有效减轻master的写压力
> - 中途变更转向:会清除之前的数据，重新建立拷贝最新的
Slaveof 新主库IP 新主库端口
反客为主<br>
SLAVEOF no one：使当前数据库停止与其他数据库的同步，转成主数据库
4. 复制原理
- Slave启动成功连接到master后会发送一个sync命令
- Master接到命令启动后台的存盘进程，同时收集所有接收到的用于修改数据集命令，
在后台进程执行完毕之后，master将传送整个数据文件到slave,以完成一次完全同步
- 全量复制：而slave服务在接收到数据库文件数据后，将其存盘并加载到内存中。
- 增量复制：Master继续将新的所有收集到的修改命令依次传给slave,完成同步
- 但是只要是重新连接master,一次完全同步（全量复制)将被自动执行
5. 哨兵模式(sentinel)
- 是什么
> - 反客为主的自动版，能够后台监控主机是否故障，如果故障了根据投票数自动将从库转换为主库
- 怎么玩(使用步骤)
> - 调整结构，6379带着80、81
> - 自定义的/myredis目录下新建sentinel.conf文件，名字绝不能错
> - 配置哨兵,填写内容<br>
sentinel monitor 被监控数据库名字(自己起名字) 127.0.0.1 6379 1<br>
 上面最后一个数字1，表示主机挂掉后salve投票看让谁接替成为主机，得票数多少后成为主机
> - 正常主从演示
> - 原有的master挂了
> - 投票新选
> - 重新主从继续开工,info replication查查看
> - 问题：如果之前的master重启回来，会不会双master冲突？不会
- 一组sentinel能同时监控多个Master
6. 复制的缺点
- 复制延时<br>
由于所有的写操作都是先在Master上操作，然后同步更新到Slave上，所以从Master同步到Slave机器有一定的延迟，当系统很繁忙的时候，延迟问题会更加严重，Slave机器数量的增加也会使这个问题更加严重。
