# Flask入门
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
### Flask程序
- 基本格式：
```
from flask import Flask

app = Flask(__name__)


@app.route('/')
def hello():
    return 'Hello,World'


if __name__ == '__main__':
    app.run()
```
### 路由请求方式
- 默认只支持get
- 增加请求方式：`@app.route('/',methods=['GET','POST'])`
### 参数处理
- 路由传参：
```python
@app.route('/orders/<order_id>')
def get_order_id(order_id):
    return f'order_id is {order_id}'
```
- 参数限制
```python
@app.route('/orders/<int:order_id>')
def get_order_id(order_id):

    return f'order_id is {order_id}'
```
## Jinja2
### Jinja2简介
- Jinja2是一个模板引擎，设计思想源于Django，并拓展语法和功能，被Flask内置的模板语言
- 模板语言是被设计用来自动生成文档的简单文本格式
```python
from flask import Flask, render_template

@app.route('/')
def index():
    # 填充模板的数据
    url_str = "www.sancho.com"
    # 从template文件夹下拉取网页
    return render_template('index.html',url_str=url_str)
```
```html
//FILE: template/index.html
{{url_str}}
```
### 变量代码块
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
### 控制代码块
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
### 表单验证
```python
from flask import request
# 获取请求方式
request.method
# 获取请求参数
request.from.get()
```
### 消息闪现
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