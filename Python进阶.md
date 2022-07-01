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
### 序列类型分类
- 容器序列：list、tuple、deque
- 扁平序列：str、bytes、bytearray、array.array
- 可变序列：list、deque、bytearray、array
- 不可变序列：str、tuple、bytes
### 序列的abc继承关系
- Sequence继承Reversible,Collection两个类，Collection继承了Sized,Iterable,Container三个抽象基类，满足了特性则构成此序列协议
  - Sized实现了len的魔法函数方法，可计算长度
  - Iterable实现了iter的魔法函数方法，可实现迭代
  - Container实现了contains的魔法函数方法，可实现判断
- MutableSequence继承Sequence类，其添加了setitem和delitem等魔法函数方法，则构成了可变序列协议
### 序列的+、+=和extend的区别
- +只能拼贴相同数据类型，+=接受拼贴其它数据类型
  - +=通过MutableSequence的iadd魔法函数实现，iadd调用extend方法用for循环将值append到值里（理论上可接受所有可迭代类型）
  - extend方法支持可迭代类型拼贴,append会直接把值传递拼贴不会进入for循环
```python
a = [1,2]
b = a + [3,4] # [1,2,3,4]
a += (3,4) # [1,2,3,4]
a.extend([5,6]) # [1,2,3,4,5,6]
a.append((7,8)) #[1,2,3,4,5,6,(7,8)]
```
### 实现可切片的对象
- 切片格式[start: end: step]
- start为起始位置默认为0，end截止（不包含）位置，step为步长默认为1
  - start为0可省略，step为1可省略（也可省略第二个冒号）
  - step为负整数表示反向切片，此时start应比end值大
  - 切片开始位置大于列表长度，返回空列表；切片结束位置大于列表长度，则从尾部截断
- 切片可实现增加、插入、替换、删除元素，返回新片段
- 函数中重写getitem魔法函数既可以实现函数切片
```python
import numbers

# 函数切片的实现
class Group:
    def __init__(self, staffs, group_name, company_name):
        self.staffs = staffs
        self.group_name = group_name
        self.company_name = company_name

    # def __getitem__(self, item): # 此item接受的Python自动转化后的slice对象
    #     return self.staffs[item]

    def __getitem__(self, item):
        cls = type(self)
        if isinstance(item, slice):
            return cls(group_name=self.group_name,
                       company_name=self.company_name,
                       staffs=self.staffs[item])
        elif isinstance(item, numbers.Integral):
            return cls(group_name=self.group_name,
                       company_name=self.company_name,
                       staffs=[self.staffs[item]])


staffs = ["Sancho", "San", "Cho"]
group = Group(company_name="com", group_name="user", staffs=staffs)
sub_group = group[:]  # ['Sancho', 'San', 'Cho']
sub_group = group[0]  # ['Sancho']
```
### bisect管理可排序序列
- 用来处理和维持已排序的升序序列，二分查找算法
```python
import bisect

inter_list = []
bisect.insort(inter_list, 3)
bisect.insort(inter_list, 2)
bisect.insort(inter_list, 1)
print(inter_list)  # [1, 2, 3]

print(bisect.bisect(inter_list, 2))  # 2;在相同元素后插入
print(bisect.bisect_left(inter_list, 2)) # 1;在相同元素前插入
```
### 列表之外的数据类型
- Python有很多数据类型，在很多应用场景之下会比list高效（如array,deque）
- array和list区别：array只能存放指定数据类型
### 列表推导式、生成器表达式、字典推导式、集合推导式
```python
# 提取出1-10之间的奇数
odd_list = []
for i in range(11):
    if i % 2 == 1:
        odd_list.append(i)
print(odd_list)  # [1, 3, 5, 7, 9]

# 列表推导式
odd_list2 = [i for i in range(11) if i % 2 == 1]
print(odd_list2)  # [1, 3, 5, 7, 9]

# 列表生成式(逻辑复杂时不建议使用)
odd_gen = (i for i in range(11) if i % 2 == 1)
print(list(odd_gen))  # [1, 3, 5, 7, 9]

# 字典推导式
my_dict = {"baidu": "www.baidu.com", "taobao": "www.taobao.com"}
reversed_dict = {value: key for key, value in my_dict.items()}
print(reversed_dict)  # {'www.baidu.com': 'baidu', 'www.taobao.com': 'taobao'}

# 集合推导式
my_set = {key for key in my_dict.keys()}
print(my_set) #{'baidu', 'taobao'}
```
## set和dict
### set和dict的继承关系
- dict属于mapping类型
```python
from collections.abc import Mapping,MutableMapping
a = {}
print(isinstance(a,MutableMapping)) # True
```
### dict常见方法
- .clear()
- .copy()浅拷贝，copy.deepcopy()深拷贝
- .fromkeys()将可迭代对象转化为dict
- .get()取值`dict.get('bobby',{}) # 替换dict['bobby']形式，如果取值为空将返回给定对象`
- .items()，.keys()
- .setdefault()向get一样取值，如果不存在则添加
- .update()拼贴可迭代对象
### dict子类
- 不建议继承内置数据结构
```python
class Mydict(dict):
    def __setitem__(self, key, value):
        super().__setitem__(key, value * 2)


my_dict = Mydict(one=1)
print(my_dict)  # {'one': 1};不会继承父类方法
my_dict["one"] = 1
print(my_dict)  # {'one': 2}



from collections import UserDict

# 如果一定要继承基本数据结构则选择以下方式
class Mydict2(UserDict): # 改变继承对象
    def __setitem__(self, key, value):
        super().__setitem__(key, value * 2)


my_dict = Mydict2(one=1)
print(my_dict)  # {'one': 2}



from collections import defaultdict

my_dict = defaultdict(dict)
my_value = my_dict['bobby'] # 没有找到索引值则调用__missing__方法，返回空字典而非报错
print(my_value) # {}
```
### set和frozenset
- set不可变集合，无序，不重复，使用哈希实现，时间复杂度1
  - .difference()返回差集
  - |&-等等集合运算
