
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
- 从库配置：slaveof 主库IP 主库端口<br>
> - 每次与master断开之后，都需要重新连接，除非你配置进redis.conf文件
> - Info replication
- 修改配置文件细节操作<br>
> - 拷贝多个redis.conf文件
> - 开启daemonize yes