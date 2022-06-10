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
  - 区别：原始值就是六种数据类型的的简单数据，按值访问，保存到栈；引用值是多个值构成的对象（地址值），按引用访问，保存到堆
  - 动态属性：引用值可以随时添加、修改和删除其属性和方法；原始值不能有属性
    ```javascript
    let name1 = "Sancho"
    let name2 = new String("Matt")
    name1.age = 22
    name2.age = 23
    console.log(name1.age)//undefined
    console.log(name2.age)//23
    console.log(typeof name1)//string
    console.log(typeof name2)//object
    ```
  - 复制值：通过变量把原始值赋值到另一个变量时，原始值复制到新变量的位置（互不干扰）；引用值赋值实则是复制指针指向同一个对象（共享）
    ```javascript
    let obj1 = new Object()
    let obj2 = obj1
    obj1.name = "Sancho"
    console.log(obg2.name)//"Sancho"
    ```
  - 传递参数：所有函数的参数都是按值传递，值被复制到局部变量（arguments对象槽位,与变量赋值类似）；原始值不会改变，引用值会被共享
      ```javascript
    //传递原始值
    function addTen(num){
        num += 10
        return num
    }
    let count = 20
    let result = addTen(count)
    console.log(count)//20,没有影响外部变量
    console.log(result)//30
    
    //传递引用值
    function setName(obj){
        obj.name = "Sancho"
    }
    let person = new Object()
    setName(person)
    console.log(person.name)//"Sancho"
    
    //证明函数参数是按值传递
    function setName(obj){
        obj.name = "Sancho"
        obj = new Object()//重写改变指针
        obj.name = "Greg"
    }
    let person = new Object()
    setName(person)
    console.log(person.name)//"Sancho"
    ```
  - typeof操作符适合判断原始类型（String、Number、Boolean、Undefined）；判断null时返回object；
    ```javascript
    let s = "Sancho"
    let u
    let n = null
    let o = new Object()
    console.log(s)//string
    console.log(u)//undefined
    console.log(n)//object
    console.log(o)//object
    ```
  - instanceof操作符判断对象类型（涉及对象原型链）；任何引用值和Object构造函数都会返回true，原始值则会返回false。
    ```javascript
    [1,2,3] instanceof Array//true
    ```
### 执行上下文（作用域）
  - 每个变量或函数都有上下文；变量或函数的上下文决定了它们可以访问哪些数据、行为和生命周期；
  - 代码执行流每进入一个新上下文都会创建一个作用域链，用于搜索变量和函数（沿作用域链前端开始，逐级往后直到找到标识符或没有找到返回undefined）
    ```javascript
    var color = "blue"
    function changeColor(){
        if (color === "blue"){
            color = "red"
        } else{
            color = "blue;"
        }
    }
    changeColor()//"red"

    var color2 = "blue"
    function getColor(){
        let color = "red"
        {
            let color = "green
            return color
        }
    }
    console.log(getColor())//green
    ```
  - 全局上下文只能访问全局上下文中的变量和函数，函数或块的局部上下文不仅可以访问自己作用域内和父级的变量
    ```javascript
    var color = "blue"
    
    function changeColor(){
        //可以访问color、anotherColor,不能访问tempColor
        let anotherColor = "red"
    
        function swapColors(){
            //可以访问color、anotherColor、tempColor
            let tempColor = anotherColor
            anotherColor = color
            color = tempColor
        }
    }
    
    //只能访问color
    changeColor()
    ```
  - 变量的执行上下文用于确定什么时候释放内存
    ```javascript
    //使用var声明变量时，变量会被自动添加到最接近的上下文
    functiion add(num1,num2){
        var sum1 = num1 + num2
        sum2 = num1 + num2//没有使用var定义
    }
    
    add(10,20)
    console.log(sum1)//报错，sum不在全局变量
    console.log(sum2)//30,sum2被添加到全局上下文
    
    //变量提升
    name = "Sancho"
    var name//等价于var name = "Sancho"
    
    //变量在声明前使用返回undefined
    console.log(name)//undefined
    var name = "Sancho"
    
    //函数提升
    function fn2(){
        var name
        name = "Sancho"
    }//等价于function fn1(){var name = "Sancho"}
    ```
  - 执行上下文分全局上下文、函数上下文、块级上下文(由花括号{}界定)
    ```javascript
    //let声明变量的作用域包括if块、while块、function块及其它单独块
    if(ture){
        let a
    }
    console.log(a)//a没有定义
    
    //重复var声明会被忽略，重复let声明会报错
    var a
    var a
    {
        let b
        let b//SyntaxError:标识符b已经声明过了
    }
    
    //const声明常量上下文也由花括号界定
    const a//SyntaxError：常量声明没有初始化
    const b = 1
    const b = 2//报错：给常量赋值
    {
        const c = 3
    }
    console.log(c)//报错，不存在于全局上下文
    
    const o1 = {}
    o1.name = "Sancho"//对象属性赋值不受影响
    const o2 = Object.freeze({})//让整个对象不能修改，不报错，但会静默失败
    o2.name = "Sancho"
    console.log(o3.name)//undefined
    ```
