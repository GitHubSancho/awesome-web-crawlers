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