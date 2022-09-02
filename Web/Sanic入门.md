# Sanic入门
## 概览
### 介绍
- 需要Python3.7+
- Sanic是高效的编写、部署和扩展生产级的Web应用程序
- 参考文档：https://sanic.dev/  
- 安装：`pip install sanic`
    - 从conda安装：
```bash
conda config --add channels conda-forge 
conda install sanic
```
- 基本格式：
```python
# FILE:main.py
from sanic import Sanic
from sanic.response import json

app = Sanic("hello_example")

@app.route("/")
async def test(request):
    return json({"hello":"world"})

if __name__ == "__main__":
    app.run(host="127.0.0.1",port=8000)
```
- 检查浏览器：打开地址`http://127.0.0.1:8000`
- Sanic拓展
  - 安装：`pip install sanic[ext]`或`pip install sanic sanic-ext`
  - 调用：`app.extend()`或`app.ext`
## 入门
### Sanic应用
- 实例化：推荐使用文件名称作为实例化对象
```python
# FILE:main.py
from sanic import Sanic

app.Sanic(__file__)
```
- 应用上下文
```python
# FILE:main.py
from sanic import Sanic
app = Sanic(__file__)
# app.db = Database() # 21.3弃用
app.ctx.db = Database() # 数据库连接
```
- 注册表
```python
# FILE:sercer.py
from sanic import Sanic
app = Sanic("my_awesome_server")
```
```python
# FILE:somewhere_else.py
from sanic import Sanic
app = Sanic.get_app("my_awesome_server",force_create=True) # 强制创建
```
- 配置
```python
app = Sanic('myapp')
# 键名通常大写
app.config.DB_NAME = 'appdb'
app.config['DB_USER'] = 'appuser'
# 或者字典方式传递
db_settings = {'DB_HOST':'localhost','DB_NAME':'appdb','DB_USER':'appuser'}
app.config.update(db_settings)
```
### 响应函数
- 响应函数是一个可调用程序，至少以一个request实例作为参数，并返回一个`HTTPResponse`实例或一个执行其它操作的协同程序作为响应
```python
# 响应指定端点的访问，并可以承载业务逻辑代码
def handler(request):
    return HTTPResponse()

async def also_handler(request):
    return HTTPResponse()
```
  - 响应示例：
```python
from sanic.response import text

@app.get("/foo")
async def foo_handler(request):
    return text("I said foo!")

```
  - 完整注释响应示例：
```python
from sanic.response import HTTPResponse, text
from sanic.request import Request

@app.get("/typed")
async def typed_handler(request: Request) -> HTTPResponse:
    return text("Done.")
```
> 更多封装逻辑信息：https://sanic.dev/zh/guide/advanced/class-based-views.html  
> 更多异步拓展：https://github.com/mekicha/awesome-sanic  
### 请求
- 请求体
```sh
curl localhost:8000 -d '{"foo":"bar"}'
```
```py
print(request.json)
# {'foo':'bar'}
```
- 请求上下文：`request.ctx` 对象是存储相关请求信息的地方
```python
@app.middleware("request")
async def run_before_handler(request):
    request.ctx.user = await fetch_user_by_token(request.token)

@app.route('/hi')
async def hi_my_name_is(request):
    return text("Hi, my name is {}".format(request.ctx.user.name))
```
- 连接上下文：多个请求共享一个连接时，上下文对象允许请求共享状态
```python
@app.on_request
async def increment_foo(request):
    if not hasattr(request.conn_info.ctx, "foo"):
        request.conn_info.ctx.foo = 0
    request.conn_info.ctx.foo += 1

@app.get("/")
async def count_foo(request):
    return text(f"request.conn_info.ctx.foo={request.conn_info.ctx.foo}")
```
```sh
$ curl localhost:8000 localhost:8000 localhost:8000
request.conn_info.ctx.foo=1
request.conn_info.ctx.foo=2
request.conn_info.ctx.foo=3
```
- 路由参数：从路径提取的路由参数作为参数（或更具体的关键字参数）传递到处理程序中
```python
@app.route('/tag/<tag>')
async def tag_handler(request, tag):
    return text("Tag - {}".format(tag))
```
- 请求参数：可通过`request.args`或`request.query_args`访问请求参数
```sh
$ curl http://localhost:8000\?key1\=val1\&key2\=val2\&key1\=val3
```
```python
print(request.args)
# {'key1': ['val1', 'val3'], 'key2': ['val2']}
print(request.args.get("key1"))
# val1
print(request.args.getlist("key1"))
# ['val1', 'val3']
print(request.query_args)
# [('key1', 'val1'), ('key2', 'val2'), ('key1', 'val3')]
print(request.query_string)
# key1=val1&key2=val2&key1=val3
```
### 响应
- 所有的响应函数都必须返回一个response对象，中间件可以自由选择是否返回response对象
- 响应方式：共有9种常用返回类型（text,html,json,file等）
```python
from sanic.response import text

@app.route("/")
async def handler(request):
    return text("Hi 😎")
```
- 状态码：默认的HTTP状态码是`200`，可以通过`status`参数修改
```python
@app.post("/")
async def create_new(request):
    new_thing = await do_create(request)
    return json({"created": True, "id": new_thing.thing_id}, status=201)
```
### 路由
- 基本挂载方式：默认监听`GET`请求
```python
async def handler(request):
    return text("OK")
app.add_route(handler, "/test")
```
  - 监听多种请求