- frozenset无法修改
### dict和set实现原理
- dict查找性能大于list
- list数据增大，查找时间也会增大；dict数据查找时间不会因数据量而增大
- dict和set采用哈希表算法
- dict内存开销大
- dict存储顺序和元素添加顺序有关
- 添加数据有可能改变已有数据的顺序
## 对象引用、可变性、垃圾回收
### 变量
- Python变量实质是一个指针
```python
a = 1
b = a
print(id(a) == id(b)) # True
```
### ==和is的区别
- 当使用==时会进入对象的__eq__()魔法函数里，以此返回值判断相等
- is判断是否是同一对象
```python
a = [1,2]
b = [1,2]
print(a is b) # False
print(a == b) # True

a = 1
b = 1
print(a is b) # True;inter机制，将短字符串、短数字等简单数据直接引用不额外创建对象的优化策略
```
### del语句和垃圾回收
- Python中垃圾回收算法是引用计数
  - 在对象创建时对其计数，当删除时，删除变量并且计数减少，归零后清除
  - 一般使用del是，其实是调用类的__del__()魔法函数来执行内存清理
```python
a = object()
b = a
del a
print(b) # <object object at 0x0000028C24258490>
print(a) # NameError: name 'a' is not defined
```
### 经典错误
- 传递参数为可变值（默认值可以修改通过__defaults__()魔法函数）
```python
def add(a, b):
    a += b
    return a


a = 1
b = 2
c = add(a, b)
print(a, b, c)  # 1 2 3

a = [1, 2]
b = [3, 4]
c = add(a, b)
print(a, b, c)  # [1, 2, 3, 4] [3, 4] [1, 2, 3, 4]

a = (1, 2)
b = (3, 4)
c = add(a, b)
print(a, b, c)  # [1, 2, 3, 4] [3, 4] [1, 2, 3, 4]


class Company:
    def __init__(self, name, staffs=[]):
        self.name = name
        self.staffs = staffs

    def add(self, staff_name):
        self.staffs.append(staff_name)

    def remove(self, staff_name):
        self.staffs.remove(staff_name)


com1 = Company("com1", ["bobby1", "bobby2"])
com1.add("bobby3")
com1.remove("bobby1")
print(com1.staffs)  # ['bobby2', 'bobby3']

com2 = Company("com2")
com2.add("bobby")
print(com2.staffs)  # ['bobby']

com3 = Company("com3")
com3.add("bobby5")
print(com2.staffs)  # ['bobby', 'bobby5']
print(com3.staffs)  # ['bobby', 'bobby5']
print(com2.staffs is com3.staffs)  #True
"""
com2和com3因为默认没有传递staffs参数，则使用默认的[]列表；
list是可变对象；
因此com2和com3公用一个对象
"""
```
## 元类编程
### property动态属性
- `if __name__ == "__main__":`的作用是在import导入后不会自动运行
- @property将函数变为属性描述符;@name.setter可以设置动态属性
```python
from datetime import date, datetime


class User:
    def __init__(self, name, birthday) -> None:
        self.name = name
        self.birthday = birthday
        # self.age = 0

    # def get_age(self):
    #     return datetime.now().year - self.birthday.year

    @property
    def age(self):
        return datetime.now().year - self.birthday.year


if __name__ == "__main__":
    user = User("bobby", date(year=1999, month=8, day=1))
    print(user.age) # 23
```
### __getattr__、__getattribute__魔法函数
- \_\_getattr\_\_()在查找不到属性时调用
```python
class User:
    def __init__(self, name) -> None:
        self.name = name

    def __getattr__(self, item):
        return self.name + ": not found."
        
if __name__ == "__main__":
    user = User("Sancho")
    print(user.age)  # Sancho :not found.

```
- \_\_getattribute\_\_()查找属性时调用
```python
class User:
    def __init__(self, name) -> None:
        self.name = name

    def __getattribute__(self, __name: str):
        return "Sancho"


if __name__ == "__main__":
    user = User("Sancho")
    print(user.age)  # Sancho
    print(user.name) # Sancho
```
### 属性描述符和属性查找过程
- 属性描述符可以重复利用一个类参数类型
```
import numbers


class IntField:
    # 属性描述符
    def __get__(self, instance, owner):
        return self.value

    def __set__(self, instance, value):
        if not isinstance(value, numbers.Integral):
            raise ValueError("int value need!")
        self.value = value

    # def __delite__(self,instance):
    #     pass

class NonDataInField:
    # 非属性描述符
    def __get__(self,instance,owner):
        return self.value


class User:
    age = IntField()


if __name__ == "__main__":
    user = User()
    user.age = 23
    print(user.age) # 23
    user.age = "a" # ValueError: int value need!
```
- 属性查找顺序
  1. 如果属性出现在类或其基类的__dict__()中，且属性是data descriptor（数据描述符），那么则调用其__get__()方法
  2.如果属性出现在obj的的__dict__中，那么直接返回obj.__dict__['属性']
  3. 如果属性出现在类或其基类的__dict__中
  3.1 如果age是non-data descriptor（），那么调用其__get__方法
  3.2 返回__dict__['属性']
  4. 如果obj有__getattr__方法，调用__getattr__()方法
  5. 抛出AttributeError
