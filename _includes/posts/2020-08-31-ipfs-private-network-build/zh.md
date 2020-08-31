# 搭建 IPFS 私有网络

## 环境

两台 Linux 设备

## 步骤

> 创建 docker-compose.yml <br>
> 通过 docker 创建 IPFS 容器 <br>
> 配置 IPFS API 以允许跨域资源共享（CORS）请求 <br>
> 共享密钥 <br>
> 移除默认的 bootstrap 节点 <br>
> 互相添加其他节点创建网络 <br>
> 测试 IPFS 私有网络是否搭建成功

### 创建 docker-compose.yml

```yaml
version: '3.4'

services:

  ipfs_host:
    container_name: ipfs_host
    image: ipfs/go-ipfs:release
    restart: always
    ports:
      - "4001:4001" # ipfs swarm - expose if needed/wanted
      - "5101:5001" # ipfs api - expose if needed/wanted
      - "8180:8080" # ipfs gateway - expose if needed/wanted
    volumes:
      - ./data:/data/ipfs
```

### 通过 docker 创建 IPFS 容器

> 分别在 xxx.xx.x.111 和 xxx.xx.x.222 节点创建 IPFS 容器

运行 docker-compose 命令

```bash
docker-compose up -d
```

### 配置 IPFS API 以允许跨域资源共享（CORS）请求

> 分别在 xxx.xx.x.111 和 xxx.xx.x.222 节点配置跨域资源共享（CORS）请求

```bash
docker exec ipfs_host ipfs config --json API.HTTPHeaders.Access-Control-Allow-Origin '["*"]'
docker exec ipfs_host ipfs config --json API.HTTPHeaders.Access-Control-Allow-Methods '["PUT", "GET", "POST"]'
```

### 共享密钥

私有网络所有的节点必须共享同一个密钥，注意不要忘记这一点

私有链密钥生成工具生成密钥文件（go get 需要配置 golang 环境）

```bash
go get github.com/Kubuxu/go-ipfs-swarm-key-gen
cd $GOPAH/src/github.com/Kubuxu/go-ipfs-swarm-key-gen/ipfs-swarm-key-gen
go build 
./ipfs-swarm-key-gen > ~/swarm.key
```

分别将生成的 swarm.key 拷贝至服务器挂载目录 data 文件夹下

```bash
scp ~/swarm.key 服务器用户名@服务器地址:/服务器改在目录data文件夹所在路径
```

重启服务

```bash
docker restart ipfs_host
```

### 移除默认的 bootstrap 节点

> 分别在 xxx.xx.x.111 和 xxx.xx.x.222 节点移除默认的 bootstrap 节点

```bash
docker exec ipfs_host ipfs bootstrap rm --all
```

### 互相添加其他节点创建网络

- 获取 xxx.xx.x.111 节点信息
  
```bash
docker exec ipfs_host ipfs id
```

```json
{
	"ID": "hash值",
	"PublicKey": "公钥值",
	"Addresses": [
		"/ip4/127.0.0.1/tcp/4001/ipfs/hash值",
		"/ip4/某个ip/tcp/4001/ipfs/hash值"
	],
	"AgentVersion": "go-ipfs/0.4.23/5b1687d",
	"ProtocolVersion": "ipfs/0.1.0"
}
```

- xxx.xx.x.222 节点添加 xxx.xx.x.111

```bash
docker exec ipfs_host ipfs bootstrap add /ip4/xxx.xx.x.111/tcp/4001/ipfs/hash值
```

同样，在其他节点执行上述步骤，把对应的ip和hash值改成所要添加的节点信息

执行下面的指令，如果显示你之前所添加的节点信息，则代表该节点添加成功

```bash
docker exec ipfs_host ipfs swarm peers
```

### 测试 IPFS 私有网络是否搭建成功

在一台机器上执行下面的指令 

```bash
docker exec ipfs_host ipfs add /data/ipfs/api
```

在另一台机器上执行下面的指令, 如果显示文件内容，则代表 IPFS 私有网络搭建成功

```bash
docker exec ipfs_host ipfs cat QmZGuZEmKBdcEsR7yiEevpS69nLKXYzmgjeoqiGrcT4RX3
```