# MongoDB
- mongdb的优势：
  - **易拓展**：数据之间没有关系，容易拓展
  - **大数据量，高性能**：非常高的读写性能，尤其是大数据量下表现优秀
  - **灵活的数据模型**：无需事先为要存储的数据建立字段，随时可以存储自定义的数据格式
## 安装和启动
- linux([参考](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/))：
  1. `sudo apt-get install -y mongodb-org`
  2. 解压：`tar -zxvf mongodb-linux-x86_64-ubuntu1604-3.4.0.tgz`
  3. 移动到/usr/local/目录：`sudo mv -r mongodb-linux-x86_64-ubuntu1604-3.4.0.tgz /usr/local/mongodb`
  4. 将可执行文件添加到PATH路径中：`export PATH=/usr/local/mongodb/bin:$PATH`
- 启动
  - 查看帮助：`mongod -help`
  - 启动：`sudo service mongod start`或`sudo mongod --config /usr/local/mongodb/mongod.conf`
  - 停止：`sudo service mongod stop`
  - 重启：`sudo service mongod restart`
  - 查看状态：`os ajx|grep mongod`
  - 配置文件位置：`/etc/mongod.conf`
  - 默认端口：`27017`
  - 日志位置：`/var/log/mongdb/mongod.log`
  - 启动本地客户端：`mongo`
  - 查看客户端帮助：`mongo -help`
  - 退出客户端：`exit`或`Ctrl+c`
## 增删改查
- 基础命令
  - 查看当前数据库：`db`
  - 查看所有数据库：`show dbs`或`show databases`
  - 切换数据库：`use [db_name]`，没有数据库则创建
  - 删除当前数据库：`db.dropDatabase()`
- 集合基础命令
  - 向不存在的集合第一次加入数据时，集合自动创建
  - 手动创建：`db.createCollection([name])`
    - 设置字节上限：`db.createCollection([name]){capped:true,size:10}`
  - 查看集合：`show collections`
  - 删除集合：`db.[Collection_name].drop()`  
- 数据类型
  - Object ID：文档ID
  - String：字符串，最常用，必须是有效UTF-8
  - Boolean：布尔值(true或false)
  - Integer：整数，32位或64位（取决于服务器）
  - Double：存储浮点值
  - Arrays：数组或列表，多个值存储到一个键
  - Object：用于嵌入式文档（一个值为一个文档）
  - Null：Null
  - Timestamp：时间戳（1970-1-1到目前的秒数）
  - Date：存储当前日期或时间的UNIX时间格式
- 数据基础操作
  - 插入：`db.[Collection_name].insert([document])`，如果id已存在则报错
  - 指定id插入：`db.[Collection_name].insert({_id:[id],[document]})`
  - 保存：`db.[Collection_name].save([document])`，如果文档id存在则修改，如果不存在则添加
  - 查询：`db.[Collection_name].find()`
  - 更新：`db.[Collection_name].update([query],[update],{multi:[boolean]})`
    - `query`：查询条件
    - `update`：更新操作符
    - `multi`：可选，默认false，表示只更新找到的第一条记录，值为true表示把满足条件的文档全部更新，只有和`$`一起使用才有效
    - `db.[Collection_name].update([query],{$set:[update]})`：只更新匹配的字段，不全部替换
  - 删除：`db.[Collection_name].remove([query],{justOne:[boolean]})`
    - `query`：可选，删除文档的条件
    - `justOne`：可选，默认为false删除多条，设置为`true`或`1`则删除一条
## 高级查询

## 备份恢复

## 聚合命令

## sort、skip、limit

## unwind

## 索引

## pymongdb
