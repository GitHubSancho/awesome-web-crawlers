# Redis基础
## 简介
- 不支持SQL语法，采用NoSQL，数据格式为KV形式，没有通用语言，需要了解各自API和语法与擅长业务场景
- NoSQL产品举例：Redis、Mongodb、Hbase hadoop、Cassandra hadoop
- NoSQL和SQL数据库比较
  - 适用场景：sql数据库适合用于关系特别复杂的数据查询场景，nosql反之
  - "事务"特性支持：sql对事务支持完善，nosql不支持事务。（事务：一组命令要么都成功，要么都失败）
- Redis特性：
  - 支持持久化，将内存数据保存到磁盘，重启后再次加载使用
  - 除KV结构数据外，同时支持list、set、zset、hash等
  - 支持数据备份(master-slave)
- Redis优势
  - 性能高（读110000次/秒,写81000次/秒）
  - 丰富数据类型
  - 原子性
  - 丰富特性（如publish/subscribe等）
- Redis应用场景
  - 缓存（ehcache/memcached）
  - 替代传统数据库（内存型应用）
  - 大型系统中的功能（如session共享、购物车等）
## 安装
- linux：
  1. 下载：`wget http://download.redis.io/releases/redis-7.0.2.tar.gz`
  2. 解压：`tar -zxvf redis-7.0.2.tar.gz`
  3. 移动：`sudo mv ./redis-7.0.2 /usr/local/redis/`
  4. 进入目录：`cd /usr/local/redis/`
  5. 生成：`sudo make`
  6. 测试：`sudo make test`
  7. 安装命令：`sudo make install`
  8. 查看命令：`cd /user/bin`,ls -all`
    - redis-server：redis服务器
    - redis-cli：命令行客户端
    - redis-benchmark：redis性能测试工具
    - redis-check-aof：AOF文件修复工具
    - redis-check-rdb：RDB文件检索工具
  9. 复制配置文件：`sudo cp /usr/local/redis/redis.conf /etc/redis/redis.conf `
## 配置
- 打开配置文件`sudo vi /etc/redis/redis.conf`
- 核心配置选项
  - 绑定IP：`bind 127.0.0.1`
  - 端口：`port 6379`
  - 守护进程：`daemonize yes`，推荐开启，后台运行
  - 写入的数据文件：`dbfilename dump.rdb`
  - 写入的文件目录：`dir /var/lib/redis`
  - 日志文件：`logfile /var/log/redis/redis-server.log`
  - 数据库数量：`database 16`
  - 主从复制：`slaveof`
## 服务端和客户端
- 服务端
  - 查看帮助：`redis-server --help`
  - 启动：`sudo service redis start`，（推荐下面指定配置文件方式启动）
  - 停止：`sudo service redis stop`
  - 重启：`sudo service redis restart`
  - 其它命令：
    - 查看redis服务器进程：`ps -ef|grep redis`
    - 杀死redis服务器：`sudo kill -9 [pid]`
    - 指定加载的配置文件：`sudo redis-server /etc/redis/redis.conf`
- 客户端
  - 查看帮助：`redis-cli --help`
  - 连接服务端：`redis-cli`默认本地6379端口
  - 进入后测试连接：`ping`
  - 切换数据库：`select [num]`，默认0~15号
## 数据操作

## 与Python交互

## 搭建主从

## 搭建集群
