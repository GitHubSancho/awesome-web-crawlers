# MongoDB
- mongdb的优势：
  - **易拓展**：数据之间没有关系，容易拓展
  - **大数据量，高性能**：非常高的读写性能，尤其是大数据量下表现优秀
  - **灵活的数据模型**：无需事先为要存储的数据建立字段，随时可以存储自定义的数据格式
## 安装和启动
- linux([参考下载](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/))：
- ubuntu([参考下载](https://blog.csdn.net/Dwj1212/article/details/123451532))
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
- 数据查询
  - 条件查询：`db.[c_name].find({age:23})`，查询年龄为23岁的数据
  - 查询一条：`db.[c_name].findOne([condition])`
  - 美化输出：`db.[c_name].find([condition]).pretty()`
- 比较运算符
  - 等于：默认等于，没有运算符
  - 小于：`$lt`
  - 小于等于：`$lte`
  - 大于：`$gt`
  - 大于等于：`$gte`
  - 不等于：`$ne`
  - 示例：`db.[c_name].find({age:{$lte:23}})`，查询年龄小于等于23的数据
- 范围运算符
  - `$in`或`$nin`：判断是否在某个范围内
  - 示例：`db.[c_name].find({age:{$in:[8,18,23]}})`，查询年龄包含在列表内的数据
- 逻辑运算符
  - and：`db.[c_name].find({age:{$gte:23},gender:true})`,查询年龄大于等于23和性别为true的学生
  - or：`db.[c_name].find({$or:[{age:{$gt:23}},gender:false}]})`，查询年龄大于23，或性别为false的数据
- 正则表达式
  - 使用`/ /`或`$regex`编写正则
  - 示例：`db.[c_name].find({sku:/^abc/})`;`db.[c_name].find({sku:{$regex:"^abc"}})`，查询sku字段abc开头的数据
- limit和skip
  - 读取指定数量的文档：`db.[c_name].find().limit([num])`
  - 跳过指定数量的文档：`db.[c_name].find().skip([num])`
  - 同时跳过和读取指定数量的文档：`db.[c_name].find().skip([num]).limit([num])`
- 自定义查询
  - 查询满足函数条件的数据(js代码)：
```mongodb
// 查询年龄大于等于23的数据
db.[c_name].find({
  $where:function(){
    return this.age>=23;}
})
```
- 投影
  - 在返回的结果中，只选择必要的字段：`db.[c_name].find({},{_id:0,name:1})`，显示name不显示id
- 排序
  - 对集进行排序：`db.[c_name].find(),sort({gender:-1,age:1})`，根据性别降序，年龄升序
- 统计
  - 统计集中的数据条数：`db.[c_name].find({}).count()`
- 消除重复
  - 对数据进行去重：`db.[c_name].distinct('hometown',{age:{$ge:23}})`,返回hometown不重复列表且年龄大于23条件的数据
## 备份恢复
- 备份
  - `mongodump -h [dbhost] -d [dbname] -o [dir]`，将服务器地址的数据库，备份到指定目录
- 恢复
  - `mongorestore -h [dbhost] -d [dbname] --dir [dir]`，将服务器地址中需要恢复的数据库实例备份到指定目录
    
## 聚合命令
- 聚合是基于数据处理的聚合管道，每个文档通过一个由多个阶段组成的管道，可以对每个阶段的管道进行分组、过滤等功能，然后经过一系列的处理，输出相应的结果
- 建立管道：
```
db.[c_name].aggregate([
  {$match:{status:"A"}},
  {$group:{_id:"$cust_id",total:{$sum:"$amount"}}}
])
```
- 常用管道
  - `$group`：将集合中的文档分组，用于统计结果
  - `$match`：过滤数据，只输出符合条件的文档
  - `$project`：修改输入文档的结构（如重命名、增加、删除字段、创建计算结果）
  - `$sort`：将输入文档排序后返回的文档数
  - `$limit`：限制聚合管道返回的文档树
  - `$skip`：跳过指定数量的文档，并返回余下的文档
  - `$unwind`：将数组类型的字段进行拆分
- 管道表达式
  - `sum`：计算总和
  - `avg`：计算平均值
  - `min`：获取最小值
  - `max`：获取最大值
  - `push`：在结果文档中插入值到一个数组中
  - `first`：根据资源文档的排序获取第一个文档数据
  - `last`：根据资源文档的排序获取最后一个文档数据
## 索引
- 创建索引
  - 索引会让读取变快
  - `db.[c_name].ensureIxdex({name:1})`，建立以name升序的索引，1为升序，-1为降序
  - `db.[s_name].getIndexes()`：查看当前集合所有索引
  - `db.[s_name].dropIndex('索引名称')`：删除索引
## pymongdb
- 安装：`pip install pymongo`
- 实例化和插入：
```python
from pymongo import MongoClient

class TestMongo:
  def __init__(self):
    client = MongoClient(host="127.0.0.1",port=27017)
    self.collection = client["test"]["tl"] # 使用方括号方式选择数据库和集合
    
  def test_insert(self):
    # 插入数据，insert接收字典，返回objectID
    ret = self.collection.insert({"name":"Sancho","age":23})
    print(ret)
  
  def test_insert_many(self):
    # 插入多条数据
    item_list = [{"name":"Sancho{}".format(i)} for i in range(1,10)]
    # insert_many接收一个列表，列表中为所有需要的插入字典
    t = self.collection.insert_many(item_list)
    for i in t.inserted_ids:
      print(i)
  def test_find(self):
    t = self.collection.find({})
    for i in t:
      print(i)
```
----
[官方文档](https://docs.mongodb.com/manual/reference/)  
[pymongo使用参考](https://www.cnblogs.com/yj0405/p/14943478.html)  
