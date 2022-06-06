# JavaScript笔记
## JavaScript基本概念
1. JavaScript是一门用来与网页交互的脚本语言，浏览器对其有不同程度的支持[(查询)](https://caniuse.com/)
    - ECMAScipt：由ECMA-262定义并提供核心功能
    - 文档对象模型（DOM）：提供与网页内容交互的方法和接口
    - 浏览器对象模型（BOM）：提供与浏览器交互的方法和接口
2. JavaScript可通过<script>元素插入到HTML页面中，也可以引入外部js代码
    - 引入外部js代码需要将src熟悉设置为JS文件的URL（可以和网页同一服务器，也可以不同的域）
    - <script>代码会在网页中按顺序被执行，<script>内的代码也会顺序执行（defer和async属性除外）
    - 顺序执行页面会在执行<script>内容时阻塞，一般吧<script>标签放主内容之后</body>标签之前
    - defer属性可把脚本推迟到文档渲染完毕后执行（推迟脚本原则上按照它们被列出的次序执行）
    - async属性表示脚本不需要等待其它脚本，同时不阻塞文档渲染，但不能保证在页面中出现的次序
3. ECMAScript的基本元素和特性（需要示例）
    - 基本数据类型：Undefined、Null、Boolean、Number、String、Symbol
    - 只有Number一种数值数据类型（不区分整数和浮点型）
    - 严格模式中对容易出错的部分施加了限制
    - ECMAScript提供类C语言的基本操作符和流程控制语句
    - 函数不需要指定返回值，不指定返回值的函数返回undefined
----
## 变量、作用域、内存
### 原始值和引用值
  - 区别：原始值就是六种数据类型的的简单数据，按值访问；引用值是多个值构成的对象（地址值），按引用访问
  - 动态属性：引用值可以随时添加、修改和删除其属性和方法；原始值不能有属性
  - 复制值：通过变量把原始值赋值到另一个变量时，原始值复制到新变量的位置（互不干扰）；引用值赋值实则是复制指针指向同一个对象（共享）
  - 传递参数：所有函数的参数都是按值传递，值被复制到局部变量（arguments对象槽位,与变量赋值类似）；原始值不会改变，引用值会被共享
  ```javascript
  function setName(obj){
    obj.name = "Sancho"
    obj = new Object()//重写
    obj.name = "Greg"
  }
  
  let person = new Object()
  setName(person)
  console.log(person.name)//"Sancho"
  ```
  - 确定类型：
    - typeof操作符适合判断原始类型（String、Number、Boolean、Undefined）；判断null时返回object；
    - instanceof操作符判断对象类型（涉及对象原型链）；任何引用值和Object构造函数都会返回true，原始值则会返回false。`[1,2,3] instanceof Array//true`
### 执行上下文和作用域

### 垃圾回收

  
## 基本引用类型
## 集合引用类型
## 迭代器、生成器
## 对象、类、面向对象编程
## 代理、反射
## 函数
## 期约、异步