### 垃圾回收
  - 离开作用域的值被自动标记为可回收
  - 主流的垃圾回收算法是标记清理（不用的值标记之后清理内存）
  - 少用的是引用计数策略，记录值被引用多少次，荣一直在循环引用时出现问题
  - 解除变量引用（赋值null）可以消除循环引用，帮助垃圾回收
  - 内存管理：
    - const和let替代var；
    - 隐藏类（V8引擎自动跟踪、优化相同属性的多个实例共享一个构造函数和原型）和不使用删除（delete）操作隐藏类；
    - 使用变量声明前加定义和严格使用闭包防止内存泄露
    - 使用对象池，避免动态分配（引擎会删除大小为100的数组创建更大的数组，可以先定义好需要大小的数组），减少对象更替引起的垃圾回收程序敏感
    ```javascript
    //伪实现对象池
    function addVector(a,b,resultant){
        resultant.x = a.x + b.x
        resultant.y = a.y + b.y
        return resultant
    }
    
    let v1 = vectorPool.allocate()//假设vectorPool是已有的对象池
    let v2 = vectorPool.allocate()
    let v3 = vectorPool.allocate()
    v1.x = 10
    v1.y = 5
    v2.x = -3
    v2.y = -6
    
    addVector(v1,v2,v3)
    console.log([v3.x,v3.y])//[7,-1]
    
    vectorPool.free(v1)
    vectorPool.free(v2)
    vectorPool.free(v3)
    
    //如果对象有属性引用了其它对象，则这里也需要把属性设置为null
    v1 = null
    v2 = null
    v3 = null
    ```
----
## 基本引用类型
### 内置基本对象
  - 内置的引用类型可创建特定类型对象，与其他面向对象编程语言类似但实现不同
    ```javascript
    //创建对象
    let now = new Date()
    ```
  - Date类型提供日期和时间的信息（包括日期、时间相关计算）
    ```javascript
    //字符串转日期时间型
    let someDate = new Date(Date.parse("May 23, 2019"))
    let someDate = new Date(May 23, 2019")//等价于上面代码，自动调用parse方法
    
    //UTC方法返回毫秒数
    let y2k = new Date(Date.UTC(2000,0))//默认1月1日
    let allFives = new Date(Date.UTC(2000,1,2,3,4,5))//Wed Feb 02 2000 11:04:05 GMT+0800 (中国标准时间);月数以零开始，采用24小时制以零开始,中国时区+8小时
    let allFives = new Date(2000,1,2,3,4,5)//隐式调用
    
    //返回执行时间和毫秒速
    let start = Date.now()
    ｛
        //doSometing()
    ｝
    let stop = Date.now()
    result = stop - start
    
    ```
  - RegExp类型是正则表达式接口，提供基础和部分高级正则表达式功能
    ```javascript
    //创建正则表达式
    let pattern = /表达式/匹配模式 //匹配模式有:g全局，i忽略大小写，m多行模式，y粘附模式，u启用Unicode匹配，s匹配任何字符
    let parrern = /Sancho/i 
    let pattern = new RegExp("Sancho","i") //构建函数方式创建
    
    //搜寻文本，返回数组
    let text = "mom and dad and baby"
    let pattern = /baby/g
    let matches = pattern.exec(text) //['baby', index: 16, input: 'mom and dad and baby', groups: undefined]
    
    ```
