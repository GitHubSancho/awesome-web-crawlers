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
- String类型
  - 字符串，二进制安全，可接受任何格式，Value最多容纳512M
  - 设置键值
    - `set [key] [value]`：key不存在是创建，存在则修改
    - `setex [key] [seconds] [value]`：设置过期时间
    - `mset [key1] [value1] [key2] [value2]`：设置多个键值
    - `append [key] [value]`：追加值
  - 获取键值
    - `get key`：获取键值，不存在返回nil
    - `mget [key1] [key2]`：获取多个键值
- 键命令
  - `keys [pattern]`：查找键，支持正则
    - `keys *`：查找所有键
  - `exists [key]`：判断键是否存在，存在返回1，不存在返回0
  - `type [key]`：查看value的数据类型
  - `del [key]`：删除键，支持多键值删除
  - `expire [key] [seconds]`：设置键值过期时间
  - `ttl [key]`：查看键值有效时间
- Hash类型
  - 哈希类型存储，结构为属性、值，值类型为String
  - 设置键值
    - `hset [key] [filed] [value]`：设置单个属性
    - `config set stop-writes-on-bgsave-error no`：关闭配置项解决不能持久化的报错
    - `hmset [key] [field1] [value1] [field2] [value2]`：设置多个属性
  - 获取键值
    - `hkeys [key]`：获取指定键的属性
    - `hget [key] [field]`：获取属性的值
    - `hmget [key] [field1] [field2]`：获取多个属性的值
    - `hvals [key]`：获取所有属性的值
  - 删除
    - `hdel [key] [field]`：删除指定的属性和对应的值，支持多个属性删除
- list类型
  - 列表元素的类型为string，按照插入顺序排序
  - 设置键值
    - `lpush [key] [value]`：从左侧插入数据，支持多元素值
    - `rpush [key] [value]`：从右侧插入数据，支持多元素值
    - `linsert [key] [before|after] [value1] [value2]`：从key列表中value1的前或后插入value2的元素值
    - `lset [key] [index] [value]`：设置指定索引位置的元素值，索引从0开始，负数索引为尾部计数
  - 获取键值
    - `lrange [key] [start] [stop]`：返回列表切片范围类的指定元素
  - 删除
    - `lrem [key] [count] [value]`：移除键中指定数量的元素
      - count > 0：从前往后移除
      - count < 0：从后往前移除
      - count = 0：移除所有
- set类型
  - 无序集合，元素为string类型，元素唯一性不重复，不能修改
  - 设置键值
    - `sadd [key] [member]`：添加元素，支持多个元素
  - 获取键值
    - `smembers [key]`：查看该集合的所有元素
  - 删除键值
    - `srem [key] [member]`：删除集合内指定元素
- zset类型
  - 有序集合，元素为string类型，元素唯一性不重复，每个元素关联一个double类型的score权重，通过权重从小到大排序，不能修改
  - 设置键值
    - `zadd [key] [score] [member]`：设置有序集合，支持多个元素，每个元素都需要一个socre权重
  - 获取键值
    - `zrange [key] [start] [stop]`：获取指定切片内的元素
    - `zrangebysocre [key] [min] [max]`：根据socre值的区间（包括mix和max）获取元素
    - `zscore [key] [member]`：获取元素的score值
  - 删除键值
    - `zrem [key] [member]`：删除指定元素，支持多元素删除
    - `zremrangebyscore [key] [min] [max]`：删除权重在指定范围的元素
## 与Python交互
- 安装包：`pip install redis`
- 导入模块：`from redis import *`
- 创建对象：`sr = StrictRedis()`，host默认为"localhost"，port默认为6379，db默认为0
```python
from redis import *

if __name__ == "__main__":
  # 创建StrictRedis对象，连接Redis数据库
  try:
    sr = StrictRedis()
    # 添加或修改一对键值，返回状态
    res = sr.set('name','Sancho')
    print(res) # True
    
    # 获取值
    res = sr.get('name')
    print(res) # b"Sancho"
    
    # 删除键值
    res = sr.delete("name")
    print(res) # 1
    
    # 获取所有键
    res = sr.keys()
    print(res)
    
  except Exception as e:
    print(e)
  
```

