# Linux 常用命令

[TOC]

文档：[link](http://c.biancheng.net/view/705.html)

## su 命令

功能：切换用户

语法：`su [-] [用户]`

- `-` 代表切换用户后加载环境变量（切换到用户吱声的工作目录），建议带上。
- 用户可以省略，省略默认切换到`root`。

>`exit` 命令用于退出终端窗口或退出当前`shell`。

## id 命令

功能：查询用户的`UID`、`GID`和附加组的信息。

语法: `id 用户名`

```bash
[root@localhost home]# id zjh
# UID | GID | 用户所在组（包括初始组和附加组）
# "wheel" 是一个特殊组，通常用于授予系统管理员权限。
> uid=1000(zjh) gid=1000(zjh) groups=1000(zjh),10(wheel)

```



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

简介：`Linux` 许多软件（内置或第三方）均支持使用 `systemctl` 命令控制：**启动、停止、开机自启**，能够被 `systemctl` 管理的软件。一般也称为 **服务**。

系统内置的服务较多，如下：

- `NetworkManager`，主网络服务
- `network`，副网络服务
- `firewalld`，防火墙服务
- `sshd,ssh`服务（远程登录服务）

功能：控制系统服务的操作。

语法：`systemctl start | stop | restart | disable | enable | status 服务名`

- 参数：`start`，启动服务。
- 参数：`stop`，停止服务。
- 参数：`restart`，重启服务。
- 参数：`status`，查看服务状态。
- 参数：`enable`，启动开机自启。
- 参数：`disable`，关闭开机自启。







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


```













