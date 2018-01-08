# NoSQL数据库的四大分类

## 一、KV键值
典型介绍:
- 新浪:BerkeleyDB+redis
- 美团:redis+tair
- 阿里、百度:memcache+redis

## 二、文档型数据库(bson格式比较多)
典型介绍:
- CouchDB
- MongoDB
1. MongoDB是一个基于分布式文件存储的数据库，由C++语言编写，旨在为WEB应用提供可扩展的高性能数据存储解决方案。
2. MongoDB是一个介于关系数据库和非关系数据库之间的产品，是非关系数据库当中的功能最丰富，最像关系数据库的。

## 三、列存储数据库
- Cassandra,HBase
- 分布式文件系统

## 四、图关系数据库
- 它不是放图形的，放的是关系比如朋友圈社交网络、广告推荐系统
- 社交网络，推荐系统等。专注于构建关系图谱
- Neo4J，InfoGrid