### 原始值包装类型
  - JS中函数是Function类型的实例（函数也是对象，所以函数有方法）
  - 原始值有包装类存在，原始值可以被当成对象使用（Boolean、Number、String）
  - 每种包装类型都映射到同名原始类型
  - 以读模式访问原始值时，后台会实例化一个原始值包装类型的对象，借助这个对象可以操作相应数据
  - 涉及原始值的语句执行完毕后，包装对象就会被销毁
    ```javascript
    //以读模式访问，调用实例上的特定方法
    let s1 = "Sancho"
    let s2 = s1.substring(2)
    //上面代码实则执行如下代码
    let s1 = new String("Sancho")
    let s2 = s1.substring(2)
    s1 = null
    
    //不能给原始值添加属性和方法
    let s1 = "Sancho"
    s1.color = "red"
    console.log(s1.color)//undefined
    ```
### 单例内置对象
  - 代码执行时全局上下文有Global和Math对象，Global一般无法直接访问（实现为window），所有全局变量和函数都是Global对象的属性；Math对象包含辅助完成复杂计算的属性和方法
    ```javascript
    //URL编码、解码方法
    let uri = "http://www.wrox.com/illegal value.js#start"
    console.log(encodeURI(uri)) //http://www.wrox.com/illegal%20value.js#start
    console.log(encodeURIComponent(uri)) //http%3A%2F%2Fwww.wrox.com%2Fillegal%20value.js%23start
    let uri2 = "http%3A%2F%2Fwww.wrox.com%2Fillegal%20value.js%23start"
    console.log(decodeURI(uri2)) //http%3A%2F%2Fwww.wrox.com%2Fillegal value.js%23start
    console.log(decodeURIComponent(uri2)) //http://www.wrox.com/illegal value.js#start
    
    //执行字符串
    eval("console.log('hello world')") //通过eval执行字符串内的代码
    
    //window对象
    var color = "red"
    console.log(window.color)//"red"
    
    //Math对象
    let array = [1,2,3,4,5]
    console.log(Math.max(...array)) //5
    console.log(Math.min(...array)) //1
    
    ```
----
## 集合引用类型（与上重合）
### 自定义引用对象
  - Object类型是基础类型，所有引用类型都继承了它的基本行为
    ```javascript
    //构造函数创建
    let person = new Object()
    person.name = "Sancho"
    person.age = 23
    
    //字面量创建(推荐)
    let person = {
        "name":"Sancho",
        "age":23,
        5,true //属性名可以是字符串或数值，最后一个属性后不能加逗号
    }
    
    //调用属性
    console.log(person.name) //"Sancho"
    console.log(person["name"]) //"Sancho"
    let propertyName = "Name"
    console.log(person[propertyName]) //"Sancho",以变量方式调用
    ```
