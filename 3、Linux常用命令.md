# Linux 常用命令

[TOC]



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

语法：`usermod -aG 用户组 用户名`

- 选项：`-a` 表示附加，如省略则会覆盖用户的现有组成员。
- 选项：`-G` 用于指定用户添加到的组列表。

>用户信息存储在 `/etc/passwd` 文件中，用户密码信息存储在`/etc/shadow`文件中。

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













