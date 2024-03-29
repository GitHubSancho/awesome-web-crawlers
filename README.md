# awesome-web-crawlers
爬虫指南

# 爬虫基础知识
网络爬虫是模拟浏览器发送网络请求，接收请求响应，一种按照一定的规则，自动地抓取互联网信息的程序。  
原则上,只要是浏览器能做的事情，爬虫都能够做。
## 爬虫基本思路
1.获取网页
* 爬虫会通过HTTP向服务器发送请求，并返回响应体。[(了解HTTP基本原理)](Net/HTTP基础知识.md)
* 返回的响应体是网页源代码。[了解网页基本构成(!TODO)]()
* 当发送请求时需要Python请求库实现HTTP请求操作。（[requests](requests基本使用.ipynb)、[urllib](urllib基本使用.ipynb)基本使用方法）

2.提取信息
* 当服务器返回网页源代码时，我们需要Python解析库对源代码进行解析[re基本使用方法!TODO](#)
* 当构造正则解析库过于复杂时，采用网络节点进行解析[Beautiful Soup!TODO]()、[pyquery!TODO]()、[lxml!TODO]()基本使用方法

3.保存数据
* 提取到所需信息后保存文本数据[txt、JSON文本保存方法!TODO]()
* 当数据庞大或需要保存到远程服务器时，采用数据库存储[MySQL!TODO]()、[MongoDB](data_base/MongoDB基础知识.md)、[Redis](data_base/redis基础知识.md)基本使用方法]

## 进阶数据爬取
* 在编写更敏捷的程序时我们可能需要使用服务器，因此需要学习[Linux系统基础](linux/Linux基础.md)相关知识
* 为了突破信道传输极限，需要我们了解[计算机网络](Net/计算机网络.md)
* 了解Python才能更好的设计代码，Python基础知识[补充](Python进阶.md)
* 编写更敏捷的爬虫吧！[异步、并发、分布式的爬虫框架设计](/crawlers/)
* 有时网站可能被加密导致我们无法正常访问，我们需要通过[JavaScript基础知识](JavaScript/JavaScript笔记.md)来了解网页执行逻辑,然后根据网页反爬虫思想使用[JS逆向技术!TODO](JavaScript/js逆向.md)
* 如果对执行速度没有要求，我们可以动态爬取网页以避免逆向JS花费太多时间。[selenium、Splush!TODO]()
* 其它爬虫框架(!TODO)


## 其它内容
* 资源管理（!TODO:代理池、Cookies池）
* 验证码识别(!TODO)
* App逆向(!TODO)