### 数组、定型数组
  - Array类型表示一组有序的值，并提供操作和转换值的能力
    ```javascript
    //创建数组
    let colors1 = new Array()
    let colors2 = new Array(20) //创建length为20的数组
    let colors3 = new Array("red","blue","green") //传入数据
    let colors4 = [] //字面量表示法
    let colors5 = [1,2,]
    
    //拆分数据
    console.log(Array.from("Sancho")) //拆分元素；(6字符['S', 'a', 'n', 'c', 'h', 'o']
    
    //迭代
    const iter = {
        *[Symbol.iterator](){
            yield 1
            yield 2
            yield 3
            yield 4
        }
    }
    console.log(Array.from(iter)) //(4) [1, 2, 3, 4]
    
    //合并数据
    console.log(Array.of(1,2,3,4)) //(4) [1, 2, 3, 4]
    
    //数组索引
    let colors = new Array("red","blue","green")
    console.log(colors[0]) //"red"
    console.log(colors.length) //3
    colors.length = 2 //修改长度
    console.log(colors[2]) //undefined
    colors.length = 4 //实则可任意修改，未填充为undefined
    console.log(colors[3]) //undefined
    colors[colors.length] = "black" //添加操作
    colors[-1] = "yellow" //倒叙索引
    
    //[!迭代器方法、复制和填充方法、转换方法、栈方法、队列方法、排序方法、操作方法、搜索和位置方法、迭代方法、归并方法]
    ```
  - 定型数组包含一套不同的引用类型，用于管理数值在内存中的类型
    ```javascript
    //预分配内存
    const buf = new ArrayBuffer(16) //内存中分配16字节，不可修改
    console.log(buf.byteLength) //16
    const buf2 = buf.slice(4,8) //截取复制
    console.log(buf2.byteLength) //8
    
    //DateView创建
    const buf = new ArrayBuffer(16)
    const fullDataView = new DataView(buf)
    console.log(fullDataview.byteOffset) //0
    console.log(fullDataview.byteLength) //16
    console.log(fullDataview.buffer === buf) //ture
    
    //DataView构造函数接收一个可选的字节偏移量和字节长度
    //  byteOffset=0 表示视图从缓冲起点开始
    //  byteLength=8 限制视图为前8个字节,不指定默认为剩余缓冲
    const firstHalfDataView = new DataView(buf, 0, 8)
    console.log(firstDataview.byteOffset) //0
    console.log(firstDataview.byteLength) //8
    console.log(firstDataview.buffer === buf) //ture
    
    //DataView读写
    const buf = new ArrayBuffer(2)
    const view = new DataView(buf)
    console.log(view.getInt(0)) //0；检查第一个字符
    console.log(view.getInt16(0)) //0；检查整个缓冲
    view.setUint8(0,255) //255的二进制表示是8个1，即将整个缓冲都设置为1
    
    //[!字节序、边界情形、定型数组（方法、合并、复制和修改定型数组、下溢和上溢）]
    ```
### Map、WeakMap、set、WeakSet
  - Map、WeakMap、Set、WeakSet是ECMAScript新增的引用类型，为组织应用程序数据和简化内存管理提供了新能力
    ```javascript
    //创建映射
    const m = new Map()
    //嵌套数组初始化映射
    const m1 = new Map([
        ["key1","val1"],
        ["key2","val2"],
        ["key3","val3"]
    ])
    
    //添加映射
    m.set("firstName","Sancho")
    console.log(m.has("firstName")) //true
    console.log(m.get("firstName")) //Sancho
    console.log(m.size) //1
    
    //清除映射
    m.clear()
    
    //迭代器
    const m1 = new Map([
        ["key1","val1"],
        ["key2","val2"],
        ["key3","val3"]
    ])
    console.log(m1.entries === m1[Symbol.iterator]) //true
    for (let pair of m1.entries()){console.log(pair)}
    //(2) ['key1', 'val1']
    //(2) ['key2', 'val2']
    //(2) ['key3', 'val3']
    
    //分别返回键值
    for (let key of m1.keys()){console.log(key)} //返回所有键
    for (let key of m1.values()){console.log(key)} //返回所有值
    
    //Object和Map：Map内存占用更少；Map插入性能稍快；Object查找速度更快；Map更适合大量删除操作；
    [!WeakMap、Set、WeakSet]
    ```
----
## 迭代器、生成器
### 迭代
  - 迭代器是一个可以由任意对象实现的接口，支持连续获取对象产出的每一个值。任何实现Iterable接口的对象都有一个Symol.iterator属性，这个属性引用默认迭代器。默认迭代器就像迭代器工厂，也就是一个函数，调用之后会产生一个实现Iterator接口的对象
    ```javascript
    //计数迭代数组（遍历）
    let collection = ["foo","bar","baz"]
    for (let i=0; i<collection.length; ++i){
        console.log(collection[index])
    }
    //方法迭代数组
    collecttion.forEach((item) => console.log(item))
    ```
