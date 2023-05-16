# Linux 常用命令

[TOC]

文档：[link](http://c.biancheng.net/view/705.html)

## su 命令

功能：切换用户

语法：`su [-] [用户]`

- `-` 代表切换用户后加载环境变量（切换到用户吱声的工作目录），建议带上。
- 用户可以省略，省略默认切换到`root`。

>`exit` 命令用于退出终端窗口或退出当前`shell`。



<br />



## id 命令

功能：查询用户的`UID`、`GID`和附加组的信息。

语法: `id 用户名`

```bash
[root@localhost home]# id zjh
# UID | GID | 用户所在组（包括初始组和附加组）
# "wheel" 是一个特殊组，通常用于授予系统管理员权限。
> uid=1000(zjh) gid=1000(zjh) groups=1000(zjh),10(wheel)

```



<br />



## sudo 命令

功能：授予普通用户，临时以`root`身份执行。

语法：`sudo 其他命令`

- 可让一条普通命令带有`root`权限。

- 普通用户配置`sudo`命令的执行权限，则用户不每次使用`sudo`时都需输入密码（为普通用户配置sudo认证）。

  ```bash
  # 用于编辑/etc/sudoers文件，指定哪些用户和用户组可以以超级用户（root）身份运行命令
  sudo visudo 
  
  > zjh ALL=(ALL) NOPASSWD:ALL
  ```



<br />



## systemctl 命令

简介：`Linux` 许多软件（内置或第三方）均支持使用 `systemctl` 命令控制：**启动、停止、开机自启**，能够被 `systemctl` 管理的软件。一般也称为 **服务**。系统内置的服务较多，如下：

- `NetworkManager`，主网络服务
- `network`，副网络服务
- `firewalld`，防火墙服务
- `sshd,ssh`服务（远程登录服务）

功能：控制系统服务的操作。

语法：`systemctl start | stop | restart | disable | enable | status 服务名`

- 参数：`start`，启动服务
- 参数：`stop`，停止服务
- 参数：`restart`，重启服务
- 参数：`status`，查看服务状态
- 参数：`enable`，启动开机自启
- 参数：`disable`，关闭开机自启



<br />



## 软链接和硬链接

### ln -s 命令

功能：创建文件、文件夹 **软链接（类似于 Windows 快捷方式）**

语法：`ln -s 源文件 目标文件`

- 选项：`-s`，建立软链接文件
- 参数1：被链接的源地址
- 参数2：要链接去的地方（快捷方式的名称和存放位置）

>**软链接 ** 的特点：
>
>- 软链接文件，使用`ls -l` 查看文件类型，可发现第一个字母为 `l`，代表是软链接文件。
>- 如果 **源文件** 被删除，则无法使用快捷方式。但一旦以同样名称创建了源文件，则会指向重新创建的源文件。

### ln 命令

功能：创建文件 **硬链接（类似于源文件的别名，两个名字指向同一个文件数据）**

语法：`ln 源文件 目标文件`

- 参数1：被链接的源地址
- 参数2：要链接去的地方（硬链接的名称和存放位置）

![image-20230516143007986](https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202305161430548.png)

>**硬链接** 的特点：
>
>- 硬链接只对文件有效，不能对目录做硬链接。
>- 源文件被删除，不影响另外一个文件。
>- 硬链接不限于两个文件之间，可在多个文件之间进行，使用`ls -l` 可以查看文件的硬链接数。
>- 不能在不同的文件系统之间做硬链接（Linux的文件系统：`ext4、xfs`等）



<br />



## 日期

### date 命令

功能：显示当前的日期和时间，也可用于日期计算。

语法：`date [-d] [+日期格式]`

- 选项：`-d` 按照给定的字符串显示日期，一般用于日期计算（可与格式化字符串配合使用）。

- 参数：

  | 日期格式 |                  解释                   |
  | :------: | :-------------------------------------: |
  |    %Y    |                   年                    |
  |    %y    |             年份后两位数字              |
  |    %m    |                   月                    |
  |    %d    |                   日                    |
  |    %H    |                  小时                   |
  |    %M    |                  分钟                   |
  |    %S    |                  秒数                   |
  |    %s    | 自 `1970-01-01 00:00:00 UTC` 到现在的秒 |

演示：

```bash
# 按照 2022-01-01 格式显示日期
[zjh@localhost ~]$ date +%Y-%m-%d
> 2023-05-16
# 按照 2022-01-01 10:00:00 格式显示日期
[zjh@localhost ~]$ date "+%Y-%m-%d %H:%M:%S"   # 加双引号，因为有空格
> 2023-05-16 02:17:44
```

### 时区

通过 `date` 查看日期时间发现时间是不准确的，这是因为系统默认时区为`UTC(标准时间)`，而中国的时区为 `CST(东八区时间)`，这两个时区实际相差 `8` 小时。

**修改时区需要使用 `root` 权限，下方修改为东八区时区：**

```bash
# 系统时区由 /etc/localtime 文件控制（删除默认时区）
[root@localhost ~]# rm -f
# 创建软链接，修改时区为东八区。   zoneinfo:时区信息
[root@localhost ~]# ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

### NTP 时间服务器

在 `Linux` 系统中，一般使用 **网络时间协议（NTP）** 来自动校准时间服务，`NTP` 是一种用于在计算机网络中同步时钟的协议，通过多个时间服务器提供准确的时间信息，以帮助计算机自动校准时钟。

在计算机系统中，时间被用来记录事件的发生时间，比如文件的创建和修改时间，进程的启动和结束时间，以及网络通信的时间戳等等。如果时钟不准确，就会导致这些事件的时间记录错误，从而影响系统的正常运行和数据的可靠性。

`Linux` 有多种 `NTP` 客户端程序可供选择，常见的客户端为 `ntpd` 和 `chrony`。历史上，`ntpd` 是常见的 `NTP` 客户端程序，但是随着发展 `chrony` 逐渐成为更流行的 `NTP` 客户端解决方案。

### chrony 命令

功能：同步时间

安装：`yum install -y chrony`

启动管理：`systemctl start | stop | restart | status | disable | enable chronyd`

配置校准：只需手动配置一次，具体流程如下

```bash
# 安装 NTP 客户端（chrony）
sudo yum -y install chrony
# 编辑配置文件
vim /etc/chrony.conf
> pool ntp.aliyun.com iburst # 指定为阿里云 NTP 服务器
# 重启 chronyd 服务
systemctl restart chronyd
# 设置开机自启动
systemctl enable chronyd
# 查看时间同步源
chronyc sources -v 
> ^? 203.107.6.88                  2   6     3     1  -2375us[-2375us] +/-   31ms
```

>`chrony` 是一个开源软件，是一个原来 **维持计算机系统时钟的准确性** 的程序，默认配置文件在 `/etc/chrony.conf`。它能保持系统时间与时间服务器(NTP)同步，让时间始终保持同步。
>
>`chronyd` 是一个系统后台运行的守护进程，他根据网络上其他时间服务器时间来测量本机时间的偏移量从而调整系统时钟。
>
>`chronyc` 提供一个用户界面，用于监控性能并进行多样化的配置。用来监控chronyd性能和配置其参数的用户界面，也可以在一台不同的远程计算机上工作。
>
>相对于 `ntpd`，`chrony` 具有如下优点：
>
>1. 更快的初始同步：`chrony` 可在几秒内实现初始时钟同步，而 `ntpd` 需几分钟或更长时间才能完成。
>2. 更准确和稳定的时间同步：`chrony`可通过时钟源选择、时钟滤波和时钟融合等技术。
>3. 更少的系统资源占用：`chrony` 占用的系统资源较少，可在较低的系统要求下运行，适合 **嵌入式设备** 和 **低功耗系统** 等场景。
>















<br />



## 软件安装

### yum 命令

功能：在 `CentOS` 系统中安装、卸载、搜索 软件包。

语法：`yum [-y] [install remove search] 软件包名 `

- 选项 ：`-y` 安装或卸载软件包时，自动确认。
- 参数：`install` 安装软件包。
- 参数：`remove` 卸载软件包。
- 参数：`search` 搜索软件包。

### apt 命令

功能：在 `Ubantu` 系统中安装、卸载、搜索 软件包。

语法：`apt [-y] [install remove search] 软件包名`

- 选项 ：`-y` 安装或卸载软件包时，自动确认。
- 参数：`install` 安装软件包。
- 参数：`remove` 卸载软件包。
- 参数：`search` 搜索软件包。

>`yum` 和 `apt` 命令都需要 `root`权限，因为安装软件包需对系统进行修改，如将文件复制到系统、创建用户和组、修改配置文件等。



<br />



## IP 地址

`IP` 地址是联网计算机的网络地址，用于在网络中进行定位。

格式：`a.b.c.d`

- `abcd` 为 `0~255`的数字，如`192.168.50.80`。

特殊`IP`：

- `127.0.0.1`:

  - 表示本机地址。

- `0.0.0.0`:

  - 可表示本机地址.
  - 在一些IP地址限制中,表示所有`IP`的意思,如放行规则设置为`0.0.0.0`,表示允许任意`IP`访问。

查看`IP`：`ifconfig`

- 如无法使用 `ifconfig` 命令，安装 `yum -y install net-tools`

  

<br />



## 主机名

功能：`Linux` 系统的名称（其他计算机可访问主机名来访问该计算机）。

查看：`hostname`

设置：`hostnamectl set-hostname 主机名`

扩展：`hostnamectl status` 显示当前主机的主机名和相关的系统信息。



<br />



## 域名解析

通过 **主机名** 找到对应计算机的 `IP`地址，称为 **主机名映射（域名解析）**。

访问 `www.baidu.com` 流程如下：

![image-20230516152001115](https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202305161520148.png)

> 通过 **系统本地记录** 去查找，如果找不到就联网去 `公开DNS服务器` 去查找。
>
> **自行配置映射：**
>
> ```bash
> # 打开 C:\Windows\System32\drivers\etc\hosts 添加映射(管理员身份)
> > 192.168.126.80 centos
> 
> # 打开终端，ping centos 或 使用主机名远程登陆
> ping centos
> > Pinging centos [192.168.126.80] with 32 bytes of data:
> > Reply from 192.168.126.80: bytes=32 time<1ms TTL=64
> > Reply from 192.168.126.80: bytes=32 time=2ms TTL=64
> 
> ssh root@centos
> > root@centos's password:
> > Last login: Tue May 16 15:21:24 2023 from 192.168.126.1
> > [root@centos ~]# 成功登陆
> ```



<br />



## 配置 VMware 固定IP

### CentOS 配置 IP

1. 修改 `VMware` 网络，流程如下

   - 编辑 — 虚拟网络编辑器 — `VMnet8` — 更改设置 — `NAT` 模式 — 设置子网`IP` 和子网掩码 — `NAT` 设置 — 网关`IP`

2. 设置 `Linux` 内部固定 `IP`

   - 修改文件：` /etc/sysconfig/network-scripts/ifcfg-ens33`，示例内容如下：

     ```bash
     TYPE=Ethernet
     PROXY_METHOD=none
     BROWSER_ONLY=no
     BOOTPROTO=static # 默认为DHCP，改为static，固定IP。
     DEFROUTE=yes
     IPV4_FAILURE_FATAL=no
     IPV6INIT=yes
     IPV6_AUTOCONF=yes
     IPV6_DEFROUTE=yes
     IPV6_FAILURE_FATAL=no
     IPV6_ADDR_GEN_MODE=stable-privacy
     NAME=ens33
     UUID=655b3e86-ccb9-4ceb-8e91-d4ca7112e6b7
     DEVICE=ens33
     ONBOOT=yes
     IPADDR=192.168.31.66 # IP地址，要匹配网络范围。
     NETMASK=255.255.255.0 # 子网掩码，前三位为网络地址，后一位为主机地址。
     GATEWAY=192.168.31.2 # 网关，与VMware配置一致。.1已被Windows占用，这里避免冲突设置为.2
     DNS1=192.168.31.2 # 主DNS
     DNS2=8.8.8.8 # 备用DNS
     ```

### Ubantu 配置 IP

`Jetson nano` 或 树莓派配置 `wlan0` 固定 `IP` 地址，具体过程如下：

```bash
# 首先切换到 root用户
su - root

# 编辑网络配置文件（该文件包含了网络接口的配置信息，如IP地址、子网掩码、网关、DNS服务器）
vim /etc/network/interfaces
> auto wlan0 # 自启动wlan0网络
> iface wlan0 inet static # 设置静态IP
> address 192.168.31.80 # IP地址
> netmask 255.255.255.0 # 子网掩码
> gateway 192.168.31.1 # 网关
> dns-nameservers 8.8.8.8 # dns服务器：解析域名。
> wpa-ssid "608" # wifi名称
> wpa-psk "20030716" # wifi密码

# 重启即生效
reboot now

```



<br />



## wget 命令

简介：`wget` 是一个非交互的文件下载器，可从命令行内下载网络文件。支持 `HTTP`、`HTTPS`、`FTP` 三个常见的 `TCP/IP`协议下载，并可使用  `HTTP` 代理。

功能：文件下载器，可下载网络文件。

语法：`wget [-b] url`

- 选项：`-b` 后台执行下载任务，会将日志写入当前工作目录的 `wget-log` 文件。
- 参数：`url` 下载链接。

实例：

```bash
# 1.下载网络文件
wget http://archive.apache.org/dist/hadoop/common/hadoop-3.3.0/hadoop-3.3.0.tar.gz # ctr l+ c：可停止下载程序

# 2.后台下载
wget -b http://archive.apache.org/dist/hadoop/common/hadoop-3.3.0/hadoop-3.3.0.tar.gz
> Continuing in background, pid 3469.
> Output will be written to ‘wget-log’. # 输出写入至 wget-log日志文件
tail -f wget-log # 持续跟踪过程
```



<br />



## curl 命令

简介：`curl` 用于发送 `http` 网络请求。

功能：获取网页信息、下载文件。

语法：`curl [-O] url`

- 选项：`-O`，用于下载文件，当 `url` 是下载链接时，使用此选项会保存文件。
- 参数：`url`，要发起请求的网络地址。

实例：

```bash
# 1. 向cip.cc发起网络请求（cip.cc：一个公开的网站，可以帮我们获取主机的公网IP地址）
curl cip.cc
> IP      : 223.104.77.175 # 公网IP
> 地址    : 中国  中国 
> 数据二  : 中国 | 移动数据上网公共出口
> 数据三  : 中国广东省东莞市 | 移动
> URL     : http://www.cip.cc/223.104.77.175

# 2. 向 www.baidu.com 发起网络请求(获取了请求，得到了网页代码，但是需要浏览器进行渲染才能显示)
curl www.baidu.com
> <!DOCTYPE html>
> <!--STATUS OK--><html> <head><meta http-equiv=content-type content=text/html;charset=utf-8><meta http-> > > equiv=X-UA-Compatible content=IE=Edge><meta content=always name=referrer><link rel=styleshee
> type=text/css href=http://s1.bdstatic.com/r/www/cache/bdorz/baidu.min.css><title>百度一下，你就知
> ...........

# 3. curl -O 下载安装包
curl -O http://archive.apache.org/dist/hadoop/common/hadoop-3.3.0/hadoop-3.3.0.tar.gz
> % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
>                                  Dload  Upload   Total   Spent    Left  Speed
> 0  477M    0 10787    0     0  17345      0  8:01:09 --:--:--  8:01:09 17342
```















<br />



## 权限管理

### 权限信息

<img src="https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202305081339154.png" alt="image-20230506210816900" style="zoom: 67%;" />

### chmod 命令

功能：**修改**文件、目录的**权限位**信息。

注意：命令只有文件、文件夹的**所属用户**或 `root` 用户可以修改。

语法：`chown [-R] 权限 文件或目录`

- 选项：`-R`，递归更改目录及所有子目录和文件的权限。

  ```bash
  [zjh@localhost ~]$ chmod 755 opencv.py
  > -rwxr-xr-x. 1 zjh zjh 7 May  7 04:02 opencv.py
  
  ```

### chown 命令

功能：**修改**文件、目录的 **所属用户、用户组** 信息。

注意：命令只适用于 `root` 用户执行。

语法：`chown [-R] [用户]/[:用户组] 文件或文件夹 `

- 选项：`-R` ，递归更改指定文件或目录的 **文件拥有者** 和 **文件所属组** 信息。

  ```bash
  # 修改文件拥有者
  [root@localhost zjh]# chown root opencv.py
  > -rwxr-xr-x. 1 root zjh 7 May  7 04:02 opencv.py
  
  # 修改文件所属组
  [root@localhost zjh]# chown :root opencv.py
  > -rwxr-xr-x. 1 root root 7 May  7 04:02 opencv.py
  ```




<br />



## 用户管理

以下命令需 `root` 用户执行。

### useradd 命令

功能： 创建用户

语法：`useradd [-g -u -d] 用户名`

- 选项：`-g`指定用户的组，使用 `-g` 不指定则创建同名组并自动加入。
- 选项：`-u` 手动指定用户的`UID`。
- 选项：`-d` 手动指定用户的主目录，且主目录必须为绝对路径。

### userdel 命令

功能：删除用户

语法：`userdel [-r] 用户名`

- 选项：`-r` 表示删除用户的同时删除用户的家目录。

### usermod 命令

功能：将用户添加到一个或多个附加组，不覆盖当前主组。

语法：`usermod -aGu 用户组 用户名`

- 选项：`-a` 表示附加，如省略则会覆盖用户的现有组成员。
- 选项：`-G` 用于指定用户添加到的组列表。
- 选项：`-u` 用于修改用户的UID。

>用户信息存储在 `/etc/passwd` 文件中，用户密码信息存储在`/etc/shadow`文件中。



<br />



## 用户组管理

以下命令需 `root` 用户执行。

### groupadd 命令

功能：创建用户组

语法：`groupadd [-g] 用户组名`

- 选项：`-g`表示指定`GID`。

### groupdel 命令

功能：删除用户组

语法：`groupdel 组名`

>组信息存储在 `/etc/group` 文件中，组密码信息存储在 `/etc/gshadow` 文件中。



<br />



## 常用快捷键

### ctrl + c 强制停止

`Linux` 某些程序运行，如果想要强制停止它，则需使用快捷键`ctrl + c`。

### ctrl + d 退出或登出

`Linux`系统中，想退出当前账户或某些特定程序的页面，则需使用快捷键`ctrl +d`。

### ctrl + l 清空内容

`Linux`系统中，通过命令`clear` 或 `ctrl + l` 清空终端内容。

### history 命令

`Linux`系统中，如果想查看历史输入的命令，则使用`history` 命令。结合`grep` 可进行过滤。

### 光标移动快捷键

- `ctrl + a`，跳到命令开头。
- `ctrl + e`，跳到命令结尾。
- `ctrl + 键盘左键`，向左跳一个单词。
- `ctrl + 键盘右键`，向右跳一个单词。



































## 计算机英语

```bash

# Permission denied（没有权限）
> Permission: 许可，权限
> denied: 拒绝

# No such file or directory（没有这样的文件和目录）
> such: 如此的，这等的
> file: 文件
> director: 目录

# disable enable
> able：有能力
> disable：使丧失能力的（Linux中为不启动开启自启）
> enable: 使能够，启动（Linux中为启动开机自启）

# 一周的所有英语单词
> Monday(星期一)
> Tuesday（星期二）
> Wednesday(星期三)
> Thursday（星期四）
> Friday(星期五)
> Saturday（星期六）
> Sunday（星期天）

```