```python
app.add_route(
    handler,
    '/test',
    methods=["POST", "PUT"],
)
```
- 使用装饰器绑定：
```python
@app.route('/test', methods=["POST", "PUT"])
async def handler(request):
    return text('OK')
```
> API文档：https://sanic.readthedocs.io/en/stable/sanic/api/app.html?highlight=url_for#module-sanic.app
- 路由参数模式匹配
```python
@app.get("/tag/<tag>")
async def tag_handler(request, tag):
    return text("Tag - {}".format(tag))
```
- 指定路由参数类型（强制类型转换）
```python
@app.get("/foo/<foo_id:uuid>")
async def uuid_handler(request, foo_id: UUID):
    return text("UUID - {}".format(foo_id))
```
> 匹配更多类型：https://sanic.dev/zh/guide/basics/routing.html#%E8%B7%AF%E7%94%B1%E5%8F%82%E6%95%B0-path-parameters
- 动态访问：`app.url_for`实现响应函数之间的处理权利移交
```python
@app.route('/')
async def index(request):
    # generate a URL for the endpoint `post_handler`
    url = app.url_for('post_handler', post_id=5)

    # Redirect to `/posts/5`
    return redirect(url)


@app.route('/posts/<post_id>')
async def post_handler(request, post_id):
    ...
```
> 更多关键字参数：https://sanic.readthedocs.io/en/stable/sanic/api/app.html?highlight=url_for#sanic.app.Sanic.url_for  
### 监听器
- 启用监听器：当前正在运行的应用程序会注入到监听器中
```python
@app.listener("before_server_start")
# 或 @app.before_server_start
async def setup_db(app):
    app.ctx.db = await db_setup()
```
- 执行顺序：  
    |                     | 执行阶段   | 执行顺序 |
    | ------------------- | ---------- | -------- |
    | main_process_start  | 主程序启动 | 正向 😃   |
    | before_server_start | 子程序启动 | 正向 😃   |
    | after_server_start  | 子程序启动 | 正向 😃   |
    | before_server_stop  | 子程序关闭 | 反向 🙃   |
    | after_server_stop   | 子程序关闭 | 反向 🙃   |
    | main_process_stop   | 主程序关闭 | 反向 🙃   |
