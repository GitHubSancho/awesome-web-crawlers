# Python进阶笔记
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
## 魔法函数
## 类和对象
## 自定义序列类
## set和dict
## 对象引用、可变性、垃圾回收
## 元类编程
## 迭代器生成器
## socket编程
## 多线程、多进程、线程池
## 协程和异步IO
## 并发编程