## 配置主从
1. 修改redis.conf文件
  - `sudo vi /etc/redis/redis.conf`
  - 修改`bind [ip]`绑定本机ip地址
  - 重启redis服务：`sudo service redis stop`，`sudo redis-server /etc/redis/redis.conf`
2. 配置从  
  - 复制redis.conf文件：`sudo cp /etc/redis/redis.conf /etc/redis/slave.conf`
  - 修改slave.conf文件：`sudo vi /etc/redis/slave.conf`
  - 编辑内容：`bind`修改为本机ip地址，`port`修改为与主机不冲突的端口，添加`slaveof [主机ip] [主机端口]`
  - 启动从数据库：`sudo redis-server /etc/redis/slave.conf`
## 搭建集群
  - 查看主从关系：`redis-cli -h [ip地址] -p [端口] info Replication`
- 数据操作
  - 主数据库修改后会备份到从数据库
  - 从数据库连接后可以读数据，不可以写

## 配置集群
- 机器1
  1. 进入Desktop目录（随意），创建conf目录
  2. conf目录下创建文件7000.conf（随意），编辑内容如下：
```
port 7000
bind [ip]
deamonize yes
pidfile 7000.pid
cluster-enabled yes
cluster-node-config-file 7000_node.conf
cluster-node-timeout 15000
appendonly yes
```
    - deamonize yes：后台运行
    - pidfile 7000.pid：进程PID
    - cluster-enabled yes：作为集群节点
    - cluster-node-config-file 7000_node.conf：节点配置文件
    - cluster-node-timeout 15000：集群节点连接超时时间
    - appendonly yes：数据文件追加
  3. conf目录下创建7001.conf，编辑内容如下：
```
port 7001
bind [本机ip]
deamonize yes
pidfile 7001.pid
cluster-enabled yes
cluster-node-config-file 7000_node.conf
cluster-node-timeout 15000
appendonly yes
```
  4. 需要在本机启动多少个就可以依次新建多少个文件
  5. `sudo redis-server 7000.conf`：依次启动所有服务，7000.conf、7001.conf...
- 机器2
  - 如上创建目录和文件，依次7010.conf、7011.conf...，并启动所有服务
- 创建集群
  1. 所有文件配置好后，拷贝包：`sudo cp /usr/share/doc/redis-tools/examples/redis-trib.rb /usr/local/bin/`
  2. 安装ruby：`sudo apt-get install ruby`，使用国内源：`gem sources --add https://gems.ruby-china.org --remove https://rubygems.org/`，安装依赖：`sudo gem install redis`
  3. 使用命令创建集群：`redis-trib.rb create --relicas 1 [本机ip]:[端口1] [本机ip]:[端口2] [外部ip]:[端口3]...`依次创建所有集群
  - 注意：必须要有3个或以上主节点，且存活的主节点数量小于总节点数量的一半时无法提供服务
- 连接集群
  - `redis-cli -h [ip] -c -p [端口]`：连接任何节点均可，主节点可以写数据（redis采用槽的方式分配节点存放数据）

## Python交互集群
- 安装包：`pip install redis-py-cluster`
- 创建py文件，示例代码：
```python
from rediscluster import *

if __name__ == "__main__":
  try:
    # 构建所有主节点
    startup_nodes = [
      {'host':'192.168.x.x','port':'xxxx'},
      {'host':'192.168.x.x','port':'xxxx'},
      {'host':'192.168.x.x','port':'xxxx'}
    ]
    # 构建StrictRedisCluster对象，连接集群
    src = StrictRedisCluster(startup_nodes=startup_nodes,decode_responses=True)
    # 设置键值
    result = src.set('name','Sancho')
    print(result) # True
    # 获取键
    name = src.get('name')
    print(name) # Sancho
  except Exception as e:
    print(e)
```

----
*[redis中文文档](http://www.redis.cn/documentation.html)*  
*[redis命令参考文档](http://doc.redisfans.com/)*  
*[redis集群搭建](http://www.cnblogs.com/wuxl360/p/5920330.html)*  
*[更多教程](https://www.runoob.com/redis/redis-tutorial.html)*  
