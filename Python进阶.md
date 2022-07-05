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
```python
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
```python
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
### HTTP、Soket、TCP
- OSI五层网络模型（应用层、传输层、网络层、数据链路层、物理层）详见[计算机网络](计算机网络.md)
- HTTP、TCP属于网络协议，socket是一个api接口，与传输层(TCP)交互
### client和server实现通信
1. 服务端绑定地址、协议、端口(作用于具体应用)
2. 服务端实时监听来自客户端的请求
3. 服务端开放接收数据
4. 服务端等待客户端发起连接请求
5. 服务端接收来自客户端发送的数据
6. 服务端返回数据给客户端
7. 断开连接  
![image](https://user-images.githubusercontent.com/42240228/176974138-877f1d30-5ccd-4a16-a504-1009b31026f1.png)
- socket协议
  - AF_INET:IPv4；AF_INET6:IPv6；AF_IPX:进程间通信
  - SOCK_STREAM:TCP；SOC K_DGRAM:UDP
```python
#!/usr/bin/env python
#-*- coding: utf-8 -*-
#FILE: socket_server.py
#CREATE_TIME: 2022-07-02
#AUTHOR: Sancho
"""
服务端实现
"""

import socket

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind(('0.0.0.0', 8000))
server.listen()
sock, addr = server.accept()

# 获取从客户端发送的数据
data = sock.recv(1024)  # 设置最大获取字节
print(data.decode("utf8"))  # Sancho
sock.send("Hello {}".format(data.decode("utf-8")).encode("utf8"))
server.close()
sock.close()
```
```python
#!/usr/bin/env python
#-*- coding: utf-8 -*-
#FILE: socket_client.py
#CREATE_TIME: 2022-07-02
#AUTHOR: Sancho
"""
客户端实现
"""

import socket

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect(("127.0.0.1", 8000))
client.send("Sancho".encode("utf8"))
data = client.recv(1024)
print(data.decode("utf8"))  # Hello Sancho
client.close()
```
### socket实现聊天和多用户连接
```python
#!/usr/bin/env python
#-*- coding: utf-8 -*-
#FILE: socket_server.py
#CREATE_TIME: 2022-07-02
#AUTHOR: Sancho
"""
服务端实现
"""

import socket
import threading

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind(('0.0.0.0', 8000))
server.listen()


def handle_socket(sock, addr):
    while True:
        data = sock.recv(1024).decode("utf8")
        print(data)
        if data == "logout":
            break
        re_data = input()  # 手动输入数据交互
        sock.send(re_data.encode("utf8"))
    server.close()
    sock.close()


while True:  # 持续交流
    # 多用户连接
    sock, addr = server.accept()
    client_thread = threading.Thread(target=handle_socket, args=(sock, addr))
    client_thread.start()

    # data = sock.recv(1024)  # 设置最大获取字节
    # print(data.decode("utf8"))
    # re_data = input()
    # sock.send(re_data.encode("utf8"))
```
```python
#!/usr/bin/env python
#-*- coding: utf-8 -*-
#FILE: socket_client.py
#CREATE_TIME: 2022-07-02
#AUTHOR: Sancho
"""
客户端实现
"""

import socket

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect(("127.0.0.1", 8000))

while True:
    re_data = input()
    client.send(re_data.encode("utf8"))
    if re_data == "logout":
        client.close()
        break
    data = client.recv(1024).decode("utf8")
    print(data)  # Hello Sancho

```
### socket模拟http请求
```python
#!/usr/bin/env python
#-*- coding: utf-8 -*-
#FILE: socket_http.py
#CREATE_TIME: 2022-07-02
#AUTHOR: Sancho

import socket
from urllib.parse import urlparse


def get_url(url):
    # url解析
    url = urlparse(url)
    host = url.netloc  # 主域名解析
    path = url.path
    if path == "":
        path = "/"

    # 建立连接
    client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    client.connect((host, 80))  # 默认80端口
    # 发送HTTP请求
    client.send(
        "GET {} HTTP/1.1\r\nHost:{}\r\nConnection:close\r\n\r\n".format(
            path, host).encode("utf-8"))

    # 接收数据
    data = b""
    while True:
        c = client.recv(1024)  # 缓存
        # 超出最大字节处理
        if c:
            data += c
        else:
            break

    data = data.decode("utf8").split("\r\n\r\n")  # 去除HTTP头部信息
    print(data)
    client.close()


if __name__ == "__main__":
    get_url("http://www.baidu.com")

```
## 多线程、多进程、线程池
### GIL(global interpreter lock)
- Python的线程对应C语言中的一个线程
- GIL使得同一时刻只有一个线程在一个cpu上执行字节码，无法将多个线程映射到多个CPU上执行
- GIL会根据执行的字节码行数以及时间片释放，遇到IO操作时主动释放
### 多线程
- 对于IO操作，多线程和多进程性能差别不大（甚至优于）
```python
#!/usr/bin/env python
#-*- coding: utf-8 -*-
#FILE: py_thread.py
#CREATE_TIME: 2022-07-02
#AUTHOR: Sancho
"""
模拟网络请求
"""

import time
import threading


def get_detail_html(url):
    print("get detail html started")
    time.sleep(2)
    print("get detail html end")


def get_detail_url(url):
    print("get detail url started")
    time.sleep(4)
    print("get detail url end")


if __name__ == "__main__":
    thread1 = threading.Thread(target=get_detail_html, args=("", ))
    thread2 = threading.Thread(target=get_detail_url, args=("", ))

    # 线程守护（主线程退出时子线程一并退出）
    # thread2.setDaemon(True)
    # thread2.setDaemon(True)

    start_time = time.time()
    thread1.start()
    thread2.start()

    # 线程阻塞（当线程运行结束后再运行之后代码）
    thread1.join()
    thread2.join()

    print("last time: {}".format(time.time() - start_time))
```
```python
#!/usr/bin/env python
#-*- coding: utf-8 -*-
#FILE: py_thread2.py
#CREATE_TIME: 2022-07-02
#AUTHOR: Sancho
"""
继承多线程，模拟网络请求
"""

import time
import threading


class GetDetailHtml(threading.Thread):
    def __init__(self, name) -> None:
        super().__init__(name=name)  # 传递线程名

    def run(self):
        print("get detail html started")
        time.sleep(2)
        print("get detail html end")


class GetDetailUrl(threading.Thread):
    def __init__(self, name) -> None:
        super().__init__(name=name)

    def run(self):
        print("get detail url started")
        time.sleep(4)
        print("get detail url end")


if __name__ == "__main__":
    thread1 = GetDetailHtml("get_detail_html")
    thread2 = GetDetailUrl("get_detail_url")

    start_time = time.time()
    thread1.start()
    thread2.start()

    # 线程阻塞（当线程运行结束后再运行之后代码）
    thread1.join()
    thread2.join()

    print("last time: {}".format(time.time() - start_time))
```
### 线程间通信(消息队列、Queue)
- 线程间通信时变量（参数传递、全局变量、外部文件变量等方式）并不可靠（安全性不高）
- Queue通过消息队列方式实现，可以控制多线程间通信顺序(阻塞)
```python 
#!/usr/bin/env python
# -*- coding:utf-8 -*-
import time
import queue
import threading


def worker(i):
    while True:
        item = q.get()
        if item is None:
            print("线程%s发现了一个None,可以休息了^-^" % i)
            break
        # do_work(item)做具体的工作
        time.sleep(0.5)
        print("线程%s将任务<%s>完成了！" % (i, item))
        # 做完后发出任务完成信号，然后继续下一个任务
        q.task_done()


if __name__ == '__main__':
    num_of_threads = 5

    source = [i for i in range(1, 21)]  # 模拟20个任务

    # 创建一个FIFO队列对象，不设置上限
    q = queue.Queue()
    # 创建一个线程池
    threads = []
    # 创建指定个数的工作线程，并讲他们放到线程池threads中
    for i in range(1, num_of_threads+1):
        t = threading.Thread(target=worker, args=(i,))
        threads.append(t)
        t.start()

    # 将任务源里的任务逐个放入队列
    for item in source:
        time.sleep(0.5)     # 每隔0.5秒发布一个新任务
        q.put(item)

    # 阻塞队列直到队列里的任务都完成了
    q.join()
    print("-----工作都完成了-----")
    # 停止工作线程
    for i in range(num_of_threads):
        q.put(None)
    for t in threads:
        t.join()
    print(threads)
```
### 线程同步(Lock、RLock、semaphores、Condition）
- 字节码处理读写时可能遇到多个线程同时对变量操作导致与预期结果不一致
- Lock锁控制字节码单元占用和释放交替运行（会影响运行性能）,可能会引起死锁（连续acquire[包括子函数acquire]、没有release、互相等待等）
- RLock，可重入的锁，同一个线程可连续调用多次，需要acquire和release同等数量的调用
```python
import threading
from threading import Lock

total = 0
lock = Lock()


def add():
    global total
    global lock

    for i in range(100):
        lock.acquire() # 占用
        total += 1
        lock.release() # 释放


def desc():
    global total
    global lock

    for i in range(100):
        lock.acquire()
        total -= 1
        lock.release()


thread1 = threading.Thread(target=add)
thread2 = threading.Thread(target=desc)
print(total) # 0
```
- Condition，条件变量，用户复杂的线程间同步（切换）
  - condition有两层锁，一个底层的锁会在线程调用wait方法时释放，另一个上层锁会在每次调用wait时分配一把锁并放入到cond等待队列，等待notify方法唤醒
```python
#!/usr/bin/env python
#-*- coding: utf-8 -*-
#FILE: thread_condition.py
#CREATE_TIME: 2022-07-03
#AUTHOR: Sancho
"""
两线程对诗
解决两个线程之间切换、同步的问题
"""

import threading


class Xiaoai(threading.Thread):
    def __init__(self, cond):
        super().__init__(name="小爱同学")
        self.cond = cond

    def run(self):
        with self.cond:  # 等于调用enter和exit两个魔法函数，实现cond.acquire和cond.release
            self.cond.wait()  # 等待唤醒
            print("{name}: 在".format(name=self.name))
            self.cond.notify()  # 释放

            self.cond.wait()  # 等待唤醒
            print("{name}: 奔流到海不复回".format(name=self.name))
            self.cond.notify()  # 释放

            self.cond.wait()  # 等待唤醒
            print("{name}: 朝如青丝暮成雪".format(name=self.name))
            self.cond.notify()  # 释放


class Tianmao(threading.Thread):
    def __init__(self, cond):
        super().__init__(name="天猫精灵")
        self.cond = cond

    def run(self):
        with self.cond:  # 等于调用enter和exit两个魔法函数，实现cond.acquire和cond.release
            print("{name}: 小爱同学".format(name=self.name))
            self.cond.notify()  # 释放
            self.cond.wait()  # 等待唤醒

            print("{name}: 君不见，黄河之水天上来".format(name=self.name))
            self.cond.notify()  # 释放
            self.cond.wait()  # 等待唤醒

            print("{name}: 君不见，高堂明镜悲白发".format(name=self.name))
            self.cond.notify()  # 释放
            self.cond.wait()  # 等待唤醒


if __name__ == "__main__":
    cond = threading.Condition()
    xiaoai = Xiaoai(cond)
    tianmao = Tianmao(cond)

    # 一定要先wait的的程序先执行，否则获取不到信号
    xiaoai.start()
    tianmao.start()

# 输出：
# 天猫精灵: 小爱同学
# 小爱同学: 在
# 天猫精灵: 君不见，黄河之水天上来
# 小爱同学: 奔流到海不复回
# 天猫精灵: 君不见，高堂明镜悲白发
# 小爱同学: 朝如青丝暮成雪
```
- Semaphore，用于控制进入数量的锁
```python
#!/usr/bin/env python
#-*- coding: utf-8 -*-
#FILE: thread_semaphore.py
#CREATE_TIME: 2022-07-03
#AUTHOR: Sancho
"""
模拟爬虫并发操作
文件读取时允许多个线程，但写入时一般不允许多个线程同时写入
"""

import threading
import time


class HtmlSpider(threading.Thread):
    """ 模拟爬取网页 """
    def __init__(self, url, sem):
        super().__init__()
        self.url = url
        self.sem = sem

    def run(self):
        time.sleep(2)
        print("got html text success")
        self.sem.release()  # 释放线程，且每次调用可并发数+1


class UrlProducer(threading.Thread):
    def __init__(self, sem):
        super().__init__()
        self.sem = sem

    def run(self):
        for i in range(20):  # 创建里线程
            self.sem.acquire()  # 占用，且每次调用可并发数-1
            html_thread = HtmlSpider("https://baidu.com/id:{}".format(i),
                                     self.sem)
            html_thread.start()
            # self.sem.release() 不要在此处释放，应在线程工作结束时，否则不能控制并发


if __name__ == "__main__":
    sem = threading.Semaphore(3)  # 并发数
    url_producer = UrlProducer(sem)
    url_producer.start()
```
### 线程池(concurrent)
- 线程池作用：主线程中可以获取某一个线程的状态或者某一个任务的状态以及返回值；当一个线程完成的时候我们主线程立刻知道
- concurrent.futures可以让多线程和多进程编码接口一致
  - 
```python
#!/usr/bin/env python
#-*- coding: utf-8 -*-
#FILE: concurrent_futures.py
#CREATE_TIME: 2022-07-03
#AUTHOR: Sancho
"""
模拟爬虫线程池
"""

import time
from concurrent.futures import ThreadPoolExecutor, as_completed, wait


def get_html(times):
    time.sleep(times)
    print("get page {}".format(times))
    return times


executor = ThreadPoolExecutor(max_workers=2)  # 最大线程数

urls = [3, 2, 4]  # 模拟传入的链接

# 立即返回法
all_task = [executor.submit(get_html, (url)) for url in urls]  # 提交任务到线程池
for future in as_completed(all_task):
    # 阻塞，获取已完成的任务
    data = future.result()
    print("get {} page success".format(data))
# wait(all_task) # 阻塞等待完成，returen_when参数设置等待模式；此处非必要

# 顺序返回法
# for data in executor.map(get_html,urls): # 依次传递参数2到参数1的函数中
#     print("get {} page success".format(data))
```
### 多进程编程(multiprocessing)
- Python中有GIL锁机制，在多线程中无法发挥CPU多核优势，所以在需要耗CPU的计算类操作时使用多进程；读写时多线程影响小，但多进程会存在进程切换代价，因此IO操作时更需要多线程
```python
#!/usr/bin/env python
#-*- coding: utf-8 -*-
#FILE: progress_test.py
#CREATE_TIME: 2022-07-03
#AUTHOR: Sancho
"""
多线程和多进程的速度对比测试
"""

import time
from concurrent.futures import ThreadPoolExecutor, as_completed
from concurrent.futures import ProcessPoolExecutor


def fib(n):
    if n < 2:
        return 1
    return fib(n - 1) + fib(n - 1)


def random_sleep(n):
    time.sleep(n)
    return n


if __name__ == "__main__":
    # 计算测试
    # with ThreadPoolExecutor(3) as executor:
    #     all_tasks = [executor.submit(fib, (num)) for num in range(25, 30)]
    #     start_time = time.time()
    #     for future in as_completed(all_tasks):
    #         data = future.result()
    #         print("exe result: {}".format(data))

    #     print("Thread last time is: {}".format(time.time() - start_time)) # Thread last time is: 108.5627212524414

    # with ProcessPoolExecutor(3) as executor:
    #     all_tasks = [executor.submit(fib, (num)) for num in range(25, 30)]
    #     start_time = time.time()
    #     for future in as_completed(all_tasks):
    #         data = future.result()
    #         print("exe result: {}".format(data))

    #     print("Process last time is: {}".format(time.time() - start_time)) # Process last time is: 65.22123789787292

    # IO模拟测试
    with ThreadPoolExecutor(3) as executor:
        all_tasks = [executor.submit(random_sleep, (num)) for num in [2] * 30]
        start_time = time.time()
        for future in as_completed(all_tasks):
            data = future.result()
            print("exe result: {}".format(data))

        print("Thread last time is: {}".format(
            time.time() -
            start_time))  # Thread last time is: 20.08672332763672

    with ProcessPoolExecutor(3) as executor:
        all_tasks = [executor.submit(random_sleep, (num)) for num in [2] * 30]
        start_time = time.time()
        for future in as_completed(all_tasks):
            data = future.result()
            print("exe result: {}".format(data))

        print("Thread last time is: {}".format(
            time.time() -
            start_time))  # Thread last time is: 20.204091787338257
```
- multiprocessing是比concurrent更底层的包，更加灵活
```python
#!/usr/bin/env python
#-*- coding: utf-8 -*-
#FILE: multiprocessing.py
#CREATE_TIME: 2022-07-03
#AUTHOR: Sancho
"""
模拟爬虫多进程
"""

import time
import multiprocessing


def get_html(n):
    time.sleep(n)
    print("sub_progress success")
    return n


if __name__ == "__main__":
    pool = multiprocessing.Pool()  # 默认最大CPU核数
    result = pool.apply_async(get_html, (2, ))  # 异步提交任务

    pool.close()  # 在join之前必须调用close关闭接收任务
    pool.join()  # 阻塞等待所有任务完成
    print(result.get())  # 获取返回值

    # imap,顺序返回
    # pool2 = multiprocessing.Pool()
    # for result in pool2.imap(get_html, [1, 5, 3]):
    #     print("{} sleep success".format(result))

    # imap_unordered,立即返回
    # pool3 = multiprocessing.Pool()
    # for result in pool3.imap_unordered(get_html, [1, 5, 3]):
    #     print("{} sleep success".format(result))
```
### 进程间通信
- 多进程之间一般不能像多线程之间一样通信
- 共享变量在多进程中不适用，Queue模块不适用多进程之间通信
- 多进程通信可使用multiprocessing.Queue，但不适用进程池；进程池中通信可使用multiprocessing.Mannager().Queue
  - multiprocessing.Mannager还可以维护公共变量，如`p_dict = multiprocessing.Mannager().dict()`
- 两个进程通信还可以使用multiprocessing.pipe()相当于简化版的Queue
## 协程和异步IO
### 并发、并行、同步、异步、阻塞、非阻塞
- 并发指一个时间段内，有几个程序在同一个CPU上运行，但是任意时刻只有一个程序在CPU上运行（进程切换）
- 并行是指任意时刻点上，有多个程序同时运行在多个CPU上（并行数与核心数一致）
- 同步是指代码调用IO操作时，必须等待IO操作完成才返回的调用方式
- 异步是指代码调用IO操作时，不必等IO操作完成就返回的调用方式
- 阻塞是指调用函数的时候当前线程被挂起
- 非阻塞是指调用函数的时候当前线程不会被挂起，而是立即返回
### C10K问题和IO多路复用（select、poll、epoll）
- C10K问题(1999年)：如何在一颗1GHz CPU，2G内存，1Gbps网络环境下，让单台服务器同时为一万个客户端提供FTP服务
- Unix下五种I/O模型：阻塞式I/O，非阻塞式I/O，I/O复用，信号驱动式I/O，异步I/O  
![image](https://user-images.githubusercontent.com/42240228/177023415-5ce14ca3-f024-486a-840d-10aacd417b8a.png)  
![image](https://user-images.githubusercontent.com/42240228/177023426-bb76e5d4-5222-4577-8c39-fedf52550f20.png)  
![image](https://user-images.githubusercontent.com/42240228/177023555-a6a2ad5f-75f4-4b74-aaf3-b56d34499844.png)  
![image](https://user-images.githubusercontent.com/42240228/177023583-620c35cf-962d-45b6-a36c-4657367323da.png)  
![image](https://user-images.githubusercontent.com/42240228/177023652-84debb2e-681c-4eb2-95e3-ca21da6021c4.png)  
- select,poll,epoll都是IO多路复用机制。I/O多路复用就是通过一种机制，一个进程可以监视多个描述符，一旦某个描述符就绪（一般就是读就绪或者写就绪），能够通知程序进行相应的读写操作。但select，poll，epoll本质上都是同步I/O，因为他们都需要在读写事件就绪后自己负责进行读写，也就是说这个读写过程是阻塞的，而异步I/O则无需自己负责进行读写，异步I/O的实现会负责把数据从内核拷贝到用户空间
- select函数监视的文件描述符分3类：writefds、readfds、exceptfds，调用后select函数会阻塞，直到有描述符就绪（有数据可读、可写或者except），或者超时（timeout指定等待时间，设为null立即返回），函数返回后，可以通过遍历fdset来找到就绪的描述符
  - select有良好的跨平台性，缺点是单个进程能够监视文件描述符的数量存在最大限制，在Linux上一般为1024，可通过修改宏定义或重新编译内核的方式提升，但也会造成效率降低
- poll不同于select使用三个位图表示三个fdset的方式，poll使用pollfd指针实现；pollfd结构包含要监视的event和发生的event，不再使用select"参数-值"传递的方式，同时pollfd并没有最大数量限制（但数量过大后性能也会下降），和select函数一样，poll返回后需要轮询pollfd来获取就绪的描述符
  - select和poll都需要在返回后，通过遍历文件描述符来获取已经就绪的socket，事实上同时连接的大量客户端在一时刻可能只有很少的处于就绪状态，因此随着监视器的描述符数量增长，其效率也会线性下降
- epoll(不支持windows)，相对select和poll来说更加灵活，没有描述符限制。epoll使用一个文件描述符管理多个描述符，将用户关系的文件描述符的事件存放到内核的一个事件表中，这样在用户空间和内核空间的copy只需一次
- 高并发、连接活跃度相对不高（网页），epoll比select好；并发性不高，连接活跃度高（游戏），select比epoll好
### epoll+回调+事件循环模式
- 非阻塞IO和select对比
```python
#!/usr/bin/env python
#-*- coding: utf-8 -*-
#FILE: socket_http_nonblocking.py
#CREATE_TIME: 2022-07-03
#AUTHOR: Sancho
"""
通过非阻塞io实现http请求
"""

import socket
from urllib.parse import urlparse


def get_url(url):
    # url解析
    url = urlparse(url)
    host = url.netloc  # 主域名解析
    path = url.path
    if path == "":
        path = "/"

    # 建立连接
    client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    client.setblocking(False)  # 非阻塞
    try:  # 因为定义了非阻塞，程序会继续运行可能请求失败，所以需要拦截报错
        client.connect((host, 80))  # 默认80端口
    except BlockingIOError as e:
        pass
    while True:
        # 发送HTTP请求
        # 因为非阻塞，所以需要循环判断是否连接成功再发送
        try:
            client.send(
                "GET {} HTTP/1.1\r\nHost:{}\r\nConnection:close\r\n\r\n".
                format(path, host).encode("utf-8"))
            break
        except OSError as e:
            pass

    # 接收数据
    data = b""
    while True:
        # 循环判断是否接收到数据
        try:
            c = client.recv(1024)  # 缓存
        except BlockingIOError as e:
            continue

        # 超出最大字节处理
        if c:
            data += c
        else:
            break

    data = data.decode("utf8").split("\r\n\r\n")  # 去除HTTP头部信息
    print(data)
    client.close()


if __name__ == "__main__":
    import time
    stime = time.time()
    for i in range(20):
        get_url("http://www.baidu.com")
    print(time.time() - stime)  # 1.9488499164581299

```
```python
#!/usr/bin/env python
#-*- coding: utf-8 -*-
#FILE: socket_http_select.py
#CREATE_TIME: 2022-07-03
#AUTHOR: Sancho
"""
通过select实现http请求
"""

import socket
from selectors import DefaultSelector, EVENT_READ, EVENT_WRITE
from tracemalloc import start
from urllib.parse import urlparse

selector = DefaultSelector()
urls = []
stop = False


class Fetcher:
    def connected(self, key):
        """ 连接成功后 """
        selector.unregister(key.fd)  # 注销监控事件
        self.client.send(
            "GET {} HTTP/1.1\r\nHost:{}\r\nConnection:close\r\n\r\n".format(
                self.path, self.host).encode("utf-8"))
        selector.register(self.client.fileno(), EVENT_READ,
                          self.readable)  # 注册可读监听事件

    def readable(self, key):
        """ 可读状态时 """
        c = self.client.recv(1024)  # 缓存返回的数据
        if c:
            self.data += c
        else:
            # 数据读取完时
            selector.unregister(key.fd)  # 注销监听事件
            html_data = self.data.decode("utf8").split(
                "\r\n\r\n")  # 去除HTTP头部信息
            print(html_data)
            self.client.close()
            urls.remove(self.spider_url)
            if not urls:
                global stop
                stop = True

    def get_url(self, url):
        self.spider_url = url
        url = urlparse(url)
        self.host = url.netloc
        self.path = url.path
        self.data = b""
        if self.path == "":
            self.path = "/"

        # 建立连接
        self.client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self.client.setblocking(False)

        # 等待连接成功
        try:
            self.client.connect((self.host, 80))
        except BlockingIOError as e:
            pass

        # 注册（文件描述符，事件，回调函数）
        selector.register(self.client.fileno(), EVENT_WRITE,
                          self.connected)  # 监控client可写时调用函数


def loop():
    """事件循环：不停地请求socket状态并调用对应回调函数"""
    while not stop:
        ready = selector.select()
        for key, mask in ready:
            call_back = key.data
            call_back(key)


if __name__ == "__main__":
    # fetcher = Fetcher()
    import time
    stime = time.time()
    for url in range(20):
        url = "http://www.baidu.com"
        urls.append(url)
        fetcher = Fetcher()
        fetcher.get_url(url)
    loop()
    print(time.time() - stime)  # 0.09574317932128906
```
### 回调问题
- 可读性差
- 共享状态管理困难
- 异常状态处理困难
### C10M问题和协程
- C10M问题：如何利用8核心CPU，64G内存，在10Gbps的网络上保持1000万并发连接
- 解决方式：1.采用同步的方式编写异步的代码；2.使用单线程切换任务
  1. 单线程切换意味着调度任务需要我们自己操作，而不再是操作系统
  2. 不再需要锁，并发性高。如果单线程内切换函数，性能远高于线程切换，并发性更高
- 协程概念：有多个入口的函数，或称可以暂停的函数（可以向暂停的地方传入值）
### 生成器进阶send、close、thow
- 生成器不仅可以输出值，也可以接收值(send)；通过send可以传递值且运行生成器
- 生成器可以关闭(close)
```python
#!/usr/bin/env python
#-*- coding: utf-8 -*-
#FILE: gen_send.py
#CREATE_TIME: 2022-07-03
#AUTHOR: Sancho


def foo():
    print("starting...")
    while True:
        try:
            res = yield 4
        except Exception as e:
            pass
        print("res:", res)


g = foo()
print(next(g))  # 在send()传递值之前，必须next(gen)或gen.send(None)
print("*" * 20)
print(g.send("sancho"))

g.throw(Exception, "download error")  # 抛出异常
print(g.send("sancho"))

g.close()  # 关闭生成器

```
- yield from可以简单理解依次取值（其内部还有catch异常）；yield from会通过委托生成器，在调用方和子生成器之间建立双向通道
```python
#!/usr/bin/env python
#-*- coding: utf-8 -*-
#FILE: yield_from.py
#CREATE_TIME: 2022-07-04
#AUTHOR: Sancho

from itertools import chain

my_list = [1, 2, 3]
my_dict = {"Robin": "baidu.com", "Jack": "taobao"}


def my_chain(*args, **kwargs):
    for my_iterable in args:
        yield from my_iterable  # 替代下面循环
        # for value in my_iterable:
        #     yield value


# 方法1：
# for value in my_chain(my_list, my_dict, range(10, 16)):
#     print(value)

# 方法2：
for value in chain(my_list, my_dict, range(4, 7)):
    print(value)

# 输出：
# 1
# 2
# 3
# Robin
# Jack
# 4
# 5
# 6
```
  - 子生成器生产的值，直接传回调用方；调用方通过.send()发送值直接传递子生成器（如果是None调用子生成器__next__()，如果非None调用子生成器.send()）
  - 子生成器退出时，最后return EXPR 会触发StopIteration（EXPR）异常
  - yield from表达式的值，是子生成器终止时，传递给StopIteration异常的第一个参数
  - 如果调用时出现StopIteration异常，委托生成器会恢复运行，同时其它异常会向上"冒泡"
  - 传入委托生成器的异常里，除了GeneratorExit之外，其它所有异常全部传递给子生成器的.throw()方法，如果调用.throw()时出现StopIteration异常，那么就恢复委托生成器的运行，其它异常全部向上"冒泡"
  - 如果在委托生成器上调用.close()或传入GenneratorExit异常，会调用子生成器的.close()方法，没有的话就不调用;如果在调用.close()时抛出了异常，那就向上"冒泡"，否则委托生成器就会抛出GenneratorExit异常
```python
#!/usr/bin/env python
#-*- coding: utf-8 -*-
#FILE: yield_from_example.py
#CREATE_TIME: 2022-07-04
#AUTHOR: Sancho

final_result = {}  # 用于接收最终结果


def sales_sum(pro_name):
    """ 子生成器 """
    total = 0 # 销量累计
    nums = [] # 整理销量数据
    while True:
        x = yield
        if not x:  #取值结束
            break
        total += x
        nums.append(x)
    return total, nums


def middle(key):
    """ 委托生成器：行统计 """
    while True:
        final_result[key] = yield from sales_sum(key)
        print(key + "销售统计完成！")


def main():
    """ 调用方 """
    data_sets = {
        "键盘销量": [500, 400, 300],
        "耳机销量": [300, 200, 500],
        "鼠标销量": [500, 600, 700]
    }
    for key, data_set in data_sets.items():
        print("start key:", key)
        m = middle(key)
        m.send(None)  # 激活生成器
        for value in data_set:
            m.send(value) # 向子生成器发送数据
        m.send(None) # 循环结束后告诉子生成器结束
    print("final_result:", final_result)


if __name__ == "__main__":
    main()

# 输出：
# start key: 键盘销量
# 键盘销量销售统计完成！
# start key: 耳机销量
# 耳机销量销售统计完成！
# start key: 鼠标销量
# 鼠标销量销售统计完成！
# final_result: {'键盘销量': (1200, [500, 400, 300]), '
# 耳机销量': (1000, [300, 200, 500]), '鼠标销量': (1800, [500, 600, 700])}
```
### async和await原生协程
- Python3.5中加入async、await两个关键字，区分生成器和协程使用中的歧义
```python
#!/usr/bin/env python
#-*- coding: utf-8 -*-
#FILE: async_await.py
#CREATE_TIME: 2022-07-05
#AUTHOR: Sancho


async def downloader(url):
    return {"done": url}


async def get_url(url):
    html = await downloader(url) # await相当于yield
    return html


if __name__ == "__main__":
    coro = get_url("http://www.baidu.com")
    try:
        coro.send(None)
    except StopIteration as e:
        print(e)
```
### 生成器如何变成协程
- 生成器有状态：GEN_CREATED、GEN_SUSPENDED、GEN_CLOSED
- 生成器可以返回值给调用方，调用方可以通过send返回值给子生成器
- 解决同步的方式编写异步代码，并在适当时暂停或启动函数
- 协程的调度依然是事件循环+协程模式，协程在单线程模式之中交由程序员调度
## 并发编程
### 事件循环
- asyncio包含各种特定系统实现的模块化事件循环；
    - 传输和协议抽象；
    - 对TCP、UDP、SSL、子进程、延时调用以及其它的具体支持；
    - 模仿futures模块但适用于事件循环使用的Future类
    - 基于yield from的协议和任务，可以用顺序的方式编写并发代码
    - 必须使用一个将产生阻塞IO的调用时，有接口可以把这个事件转移到线程池
```python
#!/usr/bin/env python
#-*- coding: utf-8 -*-
#FILE: aysncio_loop.py
#CREATE_TIME: 2022-07-05
#AUTHOR: Sancho
"""
asyncio使用示例
"""

import asyncio
import time
from functools import partial


async def get_html(url):
    print("start get: ", url)
    await asyncio.sleep(2)
    print("end get: ", url)


def callback(url, tasks):
    """
    结束时运行
    url:需要传递的参数,需要放在参数列表前面
    tasks:future参数,默认将task传递进来
    """

    print("done: ", url)


if __name__ == "__main__":
    stime = time.time()
    loop = asyncio.get_event_loop()
    # tasks = asyncio.ensure_future(get_html("http://www.baidu.com")) # 与下一行功能一致
    tasks = loop.create_task(get_html("http://www.baidu.com"))
    # tasks.add_done_callback(callback)
    tasks.add_done_callback(partial(callback,
                                    "http://www.baidu.com"))  # 结束时调用函数并传递参数
    loop.run_until_complete(tasks)
    print(time.time() - stime)  # 2.0029876232147217
```
### task取消和子协程调用
```python
#!/usr/bin/env python
#-*- coding: utf-8 -*-
#FILE: asyncio_coroutine.py
#CREATE_TIME: 2022-07-05
#AUTHOR: Sancho
"""
asyncio的取消
"""

import asyncio
import time


async def get_html(sleep_times):
    print("waiting")
    await asyncio.sleep(sleep_times)
    print("done after {}s".format(sleep_times))


if __name__ == "__main__":
    task1 = get_html(1)
    task2 = get_html(2)
    task3 = get_html(3)
    tasks = [task1, task2, task3]
    loop = asyncio.get_event_loop()

    try:
        loop.run_until_complete(asyncio.wait(tasks))
    except KeyboardInterrupt as e:
        all_tasks = asyncio.Task.all_tasks()  # 获取全部tasks
        for task in all_tasks:
            print("cancel task")
            print(task.cancel())  # 取消task并返回取消结果
        loop.stop()  # 停止事件循环
        loop.run_forever()  # 必须再次启动
    finally:
        loop.close()  # 关闭事件循环
```
```python
import asyncio
""" 子协程调用 """


async def compute(x, y):
    print("Compute %s + %s ..." % (x, y))
    await asyncio.sleep(1.0)
    return x + y


async def print_sum(x, y):
    result = await compute(x, y)
    print("%s + %s = %s" % (x, y, result))


loop = asyncio.get_event_loop()
loop.run_until_complete(print_sum(1, 2))
loop.close()
```
![image](https://user-images.githubusercontent.com/42240228/177306667-8fe52865-40f9-45b3-8ae5-72eae2d99d21.png)
### call_soon、call_later、call_at、call_soon_threadsafe
- loop.call_soon()立即执行（下一轮事件循环）
- loop.call_later()指定延迟执行
- loop.call_at()指定事件循环的时间执行
- loop.call_soon_threadsafe()线程安全执行
### ThreadPoolExector + asyncio
```python
#!/usr/bin/env python
#-*- coding: utf-8 -*-
#FILE: thread_asyncio.py
#CREATE_TIME: 2022-07-05
#AUTHOR: Sancho
"""
多线程和异步
"""
import time
import asyncio
import socket
from urllib.parse import urlparse
from concurrent.futures import ThreadPoolExecutor


def get_url(url):
    # url解析
    url = urlparse(url)
    host = url.netloc  # 主域名解析
    path = url.path
    if path == "":
        path = "/"

    # 建立连接
    client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    client.connect((host, 80))  # 默认80端口
    # 发送HTTP请求
    client.send(
        "GET {} HTTP/1.1\r\nHost:{}\r\nConnection:close\r\n\r\n".format(
            path, host).encode("utf-8"))

    # 接收数据
    data = b""
    while True:
        c = client.recv(1024)  # 缓存
        # 超出最大字节处理
        if c:
            data += c
        else:
            break

    data = data.decode("utf8").split("\r\n\r\n")[1]  # 去除HTTP头部信息
    # print(data)
    client.close()


if __name__ == "__main__":
    stime = time.time()
    loop = asyncio.get_event_loop()
    executor = ThreadPoolExecutor()
    tasks = []
    for i in range(10):
        url = "http://www.baidu.com"
        task = loop.run_in_executor(executor, get_url, url)
        tasks.append(task)
    loop.run_until_complete(asyncio.wait(tasks))
    print(time.time() - stime)  # 0.19601988792419434
```
### asyncio模拟http请求
```python
#!/usr/bin/env python
#-*- coding: utf-8 -*-
#FILE: http_asyncio.py
#CREATE_TIME: 2022-07-05
#AUTHOR: Sancho
"""
ayncio实现http请求
"""
import time
import asyncio
import socket
from urllib.parse import urlparse


async def get_url(url):
    # url解析
    url = urlparse(url)
    host = url.netloc  # 主域名解析
    path = url.path
    if path == "":
        path = "/"

    # 建立连接
    # client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    # client.connect((host, 80))  # 默认80端口
    reader, writer = await asyncio.open_connection(host, 80)  # 协程建立连接（自动注册）
    writer.write(
        "GET {} HTTP/1.1\r\nHost:{}\r\nConnection:close\r\n\r\n".format(
            path, host).encode("utf-8"))  # 发送HTTP请求
    all_lines = []
    async for raw_line in reader:  # 异步读取
        data = raw_line.decode("utf8")  # 去除HTTP头部信息
        all_lines.append(data)
    html = "\n".join(all_lines)
    # print(html)
    return html


async def main():
    tasks = []
    for i in range(10):
        url = "http://www.baidu.com"
        tasks.append(asyncio.ensure_future(get_url(url)))
    for task in asyncio.as_completed(tasks): # 立即执行
        result = await task
        print(result)


if __name__ == "__main__":
    stime = time.time()
    loop = asyncio.get_event_loop()
    loop.run_until_complete(main())
    print(time.time() - stime)  # 0.09278202056884766
```
### future和task
### asyncio同步和通信
### aiohttp实现高并发爬虫

