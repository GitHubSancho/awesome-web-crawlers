# awesome-web-crawlers
爬虫指南

# 爬虫基础知识
网络爬虫是模拟浏览器发送网络请求，接收请求响应，一种按照一定的规则，自动地抓取互联网信息的程序。  
原则上,只要是浏览器能做的事情，爬虫都能够做。
## 爬虫基本思路
1.获取网页
* 爬虫会通过HTTP向服务器发送请求，并返回响应体。[(了解HTTP基本原理)](HTTP基础知识.md)
* 返回的响应体是网页源代码。[了解网页基本构成](#)
* 当发送请求时需要Python请求库实现HTTP请求操作。[urllib、requests基本使用方法](#)

2.提取信息
* 当服务器返回网页源代码时，我们需要Python解析库对源代码进行解析[re基本使用方法](#)
* 当构造正则解析库过于复杂时，采用网络节点进行解析[Beautiful Soup、pyquery、lxml基本使用方法](#)

3.保存数据
* 提取到所需信息后保存文本数据[txt、JSON文本保存方法](#)
* 当数据庞大或需要保存到远程服务器时，采用数据库存储[MySQL、MongoDB、Redis基本使用方法](#)

## 进阶数据爬取
* Ajax
* selenium、Splush动态页面爬取
* 爬虫框架
* 分布式

## 其它内容
* 资源管理（代理池、Cookies池）
* 验证码识别
* App逆向
* JS解密