### 迭代器模式
  - 迭代器需要连续调用next()方法才能连续取得值（for-of循环也可以），这个方法返回一个IteratorObject。这个对象包含一个done属性和一个value属性。
    - done属性是一个布尔值，表示是否还有更多值访问
    - value属性包含迭代器返回的当前值
  ```javascript
  //检查类型是否实现迭代器工厂函数
  let arr = ['a','b','c']
  console.log(arr[Symbol.iterator]) //ƒ values() { [native code] }
  //调用迭代器工厂函数生成迭代器
  let iter = arr[Symbol.iterator]()
  console.log(iter) //Array Iterator {}
  //执行迭代器
  console.log(iter.next()) //{value: 'a', done: false}
  console.log(iter.next()) //{value: 'b', done: false}
  console.log(iter.next()) //{value: 'c', done: false}
  console.log(iter.next()) //{value: undefined, done: true}
  ```
### 生成器
  - 生成器是一种特殊的函数，调用之后会返回一个生成器对象。生成器对象实现了Iterable接口，因此可用在任何消费可迭代对象的地方。生成器支持yield关键字，能够暂停执行生成器函数，还可通过next()方法接收输入和产生输出，加上星号之后可以将跟在它后面的可迭代对象序列化一连串值
    ```javascript
    //生成器函数声明
    function* generatorFnA(){}
    //生成器函数表达式
    let generatorFnB = function*(){}
    
    //开始或恢复执行
    let generatorFn = function*(){return 'foo'}
    const g = generatorFn()
    console.log(g) //generatorFnA {<suspended>}
    console.log(g.next()) //{value: 'foo', done: true}
    console.log(g === g[Symbol.iterator]()) //ture
    
    //中断执行
    let generatorObj = function*(){
        yield 'foo'
        yield 'bar'
        return 'baz'
    }
    const g = generatorObj()
    console.log(g.next()) //{value: 'foo', done: false}
    console.log(g.next()) //{value: 'bar', done: false}
    console.log(g.next()) //{value: 'baz', done: true}
    console.log(g.next()) //{value: undefined, done: true}
    
    //断言增强（一次产出一个值）
    function* generatorFn(){yield* [1,2,3]}
    let g = generatorFn()
    for (const x of g){console.log(x)}
    //1
    //2
    //3
    
    //生成器可以用return()停止或throw()方法暂停（抛出错误方式）
    ```
