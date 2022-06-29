# Python进阶
## 开发环境配置
1.官网下载
https://www.python.org/downloads/source
2.Linux安装依赖包
```shell
sudo apt-get update
sudo apt-get install build-essential python-dev python-setuptools python-pip python-smbus
sudo apt-get install build-essential libncursew5-dev libgdbm-dev libc6-dev
sudo apt-get install zlib1g-dev libsqlite3-dev tk-dev
sudo apt-get install libssl-dev openssl
sudo apt-get install libffi-dev
```
3.安装Python3到/usr/local目录
```shell
mkdir tmp
tar xpvf python3.x.x.tar.xz -C ./tmp
cd /tmp/python3.x.x/
./configure --prefix=/usr/local
make
make altinstall
```
4.更改/usr/bin/python链接
```shell
ln -s /usr/local/bin/python3.x /usr/bin/pyton3x
ln -s /usr/local/bin/pip3.x /usr/bin/pip3x
```
## 面向对象
### 面向对象思想
- 静态语言和动态语言的区别：Python面向对象更彻底
- 函数和类也是对象：可赋值给变量、可添加到集合对象、可作为参数传递、可作为函数返回值
### type、object、class
- type生成基本类型对象，基本类型对象生成基本类型的数据对象(type → int → 1；type → class→obj)
- Object是所有对象的基类(`obj.__bases__ == <class 'object'>`)
- type是一个类，也是对象(`type.__bases__ == <class 'object'>;type(object) == <class 'type'>;object.__bases__ == ()`)
![image](https://user-images.githubusercontent.com/42240228/176146452-d1f7e1ad-2b0b-4955-a265-1da7ec80c4ff.png)
### 内置类型
- 对象特征：身份(id)、类型（type）、值
- None（全局只有一个）
- 数值（int、float、complex、bool）
- 迭代类型
- 序列类型（list、bytes\bytearry\memoryview(二进制序列)、range、tuple、str、array）
- 映射(dict)
- 集合(set、frozenset）
- 上下文管理类型(with)
- 其它：模块类型、class和实例、函数类型、方法类型、代码类型、object对象、type类型、ellipsis类型、notimplemented类型
## 魔法函数
### 魔法函数定义
- Python中内置的双下划线开头和结尾的函数
### 数据模型以及其对Python影响
- 魔法函数会被隐式调用，用以增强类（改变语法或改变语法调用）
### 魔法函数种类
- 非数学运算
- 数学运算
### 魔法函数重要性
- 对原生数据类型性能优化（CPython）
## 类和对象
### 鸭子类型和多态
- 不需要指明类的类型，可通过魔法函数改变类的功能（如果一只鸟走起来像鸭子、游泳起来像鸭子、叫起来像鸭子，那么这只鸟就可以称为鸭子）
### 抽象基类（abc模块）
- 类里定义了纯虚成员函数的类。纯虚函数只提供接口，并没有具体实现。
- 抽象基类不能被实例化，通常是作为基类供子类继承，子类中重写虚函数，实现具体的接口
- 报错方法：导入abc模块;类参数metaclass=abc.ABCMeta;@abc.abstractmethod装饰类；如此必须重写抽象基类才可以创建对象
### isintance
- isintance判断是否属于继承关系
### 类变量和对象变量
- 类对象可被类或实例调用；对象变量不能通过类调用
### 类属性和实例属性以及查找顺序
- 由内而外
- C3算法：树形深度优先，菱形广度优先
### 静态方法、类方法、对象方法
- 对象方法：类里的普通方法，传入self参数就是对象方法
- 静态方法：装饰器@staticmethod，不接收self参数，可返回类对象
- 类方法：装饰器@classmethod，传入cls参数，可返回cls对象
### 数据封装和私有属性
- 私有属性或方法可以用双下划线开头定义
- 可通过obj._cls__name访问（可解决重名问题）
### Python对象的自省机制
- 通过一定机制查询到对象的内部结构
- __dict__可以查询属性（也可以动态修改）
- dir()函数更加详细和功能强大（可对变量使用）
### super函数
- super()函数绕过查询机制调用父类方法
### mixin模式（组合）
- mixin模式特点：mixin类功能单一；不和基类关联，可以和任何基类组合，基类可以不和mixin关联就能初始化成功；不使用super()方法
### with
- 使用with语句会在初始化时进入方法的__enter__方法，退出时进入__exit__函数
### contexlib
- 可以将函数变为上下文管理器
    - 导入contextlib；装饰类@contextlib.contextmanager；使用yield关键字分割上下文语句
## 自定义序列类
## set和dict
## 对象引用、可变性、垃圾回收
## 元类编程
## 迭代器生成器
## socket编程
## 多线程、多进程、线程池
## 协程和异步IO
## 并发编程

