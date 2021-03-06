# Linux基础
## 基础篇
### VM和Linux安装
- VMware下载：https://www.vmware.com/cn.html
- Centos7.6/8.1下载：https://archive.kernel.org/centos-vault/
- 新建虚拟机
  - CPU分配：根据本机资源酌情分配核心
  - 网络连接模式
    - NAT模式（建议）：网络地址转换模式，可以和外部通讯，不造成IP冲突
    - 桥接模式：虚拟系统可以和外部系统通讯，但容易造成IP冲突
    - 仅主机模式：独立系统，不与外部通讯 
  - 软件选择（默认最小安装，方便选择：GNOME桌面，传统X Windows兼容性、兼容性程序库、开发工具）
  - 分区设置
    - boot 标准分区 ext4 1G
    - swap 标准分区 swap 2G（和分配内存大小一致即可）
    - / 标准分区 ext4 17G
  - KDUMP：内核崩溃转储机制，生产环境建议开启
  - 网络：打开以太网，修改主机名
  - 设置密码和用户（生产环境中建议[生成复杂密码](https://suijimimashengcheng.bmcx.com/)）
  - 安装VMtool 
- 虚拟机操作
 - 虚拟机克隆
    - 直接复制文件夹
    - 使用VMware克隆
  - 快照
  - 迁移和删除
    - 直接剪切文件夹可以迁移
    - 直接删除文件夹可以删除
### Linux目录结构
- 树状结构
  - /：根目录
  - /bin：常用指令
  - /sbin：管理员指令
  - /home：用户目录
  - /root：管理员目录
  - /lib：共享库
  - /etc：配置文件和子目录
  - /usr：应用程序
    - /usr/local：软件安装目录（一般是通过编译源码方式安装的程序）
  - /boot：启动文件
  - /dev：硬件设备管理
  - /media：外设挂载目录
  - /mnt：外部存储挂载
  - /proc：[不能动]内存映射
  - /srv：[不能动]服务启动之后需要提取的数据
  - /sys：[不能动]sysfs文件系统
  - /lost+found[隐藏]：系统非正常关机时的缓存
  - /tmp：临时文件
  - /opt：额外安装软件存放目录
  - /var：存放不断扩充的目录（如日志文件）
  - /selinux：[自由启动]安全子系统
## 操作篇
### 远程访问
- 远程登录xshell
  - [下载](https://www.netsarang.com/en/free-for-home-school/)free-for-home-shool版本
  - 检查网络
    - 查看服务端ip地址```ifconfig```
    - 客户端尝试连通服务端```ping ip地址```
    - 在xshell新建会话：主机填ip地址、端口22；双击会话，选择是否保存密码；输入用户名和密码后登录；登录后即可使用命令操作服务器
- 远程传输xftp
  - 在xftp新建会话：主机填ip地址、端口22；双击会话，选择是否保存密码；输入用户名和密码后登录
  - 界面左边是本机，界面右边是远程机； 若远程机显示乱码，则在软件左上角文件→当前会话属性→选项→编码→选择utf-8；即可对文件进行相互传输、修改等操作
### Vi和Vim编辑器
- vi三种模式：
  - 默认模式：输入esc退出当前模式进入默认模式，上下左右移动光标、删除字符、删除整行、复制粘贴操作数据
    - `yy`拷贝当前行；`5yy`拷贝当前行向下5行；`p`粘贴
    - `dd`删除当前行；`5dd`删除当前行向下5行
    - `gg`跳转首行；`G`跳转到末行
    - `u`撤销
    - `行号shift+G`跳转某行
  - 插入模式：默认模式中输入i、o、a、r大小写均可进入编辑
  - 命令行模式：默认模式中输入冒号:或斜杠/进入命令行模式；提供相关指令，完成读取、存盘、替换、离开vim、显示行号等的动作
    - `:wq`(保存并对退出);`:q`(退出);`:q!`(强制退出,不保存)
    - 查询单词`/单词,回车查询,输入n查找下一个`
    - `:set nu`显示行号；`:set nonu`隐藏行号
### 开机、重启、用户登录注销
- 关机和重启
  - `shutdown -h now`：立刻关机
  - `shutdown -h 1`：1分钟后关机
  - `shutdown -r now`：重启
  - `halt`：关机
  - `reboot`：重启
  - `sync`：把内存数据同步到磁盘（建议每次优先执行）
- 登录和注销
  - 登录时尽量少用root帐号登录，普通用户使用`su - root`切换成系统管理员身份
  - 在提示符下输入`logout`即可注销账户（图形界面无效）
### 用户管理
- 添加用户
  - `useradd 用户名`即可添加用户名；`useradd -d 目录 用户名`即可在指定目录创建家目录
  - 新用户默认在/home/用户名/下创建家目录
  - `passwd 用户名`修改用户密码
- 删除用户
  - `userdel 用户名`删除用户并保留家目录；`userdel -r 用户名`删除用户并删除家目录
- 查询用户信息
  - `id 用户名`查询用户信息(用户id，组id，所在组)
  - `su - 用户名`即可切换用户，切换到root不需要用户名；切换到权限更低的用户不需要密码，反之需要；切换回原来用户使用exit或logout命令
  - `whoami`或`who am I`查询登录到机器的当前用户
- 用户组
  - `groupadd 组名`新增组
  - `groupdel 组名`删除组
  - `useradd -g 用户组 用户名`创建用户时加入指定用户组
  - `usermod -g 用户组 用户名`修改用户的组
- 用户相关文件
  - /etc/passwd 文件存储所有用户的信息，其中每行显示:`用户名:口令:用户标识号:组标识号:注释性说明:主目录:登录Shell`
  - /etc/shadow 是口令配置文件：`登录名:加密口令:最后一次修改时间:最小时间间隔:最大时间间隔:警告时间:不活动时间:失效时间:标志`
  - /etc/group 文件存储组的配置文件:`组名:口令:组标识号:组内用户列表`
### 实用指令
- 运行级别
  - 常用运行级别（3和5常用）：
    0.关机
    1.单用户(找回丢失密码)
    2.多用户状态，没有网络服务
    3.多用户状态，有网络服务
    4.系统未使用保留给用户
    5.图形界面
    6.系统重启
  - `init 数字`切换到相应运行级别
  - 查看运行级别:`systemctl get-default`
  - 设置默认运行级别：`systemctl set-default 运行级别.target`
    - 多用户有网络状态：`systemctl set-default multi-user.target`
    - 图形界面状态：`systemctl set-default graphical.target`
- 找回root密码
  - centos7.6
    1. 启动系统开机界面按下[e]，移动光标到`linux16开头，UTF-8结尾的行`，输入[ init=/bin/sh]后按Ctrl+x进入单用户模式
    2. 在光标闪烁的位置输入`mount -o remount,rw /`后回车
    3. 在新行输入`passwd`后回车，输入新密码和确认密码
    4. 在光标位置输入`touch /.autorelabel`后回车
    5. 输入`exec /sbin/init`回车，等待后系统自动重启，完成
- 帮助指令
 - man
   - 语法：`man [命令或配置文件]`，获得帮助信息
 - help
   - 语法：`help 命令`，获得shell内置命令的帮助信息
- 文件目录操作
  - `pwd`：显示当前工作目录的绝对路径
  - `ls [选项] [目录或文件]`：默认查看当前目录信息
  - `cd [目录]`切换到指定目录，默认切换到home目录，..表示上一级目录
  - `mkdir [选项]`：创建目录，-p创建多级目录
  - `rmdir [选项]`：删除空目录，`rm -rf [目录]`删除非空目录
  - `touch [目录]`：创建空文件
  - `cp [选项] [文件] [目录]`：将文件拷贝到指定目录，-r递归复制整个文件夹，`\cp -r [文件] [目录]`拷贝并强制覆盖
  - `mv [oldFile] [newFile]`：移动文件目录或重命名
  - `cat [选项] [fileName]`：查看文件内容，-n显示行号;一般会带上管道命令` | more`
  - `more [fileName]`：按页显示文本内容
    - `Space`：翻页
    - `Enter`：下一行
    - `q`：立刻离开
    - `Ctrl+F`：向下一屏
    - `Ctrl+B`：向上一屏
    - `=`：输出当前行号
    - `:f`：输出文件名和当前行号
  - `less [fileName]`：分屏查看文件内容
    - `[Space]`、`[pagedown]`：翻页
    - `[pageup]`:上翻
    - `/字串`：向下搜寻字串，`n`向下查找，`N`向上查找
    - `?字串`：向上搜寻字串的功能，`n`向下查找，`N`向上查找
    - `q`：立刻z离开
  - `echo [选项] [输出内容]`：输出内容到控制台
  - `head [fileName]`：显示文件开头部分，默认前10行；`head -n [num] [fileName]`：通过-n和数字查看任意行数
  - `tail [fileName]`：显示文件结尾部分，默认后10行；`tail -n [num] [fileName]`：通过-n和数字查看任意行数；`tail -f [fileName]`：实时追踪文件更新
  - `>`重定向，`>>`追加
    - `ls -l > [fileName]`：将输出内容写入文件中（没有文件自动创建）
    - `ls -l >> [fileName]`：将输出内容追加到文件末尾
    - `cat [file1] > [file2]`：将文件1的内容覆盖到文件2
    - `echo "str" > [fileName]`：将内容追加到文件末尾
  - `ln -s [源文件或目录] [软链接名或目录]`：给文件创建软链接（类似快捷方式）
  - `history`：查看已执行过的历史命令，`history [num]`：查看指定数量使用过的命令历史，`![num]`：执行数字为历史编号的指令
- 时间日期类
  - `date` ：显示当前时间
    - `date +%Y`：显示当前年份
    - `date +%m`：显示当前月份
    - `date +%d`：显示当前日期
    - `date "+%Y-%m-%d %H:%M:%S"`：显示当前日期时间
    - `date -s "2022-07-08 02:23:10"`设置时间
  - `cal`查看日历，输入年份查看指定年度日历
- 搜索查找类
  - `find [搜索范围] [选项]`：搜索指定目录下满足条件的文件或目录
    - `-name`：按照指定文件名查找文件
    - `-user`：查找指定用户名下所有文件
    - `-size`：按照指定的文件大小查找（+n大于，-n小于，n等于；单位：k,M,G）
  - `locate [fileName]`：快速定位文件路径
    - `updatedb`：创建或更新locate数据库
  - `which [指令]`：查看指令的目录
  - `grep [选项] "内容" [源文件]`：过滤查找，`|`将前一个命令处理的结果传递给后面命令处理
    - `-n`：显示匹配行和行号
    - `-i`：忽略大小写
- 压缩和解压类
  - `gzip [fileName]`：将文件压缩为*.gz文件
  - `gunzip [filename.gz]`：解压缩gz文件
  - `zip [选项] [fileName] [dir]`：将文件压缩为*.zip文件
    - `-r`：递归压缩
  - `unzip [选项] [fileName.zip]`：解压缩zip文件
    - `-d`：指定解压后文件的存放目录
  - `tar [选项] [fileName] [dir+]`：打包或解压文件,可以打包多个文件
    - `-c`：产生.tar打包文件
    - `-v`：显示详细信息
    - `-f`：指定压缩后的文件名
    - `-z`：用gzip对文件压缩或解压
    - `-x`：解包.tar文件
### 组和权限管理
- 组的概念
  - 每个用户必须属于一个组
  - 每个文件有所有者、所在组、其它组
- 文件/目录所有者
  - 创建者一般是文件所有者
  - `ls -ahl`：查看所有者
  - `chown [user] [fileName]`：修改文件所有者
- 创建组
  - `groupadd [groupName]`：创建组
- 修改组
  - `chgrp [groupName] [fileName]`：修改文件所在组
- 其他组
  - 除文件所有者所在组的用户外，系统的其它用户都是文件的其它组
  - `usermod -g [groupName] [user]`：改变用户所在组
  - `usermod -d [dir] [user]`：改变用户登录的初始位置（用户需要目录的权限）
- 权限概念
  - 权限标识(-rwxrw-r-- 1 root root 12 Jul 8 06:02 sancho)：
    - 第0位:确定文件类型;l 是链接，d是目录，c是字符设备文件，b是块设备，-是普通文件
    - 第1~3位：确定文件所有者拥有的文件权限
    - 第4~6位：确定文件所属组拥有的文件权限
    - 第7~9位：确定其它用户拥有该文件的权限
  - 文件读写执行权限:
    - [r]：代表可读
    - [w]：代表可写（删除需要用户对目录有写权限）
    - [x]：可执行
  - 目录读写执行权限:
    - [r]：代表可读，查看目录内容
    - [w]：代表可修改，对目录内创建+删除+重命名目录
    - [x]：代表可执行，可进入该目录
  - 其它说明
    - [1]：表示硬链接数或子目录数
    - [root]：用户
    - [root]：组
    - [12]：文件大小（字节），文件夹显示4096字节
    - [Jul 8 06:02]：最后修改时间
    - [sancho]：文件名
- 修改权限
  - `chmod [dir|fileName]`：修改文件或目录权限
    - `a`:所有者，`g`:所有组，`o`其它人，`所有者`；`+`：增加，`-`：减少，`=`：赋予；eg:`chmod u=rwx,g=rx,o=x [dir|fileName]`
    - r=4,w=2,x=1,rwx=4+2+1=7，eg:`chomod 751 [dif|fileName]`
  - `chown [user] [dir|fileName]`：修改文件或目录所有者；`chown [user]:[group] [dir|fileName]`：修改文件或目录所有者和所在组
    - `-R`：递归生效
  - `chgrp [group] [dir/fileName]`：修改文件或目录所属组
     - `-R`：递归生效
### 定时任务调度
- corond：系统在某个时间执行的特定命令或程序
- `crontab [选项]`进入编辑页面后`[占位符] [命令]`
  - `-e`：编辑crontab定时任务
  - `-l`：查询crontab任务
  - `-r`：删除当前用户所有的crontab任务
  - `service crond restart`：重启任务调度
  - 第1位：一小时当中的第几分钟(0~59)
  - 第2位：一天当中的第几小时钟(0~23)
  - 第3位：一月当中的第几天(1~31)
  - 第4位：一年当中的第几月(1~12)
  - 第5位：一周当中的星期几(0~7)
  - 不指定的占位符需要`*`替代，每个占位符需要空格
    - eg:`*/1 * * * * ls -l /etc/ > /tmp/to.txt`
- 特殊符号说明
  - `*`：表示任何时间（第一个`*`表示一小时中每分钟执行一次）
  - ','：表示不连续的时间，eg:`0 8,12,16 * * *`：每天8:00，12:00，16:00都执行一次
  - '-'：表示连续的范围,eg:`0 5 * * 1-6`：周一到周六凌晨5:00执行命令
  - `*/n`：表示每隔多久执行一次，eg:`*/10 * * * *`：每隔十分钟执行一次
- at定时任务
  - `at [选项] [时间]`：一次性定时计划任务，守护进程atd以后台模式检查作业队列来运行，默认60秒检查一次
    - `-m`：当指定任务完成后，给用户发送邮件，即使没有标准输出
    - `-I`：atq别名，`atq`：查看系统中的工作任务
    - `-d`：atrm别名，`atrm`：删除指定编号的任务
    - `-v`：显示任务将执行的时间
    - `-c`：打印任务的内容到标准输出
    - `-V`：显示版本信息
    - `-q`：使用指定的队列
    - `-f`：从指定文件读入任务而不是从标准输入读入
    - `-t`：以时间参数的形式提交要运行的任务
    - `Ctrl+D`：输入两次退出命令编辑
  - at指定时间方法
    - 接收`hh:mm`形式时间，假如当日时间已过去，则第二天执行
    - 接收`midnight`：午夜,`noon`：中午，`teatime`：下午4点，等模糊词时间
    - 接收如`12pm`等am上午，pm下午等十二小时制时间
    - 接收`mon day`或`mm/dd/yy`或`dd.mm.yy`的具体日期格式（指定日期需要跟在时间后面，eg`04:00 2022-17-34`）
    - 接收相对计时法，`now + count time-units`，eg:`now + 5 minutes`
    - 接收`today`、`tomorrow`方式指定日期
### 磁盘分区、挂载
- `lsblk`或`lsblk -f`查看所有挂载情况
- 增加硬盘步骤：
  - 添加硬盘（并重启）
  - 分区：`fdisk /dev/[disk]`，[disk]为新硬盘，如`sdb`，后输入`n`新增分区，选择`p`分区类型为主分区，选择分区号默认为1，两次回车默认选择全部空间，最后输入`w`写入分区并退出，若不保存退出输入`q`
    - `m`：显示命令列表
    - `p`：显示磁盘分区，同`fdisk -l`
    - `n`：新增分区
    - `d`：删除分区
    - `w`：写入并退出
  - 格式化：`mkft -t ext4 /dev/[disk]`
  - 挂载：`mount /dev/[disk] [dir]`
    - 需要创建待挂载的目录：`mkdir [dir]`
    - 取消挂载：`umount /dev/[disk]|[dir]`，输入分区取消挂载，输入目录重新挂载到新目录（旧文件依然存在）
  - 设置自动挂载：`vim /etc/fstab`，添加`/dev/[disk] [dir] ext4 defaults 0 0`,`[Esc]`回到默认模式`:wq`保存退出，执行`mount -a`立即生效
- 磁盘情况查询
  - `df -h`：查询系统整体磁盘使用情况
  - `du -h [dir]`：查询指定目录磁盘占用情况，默认当前目录
    - `-s`：指定目录占用大小汇总
    - `-h`：带计量单位
    - `-a`：含文件
    - `--max-depth=1`：子目录深度
    - `-c`：列出明细的同时，增加汇总值
- 工作实用指令
 - 统计文件夹下文件个数：`ls -l [dir] | grep "^-" | wc -l`
 - 统计文件夹下目录的个数：`ls -l [dir] | grep "^d" | wc -l`
 - 统计文件夹下文件的个数，包括子文件夹：`ls -lR [dir] | grep "^-" | wc -l`
 - 统计文件夹下的目录个数，包括子文件夹：`ls -lR [dir] | grep "^d" | wc -l`
 - 以树状显示目录结构：`tree [dir]`
   - 可能需要：`yun install tree`安装tree命令
### 网络配置
- 网络环境配置
  - `ipconfig`：查看网络地址
  - `ping [add]`：测试连接通畅性
  - 设置指定ip：`vi /etc/sysconfig/network-scripts/ifcfg-ens33`
    - `ifcfg-ens33`文件说明：
      - `DEVICE=eth10`：接口名（设备、网卡）
      - `HWADDR=00:0C:2x:6x:0x:xx`：MAC地址
      - `TYPE=Ethernet`：网络类型，通常是Ethernet
      - `UUID=9031ad3-dsaalj21`：随机ID
      - `ONBOOT=yes`：系统启动时网络接口是否有效
      - `BOOTPROTO=static`：IP的配置方法[none|staic|bootp|dhcp]（引导时不使用协议|静态分配IP|BOOTP协议|DHCP协议）
      - `IPADDR=192.168.200.130`：IP地址
      - `GATEWAY=APADDR=192.168.200.2`：网关
      - `DNS1=192.168.200.2`：域名解析器
  - 重启网络服务:`service network restart`
- 设置主机名和hosts映射
  - 查看主机名：`hostname`
  - 修改主机名：`vim etc/hostname`，修改保存后重启
### 进程管理
- 显示系统执行的进程
 - `ps [选项]`：显示进程。
   - `-a` ：显示当前终端的所有进程消息
   - `-u` ：以用户的格式显示进程信息
   - `-x` ：显示后台进程运行的参数
   - `-e` ：显示所有进程
   - `-f` ：全格式
 - 进程详细参数
   - UID：用户ID
   - PID：进程识别号
   - PPID：父进程ID
   - C：优先级因子，数值越大CPU运算越密集，执行优先级越低；数值越小I/O运算越密集，执行优先级越高
   - %CPU：占CPU百分比
   - %MEN：占用物理内存百分比
   - VSZ：虚拟内存大小
   - RSS：物理内存大小
   - TTY：终端机号
   - STAT：状态（S=sleep，N=低优先级，R=running，D=等待，Z=僵死，T=被跟踪或停止等）
   - STARTED：进程启动时间
   - TIME：占用CPU时间
   - COMMAND正在执行的命令或进程
 - 终止进程
   - `kill [选项] [pid]`：通过pid终止进程
     - `-9`：强制终止
   - `killall [进程名]`：通过进程名终止进程及其子进程，支持通配符
     - `-9`：强制终止
- 查看进程树
  - `pstree [选项]`:查看树状进程信息
    - `-p`：显示进程的PID
    - `-u`：显示进程的所属用户
- 服务管理
  - `service [服务名] [start|stop|restart|reload|status]`：启动服务
    - Centos7.0后很多服务不再使用service而是systemctl
    - service指令管理的服务在/etc/init.d查看
  - 使用`setup`命令→系统服务，可以查看全部服务或`ls -l /etc/init.d`
  - `chkconfig [选项] [服务名] [on|off]`:给服务的各个运行级别设置自启动|关闭,需要重启
    - `--list`：以列表显示
    - `--level [5]`：只查看level [num]的运行级别
  - `systemctl [选项] [start|stop|restart|status] [服务名]`
    - systemctl管理的服务在/usr/lib/systemd/system查看
    - `list-unit-file`：查看服务关机启动状态
    - `enable`：设置服务开机启动
    - `disable`：关闭服务开机启动
    - `is-enabled`：查询某个服务是否是自启动
  - firewall指令
    - 打开端口：`firewall-cmd -- permanent --add-port=[端口号/协议]`
    - 关闭端口：`firewall-cmd -- permanent --remove-port=[端口号/协议]`
    - 重新载入：`firewall-cmd --reload`
    - 查询端口：`firewall-cmd --query-port=[端口号/协议]`
- 动态监控进程
  - `top [选项]`：动态显示正在执行的进程
    - `-d [秒数]`：指定多少秒更新，默认3秒
    - `-i`：不显示闲置或僵死进程
    - `-p`：通过监控进程ID来仅仅监控某个进程的状态
  - 交互操作：
    - `P`：以CPU使用率排序（默认）
    - `M`：以内存使用率排序
    - `N`：以PID排序
    - `q`：退出top
    - `u [user]`：跟踪用户进程
    - `k [pid]`：终止指定进程
- 监控网络状态
  - `netstat [选项]`：查看系统网络状况
    - `-an`:按一定顺序排列输出
    - `-p`：显示哪个进程在调用
### RPM和YUM
- `rpm [选项]`
  - `-qa`：查询所有已安装程序
  - `-q [appName]`:查询app是否安装
  - `-qi`：查询软件包信息
  - `-ql`：查询软件包文件
  - `-qf [dir]`：查询文件所属软件包
  - `-e [appName]`：卸载rpm包
    - `-e --nodeps [appName]`：强制卸载
  - `-ivh [dir]`：安装RPM包，i=安装，v=提示，h=进度条
- `yum [选项] [appName]`：基于RPM，自动下载且自动下载依赖
  - `list`：查看所有可安装的包
  - `install [appName]`：下载指定包
### !shell编程
### Python相关
- 下载ubuntu
  - [官网下载](https://cn.ubuntu.com/download)
  - !中文支持
  - 新机注意需要先`sudo passwd`设置root初始密码
- APT软件管理
  - 常用命令：
    - `sudo apt-get update`：更新源
    - `sudo apt-get install [appName]`：安装包
    - `sudo apt-get remove [appName]`：卸载包
    - `sudo apt-cache show [appName]`：获取包相关信息
    - `sudo apt-get source [appName]`：下载该包源代码
  - 镜像源：
    - apt地址：/etc/apt/sources.list
    - 清华大学镜像地址：进入[官网](https://mirrors.tuna.tsinghua.edu.cn/)，选择系统和版本号，复制内容，备份源地址`sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup`,粘贴到文件
- 远程登录
  - 安装SSH服务：`sudo apt-get install openssh-server`
  - 启用ssh服务：`service sshd restart`
  - 连接另一台linux：`ssh [user]@[ip]`
    - 退出命令:`exit`或`logout`
