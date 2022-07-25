# Linux相关问题
## Ubuntu
### 更改默认root密码
`sudo passwd`  
`[输入当前用户密码]`  
`[设置root密码]`  
`[再次输入root密码]`
`su root`：切换到root用户
`[输入root密码]`：确认成功
### 更换源
- 备份官方源  
`cp  /etc/apt/sources.list  /etc/apt/sources.list.backup`
- 替换第三方源  
`vi /etc/apt/sources.list`  编辑文件
`:%d`：清空文件  
  - 复制如下清华源（20.04 LTS），具体版本参考[使用帮助](https://mirror.tuna.tsinghua.edu.cn/help/ubuntu/)
```
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
```
`p`：粘贴文件
`:wq`：保存并退出
- 测试源
```
sudo apt-get update
sudo apt-get upgrade
```
### 更换pip源
- 新建pip路径  
`mkdir ~/.pip`  
- 创建pip配置文件  
`sudo gedit ~/.pip/pip.conf`  
- 复制以下代码到文件
```
[global]
timeout = 6000
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
trusted-host = pypi.tuna.tsinghua.edu.cn
[global]
timeout = 6000
index-url = https://mirrors.aliyun.com/pypi/simple/
trusted-host = mirrors.aliyun.com
```
- 其他国内pip源
豆瓣(http://pypi.douban.com/)  
华中理工大学(http://pypi.hustunique.com/ )  
山东理工大学(http://pypi.sdutlinux.org/ )  
中国科学技术大学(http://pypi.mirrors.ustc.edu.cn/ )  
阿里云(https://mirrors.aliyun.com/pypi/simple/  )  
清华大学(https://pypi.tuna.tsinghua.edu.cn/simple/ )  
V2EX(pypi.v2ex.com/simple)  
搜狐镜像(http://mirrors.sohu.com/Python/)  
Pypi官方(https://pypi.org/simple/)  
- 可能出现的问题
  - pip list出现警告
```DEPRECATION: The default format will switch to columns in the future. You can use --format=(legacy|columns) (or define a format=(legacy|columns) in your pip.conf under the [list] section) to disable this warning.```
  - 解决办法：
    - 修改pip.conf配置，添加如下内容  
    - `sudo gedit ~/.pip/pip.conf`
```
[list]
format=columns
# 或者
format=legacy
```