### __new__和__init__的区别
- \_\_new\_\_()控制对象的生成过程，在生成过程之前
  - \_\_new\_\_()不返回对象，则不会调用\_\_init\_\_()函数
- \_\_init\_\_()控制参数，完善对象，在对象生成之后
### 自定义元类
- type动态创建类
```python
def say(self):
    return "hi hi hi"


class BaseClass:
    def answer(self):
        return "HI HI HI"


if __name__ == "__main__":
    User = type("User2", (BaseClass, ), {"name": "Sancho", "say": say})
    user = User()
    print(user.name)  # Sancho
    print(user.say())  # hi hi hi
    print(user.answer())  # HI HI HI
```
- 当使用class定义类的时候，首先查找MetaClass，通过MetaClass再创建类
```python
class MetaClass(type):
    def __new__(cls, *args, **kwargs):
        print("this is MetaClass.__new__()")
        return super().__new__(cls, *args, **kwargs)


class User(metaclass=MetaClass):
    def __init__(self, name):
        print("this is User.__init__()")
        self.name = name

    def __str__(self):
        print("this is User.__str__()")
        return "user"


if __name__ == "__main__":
    user = User(name="Sancho")
    print(user)

# 输出：
# this is MetaClass.__new__()
# this is User.__init__()
# this is User.__str__()
# user
```
### !orm
## 迭代器生成器
### 迭代协议
- 迭代器是访问集合内元素的一种方式，一般用来遍历数据
- 迭代器(\_\_iter\_\_)和以下标(\_\_getiter\_\_)的访问方式不一样，采用惰性方式
- 可迭代对象提供\_\_iter\_\_()魔法函数；迭代器提供\_\_iter\_\_()和\_\_next\_\_()两个魔法函数
### 迭代器和可迭代对象
- for循环优先寻找\_\_iter\_\_()，如果不存在则寻找\_\_getier\_\_()后开始遍历
```python
# 迭代器实现
class Company(object):
    def __init__(self, employee_list) -> None:
        self.employee_list = employee_list

    def __getitem__(self, item):
        return self.employee_list[item]


if __name__ == "__main__":
    company = Company(["Sancho", "Cat", "Dog"])
    for item in company:
        print(item)

# 输出：
# Sancho
# Cat
# Dog
```
- iter()函数实现将可迭代对象添加\_\_next\_\_()魔法函数，返回迭代器
```python
# 迭代器具体实现
from collections.abc import Iterator


class Company(object):
    def __init__(self, employee_list):
        self.employee_list = employee_list

    def __iter__(self):
        return MyIterator(self.employee_list)


class MyIterator(Iterator):
    def __init__(self, employee_list) -> None:
        self.iter_list = employee_list
        self.index = 0

    def __next__(self):
        try:
            word = self.iter_list[self.index]
        except IndexError:
            raise StopIteration
        self.index += 1
        return word


if __name__ == "__main__":
    company = Company(["Sancho", "Cat", "Dog"])
    my_itor = iter(company)
    while True:
        try:
            print(next(my_itor))
        except StopIteration:
            break

# 输出：
# Sancho
# Cat
# Dog
```
### 生成器函数
- 函数里有yield关键字就可称为生成器，返回生成器对象；生成器也实现了迭代器协议（可迭代）
```python
def gen_func():
    yield 1
    yield 2 # 可继续返回值
    yield 3


if __name__ == "__main__":
    gen = gen_func()
    print([i for i in gen]) # [1, 2, 3]
```
```python
# 斐波拉契数列
def gen_fib(index):
    n, a, b = 0, 0, 1
    while n < index:
        yield b
        a, b = b, a + b
        n += 1


if __name__ == "__main__":
    print({i for i in gen_fib(10)})  # {1, 2, 3, 34, 5, 8, 13, 21, 55}
```
### 生成器原理
- Python创建对象时会用PyEval_EvalFramEx(C函数)去执行函数，首先创建一个栈帧对象(stack frame)，将函数代码编译成字节码对象；当调用子函数时又会创建一个栈帧对象，并申请控制权；所有栈帧都分配到堆内存上，这决定了栈帧可以独立于调用者
![image](https://user-images.githubusercontent.com/42240228/176946907-81870e03-6a9f-4b08-a4ce-edf7fa92a0b0.png)
```
# 栈帧的调用流程
import inspect

frame = None


def foo():
    bar()


def bar():
    global frame
    frame = inspect.currentframe()  # 获取当前栈帧，并赋值于全局变量


if __name__ == "__main__":
    foo()
    print(frame.f_code.co_name)  # bar; 输出之前栈帧
    caller_frame = frame.f_back  # 查看上一级栈帧
    print(caller_frame.f_code.co_name)  # foo； 输出上一级栈帧

```
- 生成器函数在创建栈帧时，记录最近执行的代码位置(f_lasti)和环境变量(locals)
![image](https://user-images.githubusercontent.com/42240228/176947405-207ee667-9adf-4f44-af49-d25310499073.png)
```
# 生成器的栈帧调用流程
def gen_func():
    yield 1
    name = "Sancho"
    yield 2
    age = 30
    return "Hello"


if __name__ == "__main__":
    gen = gen_func()
    print(gen.gi_frame.f_lasti)
    print(gen.gi_frame.f_locals)
    next(gen)
    print(gen.gi_frame.f_lasti)
    print(gen.gi_frame.f_locals)
    next(gen)
    print(gen.gi_frame.f_lasti)
    print(gen.gi_frame.f_locals)

# 输出：
# -1
# {}
# 2
# {}
# 12
# {'name': 'Sancho'}
```
### UserList中的生成器
- Python中list使用C语言实现因此查看不到源码，但Python内置了UserList类型方便用户继承；
- 在collections模块中的UserList继承链是：MutableSequence → Sequence；
- Sequence中的\_\_iter\_\_()魔法函数实现了生成器，在每次调用list时进行循环
![image](https://user-images.githubusercontent.com/42240228/176950685-4acbd0b1-6377-4c19-84ce-99305dfedccd.png)
### 生成器实例：大文件读取
```python
#!/usr/bin/env python
#-*- coding: utf-8 -*-
#FILE: read_files.py
#CREATE_TIME: 2022-07-02
#AUTHOR: Sancho


"""
功能：文件读取
需求：支持读取大量数据，数据只有一行（有分隔符）
"""


def my_read_lines(f, newline):
    buf = ""  # 缓存
    while True:
        while newline in buf:  # 在缓存中查找分隔符
            pos = buf.index(newline)
            yield buf[:pos]  # 找到读取到的文本到分隔符位置之间文本并返回
            buf = buf[pos + len(newline):] # 更新，上次位置往后取值
        chunk = f.read(4096)  # 每次读取的数量

        if not chunk:  # 已经读到文件结尾
            yield buf
            break
        buf += chunk # 读取下一批


if __name__ == "__main__":
    with open("input.tex") as f:
        for line in my_read_lines(f, "{|}"):
            print(line)
```
## socket编程
## 多线程、多进程、线程池
## 协程和异步IO
## 并发编程

