# HTTP基础知识
HTTP协议指超文本传输协议(HyperText Transfer Protocol)  
用于分布式、协作式和超媒体信息系统的[应用层<sup>1</sup>]()协议  

## HTTP工作原理
浏览器作为HTTP客户端通过[URL<sup>2</sup>]()向HTTP服务端即WEB服务器发送所有请求。  
Web服务器根据接收到的请求后，向客户端发送响应信息。  
HTTP协议通信流程：浏览器↔服务端↔公共网关接口↔数据库

## HTTP消息结构
客户端发送HTTP请求消息格式：请求行（request line）、请求头部（header）、空行和请求数据四个部分组成  
服务器响应HTTP请求消息格式：状态行、消息报头、空行和响应正文  
```
#客户端请求实例：
GET /hello.txt HTTP/1.1
User-Agent: curl/7.16.3 libcurl/7.16.3 OpenSSL/0.9.7l zlib/1.2.3
Host: www.example.com
Accept-Language: en, mi
```
```
#服务器响应示例：
HTTP/1.1 200 OK
Date: Mon, 27 Jul 2009 12:28:53 GMT
Server: Apache
Last-Modified: Wed, 22 Jul 2009 19:15:56 GMT
ETag: "34aa387-d-1568eb00"
Accept-Ranges: bytes
Content-Length: 51
Vary: Accept-Encoding
Content-Type: text/plain
```

### HTTP请求方法*(HTTP版本1.1)*
|序号|方法|描述|
|----|----|----|
|1|get|请求指定的页面信息，并返回实体主体|
|2|head|类似于 GET 请求，只不过返回的响应中没有具体的内容，用于获取报头|
|3|post|向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求体中。POST 请求可能会导致新的资源的建立和/或已有资源的修改|
|4|put|	从客户端向服务器传送的数据取代指定的文档的内容。|
|5|delete|请求服务器删除指定的页面。|
|6|connect|HTTP/1.1 协议中预留给能够将连接改为管道方式的代理服务器|
|7|options|允许客户端查看服务器的性能|
|8|trace|回显服务器收到的请求，主要用于测试或诊断|
|9|patch|	是对 PUT 方法的补充，用来对已知资源进行局部更新|

### HTTP响应头信息
|应答头|说明|
|----|----|
|Allow|服务器支持哪些请求方法（如GET、POST等）|
|Content-Encoding|文档的编码类型|
|Content-Length|文档的内容长度|
|Content-Type|文档的MIME类型|
|Date|当前的GMT时间|
|Expires|缓存文档的到期时间|
|Last-Modified|文档的最后改动时间|
|Location|文档的提取位置|
|Refresh|距离文档刷新时间（秒）|
|Server|服务器名|
|Set-Cookie|设置和页面关联的Cookie|
|WWW-Authenticate|客户在Authorization头中提供授权信息的类型|

### HTTP常见状态码
|状态码|说明|
|----|----|
|200|请求成功|
|301|资源（网页等）被永久转移到其它URL|
|404|请求的资源（网页等）不存在|
|500|内部服务器错误|
* 信息响应(100–199)：信息，服务器收到请求，需要请求者继续执行操作
* 成功响应(200–299)：成功，操作被成功接收并处理
* 重定向(300–399)：重定向，需要进一步的操作以完成请求
* 客户端错误(400–499)：客户端错误，请求包含语法错误或无法完成请求
* 服务器错误 (500–599)：服务器错误，服务器在处理请求的过程中发生了错误

### HTTP内容类型
常见的媒体格式类型如下：
* text/html ： HTML格式
* text/plain ：纯文本格式
* text/xml ： XML格式
* image/gif ：gif图片格式
* image/jpeg ：jpg图片格式
* image/png：png图片格式

以application开头的媒体格式类型：
* application/xhtml+xml ：XHTML格式
* application/xml： XML数据格式
* application/atom+xml ：Atom XML聚合格式
* application/json： JSON数据格式
* application/pdf：pdf格式
* application/msword ： Word文档格式
* application/octet-stream ： 二进制流数据（如常见的文件下载）
* application/x-www-form-urlencoded ： <form encType=””>中默认的encType，form表单数据被编码为key/value格式发送到服务器（表单默认的提交数据的格式）

另外一种常见的媒体格式是上传文件之时使用的：
* multipart/form-data ： 需要在表单中进行文件上传时，就需要使用该格式