### 中间件
- 启用
```python
@app.middleware("request")
async def extract_user(request):
    request.ctx.user = await extract_user_from_request(request)
# 或同时接受两个参数
@app.middleware('response')
async def prevent_xss(request, response):
    response.headers["x-xss-protection"] = "1; mode=block"
```
```python
# 简写
@app.on_request
async def extract_user(request):
    ...

@app.on_response
async def prevent_xss(request, response):
    ...
```
- 执行顺序
```python
@app.middleware("request")
async def add_key(request):
    # Arbitrary data may be stored in request context:
    request.ctx.foo = "bar"


@app.middleware("response")
async def custom_banner(request, response):
    response.headers["Server"] = "Fake-Server"


@app.middleware("response")
async def prevent_xss(request, response):
    response.headers["x-xss-protection"] = "1; mode=block"


@app.get("/")
async def index(request):
    return text(request.ctx.foo)

# 1. 请求中间件：add_key
# 2. 响应函数：index
# 3. 响应中间件：prevent_xss
# 4. 响应中间件：custom_banner
```
- 变更参数：`request.match_info`
```python
@app.on_request
def convert_slug_to_underscore(request: Request):
    request._match_info["slug"] = request._match_info["slug"].replace("-", "_")


@app.get("/<slug:[a-z0-9]+(?:-[a-z0-9]+)*>")
async def handler(request, slug):
    return text(slug)
```
```sh
$ curl localhost:9999/foo-bar-baz
foo_bar_baz
```
### 标头
- 令牌
```python
@app.route("/")
async def handler(request):
    return text(request.token)
```
```sh
$ curl localhost:8000 \
    -H "Authorization: Token ABCDEF12345679"
ABCDEF12345679
```
- 代理头
```python
app.config.SERVER_NAME = "https://example.com"

@app.route("/hosts", name="foo")
async def handler(request):
    return json(
        {
            "effective host": request.host,
            "host header": request.headers.get("host"),
            "forwarded host": request.forwarded.get("host"),
            "you are here": request.url_for("foo"),
        }
    )
```
```sh
$ curl localhost:8000/hosts
{
  "effective host": "example.com",
  "host header": "localhost:8000",
  "forwarded host": null,
  "you are here": "https://example.com/hosts"
}
```
### Cookies
- 读取：
```python
@app.route("/cookie")
async def test(request):
    test_cookie = request.cookies.get("test")
    return text("Test cookie: {}".format(test_cookie))
```
- 写入：
```python
@app.route("/cookie")
async def test(request):
    response = text("There's a cookie up in this response")
    response.cookies["test"] = "It worked!"
    response.cookies["test"]["domain"] = ".yummy-yummy-cookie.com"
    response.cookies["test"]["httponly"] = True
    return response
```
- coockies参数：  
    | 参数名称 | 参数类型 | 参数说明                                                      |
    | -------- | -------- | ------------------------------------------------------------- |
    | expires  | datetime | Cookie 在客户端浏览器上失效的时间。                           |
    | path     | str      | 此 Cookie 适用的 URL 子集。默认值为 /                         |
    | comment  | str      | 注释（元数据）                                                |
    | domain   | str      | 指定 Cookie 的有效域。显式指定的域必须始终以点开始。          |
    | max-age  | int      | Cookie 应生存的秒数。                                         |
    | secure   | bool     | 指定是否仅通过 HTTPS 发送 Cookie。                            |
    | httponly | bool     | 指定 Javascript 是否无法读取 Cookie。                         |
    | samesite | str      | 默认值取决于浏览器，规范状态（Lax、Strict 和 None）是有效值。 |
- 删除
```python
@app.route("/cookie")
async def test(request):
    response = text("Time to eat some cookies muahaha")

    # 此 cookie 将被立即删除
    del response.cookies["kill_me"]

    # 此 cookie 将在 5 秒后删除
    response.cookies["short_life"] = "Glad to be here"
    response.cookies["short_life"]["max-age"] = 5
    
    del response.cookies["favorite_color"]

    # 此 cookie 将保持不变
    response.cookies["favorite_color"] = "blue"
    response.cookies["favorite_color"] = "pink"
    
    del response.cookies["favorite_color"]

    return response
```
### 后台任务
- 创建任务
```python
async def auto_inject(app):
    await asyncio.sleep(5)
    print(app.name)

app.add_task(auto_inject)
```
- 在启动之前添加任务
```python
async def slow_work(...):
   ...

app = Sanic(...)
app.add_task(slow_work) # Note: we are passing the callable and not coroutine object `slow_work(...)`
app.run(...)
```
- 命名任务：`app.add_task(slow_work, name="slow_task")`
- 调用任务：`task = app.get_task("slow_task")`
- 取消任务：`await app.cancel_task("slow_task")`
- 清除冗余任务：`app.purge_tasks()`