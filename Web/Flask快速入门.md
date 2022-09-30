# Flask快速入门
## Flask
### Flask概述
- Flask是一个轻量级的Web框架
- 优点是稳定性和可拓展性，以降低开发难度，提高开发效率
- 丰富的[扩展列表](https://flask.pocoo.org/extensions)
- [中文文档](https://flask.net.cn/)
- Django功能比Flask大而全，Flask则比Django更加灵活
### Flask安装
- 安装虚拟环境（Docker、Virtualenv、Conda皆可）
- 安装Flask
```bash
pip install flask
```
### 导出模块列表
- 导出：`pip freeze > requirements.txt`
- 导入：`pip install -r requirements.txt`
### 基本Flask程序
```python
from flask import Flask

# 创建实例，参数是导入应用模块或包的名称
app = Flask(__name__) # 当前模块名


@app.route('/') # 触发函数的url
def hello(): # 关联url的函数
    return 'Hello,World' # 显示的结果


if __name__ == '__main__':
    app.run() # 运行实例
```

### 路由
- route()装饰器可以把函数绑定到URL，一般填有意义的URL方便记忆
- 路由传参：
```python
@app.route('/orders/<order_id>') # `<something>`标记变量，会作为关键字传递给函数
def get_order_id(order_id):
    return f'order_id is {order_id}'
```
- 参数数据类型限制，支持的转换器类型：string/int/float/path/uuid
```python
@app.route('/orders/<int:order_id>') # 限制参数为int数据类型
def get_order_id(order_id):

    return f'order_id is {order_id}'
```
- url唯一性和重定向
```python
@app.route('/projects/') # 尾部有斜杠表示文件夹，没有斜杠时会进行重定向自动添加斜杠
def projects():
    return 'The project page'

@app.route('/about') # 尾部没有斜杠表示文件，如果访问时添加斜杠会得到404错误，如此保证唯一性
def about():
    return 'The about page'
```
- `url_for`函数用于构建指定函数的url
```python
from flask import Flask, escape, url_for

app = Flask(__name__)

@app.route('/')
def index():
    return 'index'

@app.route('/login')
def login():
    return 'login'

@app.route('/user/<username>')
def profile(username):
    return '{}\'s profile'.format(escape(username))

with app.test_request_context(): # 测试Flask请求
    # url_for()第一个参数为函数名称，支持任意个关键字参数，每个关键字参数对应URL中的变量
    print(url_for('index'))
    print(url_for('login'))
    print(url_for('login', next='/'))
    print(url_for('profile', username='John Doe'))

# 输出：
# /
# /login
# /login?next=/
# /user/John%20Doe
```
- HTTP方法：默认请求方式GET（支持GET也自动支持HEAD和OPTIONS方式）
```python
# 增加请求方式
from flask import request

@app.route('/login', methods=['GET', 'POST'])
def login():
    # 根据请求方式改变执行逻辑
    if request.method == 'POST': # 查看请求方式
        return do_the_login()
    else:
        return show_the_login_form()
```
- 引用静态文件(css or js)
```python
# filepath:./static/style.css
url_for(static,filename='style.css')
```

## Jinja2
### Jinja2简介
- Jinja2是一个模板引擎，设计思想源于Django，并拓展语法和功能，被Flask内置的模板语言
- 模板语言是被设计用来自动生成文档的简单文本格式
- Jinja2模板文档：http://jinja.pocoo.org/docs/templates/
### 渲染模板
- `render_template()`: 提供模板名称和需要作为参数传递给模板的变量自动配置
```python
from flask import Flask, render_template

@app.route('/hello/')
@app.route('/hello/<name>')
def hello(name=None):
    # 从template文件夹下拉取网页
    return render_template('hello.html',name=name)
```
```html
//FILE: template/hello.html
<!doctype html>
<title>Hello from Flask</title>
{% if name %}
  <h1>Hello {{ name }}!</h1>
{% else %}
  <h1>Hello, World!</h1>
{% endif %}
```
- 注释：`{# #}`
- 其它数据类型传入：
```python
from flask import Flask, render_template

@app.route('/')
def index():
    # 填充模板的数据
    url_str = "www.sancho.com"
    my_list = [1,2,3,4,5]
    my_dict = {'name':'Sancho',"age":23}
    # 通常传递的数据名和变量名保持一致
    return render_template('index.html',url_str=url_str,my_list=my_list,my_dict=my_dict)
```
- 数据索引：
```html
//FILE: template/index.html
{{url_str}} <br/>

{{url_list}} <br/>
{{url_list.0}} <br/>
{{url_list[1]}} <br/>

{{url_dict}} <br/>
{{url_dict.name}} <br/>
{{url_dict['age']}} <br/>
```
- 基本判断
```html
{%if user %}
    {{user}}
{% else %}
    Not found user!
```
- 基本遍历
```html
{% for index in indexs %}
    {{ index }}
{% endfor %}
```
### 过滤器
- 过滤器类似函数，改变输出样式
- 更多过滤器示例：https://blog.csdn.net/LarsGyonX/article/details/123075843
```html
# 小写示例
{{ url_dict.name | lower}}
```
- 链式调用：`{{ url_dict.name | upper | reverse }}`

## 表单
### 请求对象
- 处理表单数据，`request.form`属性返回传输的数据，不存在时返回HTTP 400 Bad Request
```python
from flask import request

@app.route('/login', methods=['POST', 'GET'])
def login():
    error = None
    if request.method == 'POST': # 判断请求方式
        if valid_login(request.form['username'],
                       request.form['password']): 
            return log_the_user_in(request.form['username'])
        else:
            error = 'Invalid username/password' 
    return render_template('login.html', error=error)
```
- 处理url参数
```python
# 例：?key=value
searchword = request.args.get('key', '')
```
### 文件上传
- 参考：https://flask.net.cn/quickstart.html#id17

### Cookies
- 参考：https://flask.net.cn/quickstart.html#cookies

## 重定向和错误
-  `redirect()`函数可以重定向 
-  `abort()`可以 更早退出请求，并返回错误代码
```python
from flask import abort, redirect, url_for

@app.route('/')
def index():
    return redirect(url_for('login'))

@app.route('/login')
def login():
    abort(401)
    this_is_never_executed()
```
- `errorhandler()`装饰器定制出错页面，错误处理参考：https://flask.net.cn/errorhandling.html#error-handlers
```python
from flask import render_template

@app.errorhandler(404) # 404表示错误代码，缺省为200=访问正常
def page_not_found(error):
    return render_template('page_not_found.html'), 404
```
## 相应对象和自动转换
- 参考：https://flask.net.cn/quickstart.html#id17

## 会话
- 参考：https://flask.net.cn/quickstart.html#sessions

### 消息闪现
- 参考：https://flask.net.cn/patterns/flashing.html#message-flashing-pattern
```python
from flask import flash
# 消息混淆加密
app.secret_key = 'sancho'
if not all([username,password]):
    flash(u'请正确输入账号/密码')
```
```html
//FILE: template/index.html
{# 获取闪现消息 #}
{% for message in get_flashed_messages() %}
    {{ message }}
{% endfor %}
```

## 日志
- 参考：https://docs.python.org/3/library/logging.html#module-logging

## WSGI中间件
- 参考：https://werkzeug.palletsprojects.com/

## Flask拓展
- 参考：https://flask.net.cn/extensions.html#extensions
### WTF扩展
- 安装：`pip install flask-wtf`
- 自定义表单类
```python
from flask_wtf import FlaskForm
from wtforms import StringField,PasswordField,SubmitField

class LoginForm(FlaskForm):
    username = StringField(u'用户名:')
    password = PasswordField(u'密码:')
    submit = SubmitField('提交')

@app.rout('/from',methods=['GET','Post'])
def login():
    login_form = LoginForm()
    return render_template('index.html',form=login_form)
```
```html
//FILE: template/index.html
<form method="post">
    {{ form.username.label }}{{ form.username }} <br>
    {{ form.password.label }}{{ form.password }} <br>
    {{ form.submit }}
```
- 表单逻辑验证
```python
from flask_wtf import FlaskForm
from wtforms import StringField,PasswordField,SubmitField
from wtforms.validators import DataRequired, EqualTo

class LoginForm(FlaskForm):
    username = StringField(u'用户名:',validators=[DataRequired()])
    password = PasswordField(u'密码:',validators=[DataRequired()])
    password2 = PasswordField(u'确认密码:',validators=[DataRequired(),EqualTO('password','前后密码不一致')])
    submit = SubmitField('提交')

@app.route('/form',methods=['GET','POST'])
def login():
    login_form = LoginForm()
    if request.method == 'POST':
        username = request.form.get('username')
        password = request.form.get('password')
        password2 = request.form.get('password2')

        # 验证参数(需要在html中添加csrf_token())
        if login_form.validate_on_submit():
            print(username,password)
            return 'success'
        elif err:=login_form.errors.get('password2')[0]:
            flash(err)
        else:
            flash(u'请检查账号/密码')

    return render_template('index.html',form=login_form)  
```
```html
//FILE: template/index.html
<form method="post">
    {{ form.csrf_token() }}
    {{ form.username.label }}{{ form.username }} <br>
    {{ form.password.label }}{{ form.password }} <br>
    {{ form.submit }}
```

## 数据库
### SQLAlchemy安装
- 安装：pip install flask-sqlalchemy
- 连接mysql数据库需要下载：pip install flask-mysqldb
- 数据库配置：
    - 导入模块：`from flask_sqlalchemy import SQLAlchemy`
    - 初始化模块：`db = SQLAlchemy(app)`
    - 数据库位置：`app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql://root:mysql@127.0.0.1:3306/test'`
    - 动态追踪修改：`app.config['SQLALCHEMY_TRACK_MODIFICATIONS']=False`
    - 显示SQL语句：`app.config['SQLALCHEMY_ECHO']=True`
### 定义数据类型
```python
# 继承数据库模型
class Role(db.Model):
    # 定义表名
    __tablename__ = 'roles'
    # 定义字段
    id = db.Column(db.Intger,primary_key=True)
    name = db.Column(db.String(16),unique=True)

class User(db.Model):
    # 定义表名
    __tablename__ = 'users'
    # 定义字段
    id = db.Column(db.Intger,primary_key=True)
    name = db.Column(db.String(16),unique=True)
    # 链接外键
    role_id = db.Column(db.Interger,db.ForeignKey('roles.id'))
```
### 数据库基本操作
- 增删改
```python
db.session.add(role) # 添加到数据库的session中
db.session.add_all([user1,user2]) # 添加多个信息到session中
db.session.committ() # 提交数据库的修改（包括增/删/改）
db.session.rollback() # 数据库回滚操作
db.session.delete(user) # 删除数据库（需要更上commit修改）
```
- 关系引用：`user = db.relationship('User',backref='role')`
- 查询：
```python
User.query.all() # 查询所有用户数据
User.query.count() # 统计用户
User.query.first() # 查询第一个用户
User.query.get(4) # 按id查询
```

## 服务器部署
- 参考：https://flask.net.cn/deploying/index.html#deployment