----
## 对象、类、面向对象编程
### 自定义对象
    ```javascript
    //创建对象
    let person = new Object()
    person.name = "Sancho"
    //字面量创建
    let person = {
        name: "Sancho"
    }
    
    //对象属性
    //  [[Configurable]]:是否可被delete删除并重新定义
    //  [[Enumerable]]:是否可被for-in循环返回
    //  [[writable]]:是否可被修改
    //  [[value]]:包含的属性值
    //  [[Get]]:读取时返回的获取函数
    //  [[Set]]:写入时返回的设置函数
    
    //修改属性
    let person = {}
    Object.defineProperty(person,"name",{writable: false,value: "Sancho"})
    console.log(person.name) //Sancho
    person.name = "Greg"
    console.log(person.name) //Sancho
    //修改多个属性用Object.defineProperties(person,{})方法
    //读取属性使用Object.getOwnPropertyDescriptor(person,"name")方法
    //返回全部属性使用Object.getOwnPropertyDescriptors(person)方法
    
    ```
  - 工厂模式：简单函数，可创建对象，为其添加属性和方法，返回这个对象(不能标识对象类型)
    ```javascript
    function createPerson(name){
        let o = new Object()
        o.name = name
        o.sayNmae = function(){console.log(this.name)}
    }
    let p1 = createPerson("Sancho")
    let p2 = createPerson("Greg")
    ```
  - 构造函数模式：可自定义引用类型；缺点是成员无法重用（包括函数）
    ```javascript
    function Person(name){
        this.name = name
        this.sayNmae = function(){console.log(this.name)
    }
    let p1 = new Person("Sancho")
    let p2 = new Person("Greg")
    p1.sayName() //"Sancho"
    p2.sayname() //"Greg"
    //如上构造函数会创建对象时把方法创造一遍增加消耗
    
    //方法外置
    function Person(name){
        this.name = name
        this.sayNmae = sayName //只包含函数的指针
    }
    function sayName(){console.log(this.name)}
    
    let p1 = new Person("Sancho")
    let p2 = new Person("Greg")
    p1.sayName() //"Sancho"
    p2.sayname() //"Greg"
    //此方法污染了全局作用域命名空间
    ```
  - 原型模式：成员可以共享；
  - 组合构造函数和原型模式：通过构造函数定义实例属性，通过原型定义共享的属性和方法
  - 盗用构造函数模式（传统继承）：在子类构造函数中调用父类构造函数实现每个实例继承的属性都是私有，只能通过构造函数模式定义（子类不能访问父类原型上的方法）
  - 组合继承模式（推荐）：通过原型链继承共享的属性和方法，通过盗用构造函数继承实例属性
  - 原型式继承模式：无须明确定义构造函数而实现继承，本质上是对给定对象执行浅复制（操作的结果可以在之后再次增强）
  - 寄生式模式：基于一个对象创建一个新对象，然后再增强这个新对象，然后返回新对象；
  - 寄生组合继承模式（最有效）：用于避免重复调用父类构造函数导致的浪费
### 对象创建过程（原型链）、继承
 - JS的继承主要通过原型链实现，原型链涉及吧构造函数的原型赋值为另一个类型的实例（子类可访问父类所有属性和方法）；所有继承的属性和方法都会在对象实例间共享（实例不能私有）
### 类
  - ES6新增类基于语法糖，方便定义向后兼容的类，可以继承内置类型和自定义类型；有效使用对象实例、对象原型和对象类
## 代理、反射
### 代理
  - 代理可以定义包含捕获器的处理程序对象，可以蓝爵大部分基本操作和方法，以修改基本操作的行为（遵从捕获器不变式）
### 代码捕获器与反射方法
  - 反射API封装了与捕获器拦截操作相对应的方法
### 代理模式
  - 代理可以创建出各种编码模式（如跟踪属性访问、隐藏属性、阻止修改或删除属性、函数参数验证、构造函数参数验证、数据绑定、可观察对象等）
----
## 函数
### 函数表达式、函数声明、箭头函数
  - 函数声明需要函数名称，函数表达式不需要，没有名称叫匿名函数
  - 箭头函数语法
### 默认参数及扩展操作符
  - arguments对象及ES6新增扩展操作符可以实现函数定义和调用的完全动态化
  - 函数内部暴露的对象和引用
  - 尾调用函数优化栈空间
### 使用函数实现递归
  - 闭包在函数返回之后，其作用域会一直保存在内存中，直到闭包被销毁
  - 立即调用函数在执行后不留下对函数的引用；立即调用函数表达式如果不在包含作用域中将返回值赋给一个变量，则其包含的所有变量都会被销毁
### 使用闭包实现私有变量
  - 可使用闭包实现公共方法，访问位于作用于中定义变量
  - 可以访问私有变量的公共方法（特权方法），特权方法可以使用构造函数或原型模式通过自定义类型中实现，也可以使用模块模式或者模块增强模式在单例对象上实现
## 期约、异步
### 异步
  - async/await
### 期约
  - 期约的主要功能是为异步代码提供清晰的抽象，可以用期约表示异步代码执行的代码块，也可以用期约表示异步计算的值；异步代码串行，可塑性强，可序列化、连锁使用、复合、拓展和重组
### 异步函数
  - 异步函数是将期约应用于JS函数的结果；异步函数可以暂停执行而不阻塞主线程；可方便编写基于期约的代码和组织串行或平行执行的异步代码
