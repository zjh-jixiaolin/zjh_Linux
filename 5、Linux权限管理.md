# Linux 权限管理

[TOC]

文档：[link](http://c.biancheng.net/view/705.html)

## 0. 权限管理

所谓权限管理，其实是指对不同用户，设置不同文件的访问权限，包括对文件的 **读、写、执行** 等。

在 `Linux` 系统中，每个用户都具有不同权限，对于普通用户来说，它们只能在自己的主目录才具有 **写** 和 **执行** 的权限，而主目录外，只具有 **访问** 和 **读** 权限。

与 `Windoes` 系统不同，`Linux` 为每个文件添加了很多属性，最大的作用是 **维护数据的安全**。

>除自身主目录外，普通用户也有对主目录外文件拥有 **写** 权限，如为目录的所有者或属于该目录的组。
>
>`root` 用户拥有最高权限，可以修改任何文件的权限，而普通用户智能修改自己文件的权限。

### 权限信息

---

使用 `ls -l ` 以列表的形式列出目录或文件的详细信息，并显示权限细节。如文件或目录的 **权限、所有者、所属组、大小、时间戳和名称**等信息。

![image-20230506180051618](https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202305061800682.png)



### 权限位

---

在 `Linux` 系统中，常见的文件权限有 `3` 种，即对文件的 **读 、写、执行** 权限。权限细节总分为 `10` 个单位，具体如下：

![image-20230506180152938](https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202305061947342.png)

针对文件，文件夹的不同，`rwx` 的含义有细微差别：

- `r ` — **读** 权限，数字 `4` 表示：
  - 针对文件，可以查看文件内容。
  - 针对文件夹，可以查看文件夹内容，无`r`权限无法查看文件夹内容，如`ls`命令一样。
- `w` — **写** 权限，数字 `2` 表示：
  - 针对文件，可 **修改** 此文件。
  - 针对文件夹，可 **创建、删除、改名** 等操作。
- `x` — **执行** 权限，数字 `1` 表示：
  - 针对文件，可将文件作为程序执行。
  - 针对文件夹，可更改工作目录到此文件夹，无`x`权限则无法到此文件夹，即 `cd` 进入。

>举例 `drwxr-xr-x`，表示如下：
>
>- 首字母 `d` 表示为 **文件夹**。
>- 所属用户的权限有：`r`— 读 、`w` — 写、`x` — 执行。
>- 所属用户组的权限有：`r`—读、`-`—无写入权限、`x`—执行。
>- 其他用户的权限有，`r`—读、`-`—无写入权限、`x`—执行。

>**案例**：假设账号名称为 `zjh` ，它的家目录在`/home/zjh`，`zjh` 对此目录具有 `rwx` 权限。若在此目录有个名为 `opencv.py` 的文件，该文件的权限如下:
>
>```bash
>[zjh@localhost ~]$ ll
>> -rw-r-----. 1 root root 7 May  7 04:02 opencv.py
>```
>
>**问题：** `zjh` 对此文件的权限是什么? 是否可删除文件 ?
>
>**答案：** 账户 `zjh` 对此文件为其他人，因此无法读取、编辑和执行。但是由于这个文件在 `zjh`账户的目录下，因此具有 `rwx`的完整权限，因此可对`opencv.py` 文件进行删除。



<br />



## 1. 默认权限

`Linux` 是注重安全性的操作系统，而安全的基础在于对权限的设定，不仅是已存在的文件或目录要设定必要的访问权限，创建 **新文件或目录** 时，也要设定必要的 **初始权限**。

### 默认权限 — 设定

`Windows` 系统中，新建文件或目录时，通过继承上级目录的权限获得初始权限，而`Linux`不同，它是通过使用 `umask` 默认权限给所有新建文件或目录赋予 **初始权限**。通过以下命令查看 umask默认权限：

```bash
# root用户默认为0022,普通用户默认为0002
[root@localhost ~]# umask
> 0022

[zjh@localhost ~]$ umask
> 0002

```

如上所示使用`root`用户举例，`umask` 默认权限由 `4` 个八进制数组成，第 `1` 个数代表的是文件的 **特殊权限（SetUID、SetGID、Sticky BIT）**，后 `3` 位数为 `022` 是文件的 **普通权限**。转换为字母形式为`-----w--w-`。

> **注意**：虽然 `umask` 默认权限是用来设定文件或目录的 **初始权限**，但并不是直接将 `umask` 默认权限作为文件或目录的初始权限，还需对其进行计算。

文件和目录的 **真正初始权限**，通过如下格式计算可得：

```bash
真实初始权限 = 文件/目录的最大默认权限 - umask权限
```

如上计算公式，可知如果想最终得到文件或目录的 **初始权限值**，还需了解文件或目录的 **最大默认权限**，在 `Linux` 系统中，文件或目录的 **最大默认权限** 是不同的：

- 对 **文件** 来讲，其拥有的 **最大默认权限** 是 `666`，即 `rw-rw-rw-`。也就是说，使用文件的任何用户都没有 **执行（x）** 权限。因为 **执行** 权限是文件的最高权限，不能默认被赋予，需通过用户手工赋予。
- 对 **目录** 来讲，其拥有的 **最大默认权限** 是 `777`，即 `rwxrwxrwx`。

通过 **字母权限** 的方式来计算机文件或目录的初始权限，以 `umask` 值为 `022` 为例，分别计算新建文件和目录的初始权限：

- **文件** 的 **最大默认权限**是`666`，换算为字母的形式为 `-rw-rw-rw`。`umask`值为`022`，换算为字母为`-----w--w-`。通过公式 **文件最大默认权限 - umask权限= 初始权限**，即`(-rw-rw-rw-) - (-----w--w-) = (-rw-r--r--)`，测试如下：

  ```bash
  # root用户新建文件后的初始权限
  [root@localhost ~]# umask
  > 0022
  [root@localhost ~]# touch test.txt
  [root@localhost ~]# ll test.txt
  > -rw-r--r--. 1 root root 0 May  8 23:46 test.txt
  
  ```

- **目录** 的 **最大默认权限**是`777`，换算为字母的形式为 `drwxrwxrwx`。`umask`值为`022`，换算为字母为`-----w--w-`。通过公式 **目录最大默认权限 - umask权限= 初始权限**，即`(drwxrwxrwx) - (-----w--w-) = (drwxr-xr-x)`，测试如下：

  ```bash
  # root用户新建目录后的初始权限
  [root@localhost ~]# umask
  > 0022
  [root@localhost ~]# mkdir test
  [root@localhost ~]# ll -d test
  > drwxr-xr-x. 2 root root 6 May  8 23:51 test
  
  ```



### 默认权限 — 修改

`umask` 权限值可通过如下命令直接修改：

```bash
# umask初始权限
[root@localhost ~]# umask
> 0022
# 修改umask默认权限
[root@localhost ~]# umask 555
[root@localhost ~]# umask
> 0555
```

以上通过命令方式修改的 `umask` 只临时有效，**重启即失效**，恢复原来的`0022`。如想要 **永久生效**，则需修改对应的环境变量配置文件 `/etc/profile`。具体如下：

```bash
[root@localhost ~]# vim /etc/profile
> ...
> if [ $UID -gt 199 ] && [ "`/usr/bin/id -gn`" = "`/usr/bin/id -un`" ]; then
>       # 当UID大于199（普通用户），则使用此umask的值
>       umask 002
> else
>	    # 当UID小于199（超级用户），则使用此umask的值
>       umask 022
> fi
> ...
```

>在`Linux`系统中，`/etc/profile` 文件包含了系统全局环境变量 和 `Shell` 函数。



<br />

## 2. 特殊权限位

### SUID — 用户 ID 权限

---

在 `Linux` 系统中，除了`rwx` 权限外，还有 `s` 权限，当 `s` 权限位位于 **文件所有者** 的`x` 权限位时，权限简称为 `SUID` ，如下所示：

```bash
[root@localhost ~]# ls -l /usr/bin/passwd
> -rwsr-xr-x. 1 root root 27856 Apr  1  2020 /usr/bin/passwd

```

如上观察，可知原本表示 **文件所有者** 权限中的 `x` 权限位，却成了 `s` 权限，权限简称 `SetUID`，简称`SUID 特殊权限`。`SUID` 特殊权限仅适用于 **可执行文件**。

功能：用户对设有 `SUID` 的文件有**执行**权限，当用户执行此文件时，会以 **文件所有者** 的身份去执行文件，一旦文件执行结束，身份的切换也随之消失。

**案例：** 在 `Linux` 系统中所有用户的密码数据都存储在 `/etc/shadow` 文件中，通过 `ls -l /etc/shadow` 命令可知，此文件的权限为 `0(---------)`，由此可知，**普通用户**对此文件没有任何权限。

那为什么 **普通用户** 可使用 `passwd`命令修改自己的密码？

如上代码块所示，`passwd`命令的权限配置，可知，此命令拥有 `SUID` 特殊权限，且 **其他人** 对此文件也有 **执行** 权限，这就意味着，任何一个用户都可以用 **文件所有者（root）** 身份执行 `passwd` 命令。

**总结：**  当 **普通用户** 使用 `passwd` 命令更改自己的密码时，实际是以 `root` 用户执行 `passwd`命令，因为 `root` 可将密码写入 `/etc/shadow` 文件，只不过命令一旦执行完成，普通用户具有的 `root` 身份也随之消失。

>`Linux` 系统中，大多数命令的文件所有者默认为 `root`。

`SUID` 特殊权限具有如下特点：

- 只有 **可执行文件** 才能设定 `SetUID` 权限，对目录设定 `SUID` 是无效的。
- 用户要对该执行文件拥有 `x（执行）` 权限。（其他用户要有 `x` 权限）
- 用户在执行该文件时，会以 **文件所有者** 的身份执行。
- `SetUID` 权限只在文件执行过程中有效，执行完毕后，身份的切换也随之消失。

### SGID — 组 ID 权限

---

在 `Linux` 系统中，当 `s` 权限位位于 **所属组** 的 `x`权限位时，权限简称为 `SGID`，如下所示：

```bash
[root@localhost ~]# ll /usr/bin/write
> -rwxr-sr-x. 1 root tty 19544 Feb  3  2021 /usr/bin/write

```

与 `SUID` 不同的是，`SGID` 即可对文件进行配置，也可对目录进行配置。

#### SGID 对文件的作用

对于 **文件** 来说，`SGID` 具有如下特点：

- `SGID` 只针对 **可执行文件** 有效，换句话说，只有 **可执行文件** 才可被赋予 `SGID` 权限，普通文件赋予 `SGID` 无意义。
- 用户需对此 **可执行文件** 有 `x` 权限。（其他用户要有 `x` 权限）
- 用户执行带有 `SGID` 权限的 **可执行文件** 时，用户的 **群组身份** 会变为 **文件所属群组**；
- `SGID` 权限赋予**用户改变组身份**的效果，只在 **可执行文件** 运行过程中有效；

>**总结：** `SGID` 和 `SUID` 不同之处在于，`SUID` 赋予用户的是 **文件所有者** 的权限，而 `SGID` 赋予用户的是 **文件所属组** 的权限。

#### SGID 对目录的作用

`SGID` 也可用于 **目录**，具体内容如下：

当一个目录被赋予 `SGID` 权限后，进入此目录的 **普通用户**，其 **有效群组** 会变成该目录的 **所属组**。也就是当用户在创建文件（目录）时，该文件（目录）的所属组不在是用户的 **所属组**，而使用的是目录的 **所属组**。

```bash
# 进入临时目录做此实验。因为只有临时目录才允许普通用户修改
[root@localhost ~]# cd /tmp

# 建立测试目录并查看权限 
[root@localhost tmp]# mkdir dtest
[root@localhost tmp]# ll -d dtest
> drwxrwxrwx. 2 root root 6 May 10 08:32 dtest/
# 使用用户zjh在此目录创建 ztest.txt 文件，并查看权限
[zjh@localhost dtest]$ touch ztest.txt
[zjh@localhost dtest]$ ll
> -rw-rw-r--. 1 zjh zjh 0 May 10 08:40 ztest.txt
------------------------------------ 上方dtest目录未添加SGID，下方dtest目录添加SGID权限。
# 测试目录赋予 SGID 权限
[root@localhost tmp]# chmod g+s dtest/
[root@localhost tmp]# ll -d dtest/
> drwxrwsrwx. 2 root root 6 May 10 08:32 dtest/
# 使用用户zjh在此目录创建 test.txt 文件，并查看权限
[zjh@localhost dtest]$ touch test.txt
[zjh@localhost dtest]$ ll
> -rw-rw-r--. 1 zjh root 0 May 10 08:43 test.txt
> -rw-rw-r--. 1 zjh zjh  0 May 10 08:40 ztest.txt

> # 结论：目录添加SGID权限后，虽然是使用 zjh 用户创建ztest.txt，但它的所属组不是 zjh（zjh用户的所属组），而是root（dtest目录的所属组）
```

>**注意：** 只有当普通用户对具有 `SGID` 权限的目录有`rwx` 权限时，`SGID` 功能才能发挥。如用户对该目录有 `rw` 权限，用户进入此目录后虽其 **有效组** 变为此目录的 **所属组**，但由于没有 `x` 权限，用户无法在目录中创建文件或目录，`SGID` 则无法发挥它的作用。

### SBIT 粘滞位

---

`SBIT` 特殊权限，意味 **粘着位、粘滞位、防删除位**等。

`SBIT` 权限仅对 **目录** 有效，一旦目录设定了 `SBIT` 权限，则用户在此目录下创建的文件或目录，就只有 **用户自身** 和 `root` 才有权利 **删除** 和 **修改** 该文件。

```bash
# 存储临时文件的/tmp目录就设有SBIT权限。观察可知，其他人的权限设定，原来的x权限被t权限占用。
[root@localhost ~]# ll -d /tmp/
drwxrwxrwt. 16 root root 4096 May 10 08:37 /tmp/
--------------------------- # 案例2：下方验证只有用户自身和root才有权限删除和修改文件
# 如下演示。观察SBIT权限对 /tmp目录的作用。
# 在zjh用户创建文件
[root@localhost ~]# su - zjh
[zjh@localhost ~]$ cd /tmp
[zjh@localhost tmp]$ touch test
[zjh@localhost tmp]$ ll test
-rw-rw-r--. 1 zjh zjh 0 May 10 12:29 test

# 切换用户hw，测试在SBIT权限目录下，对zjh用户的文件进行删除和修改
[root@localhost ~]# su - zjd
[zjd@localhost ~]$ cd /tmp/
# 删除
[zjd@localhost tmp]$ rm -rf test
> rm: cannot remove ‘test’: Operation not permitted # 不允许这样操作
# 写入
[zjd@localhost tmp]$ echo 'aaa' > test
> -bash: test: Permission denied # 无权限，拒绝访问

----------------------------- # 案例2：赋予test文件667权限呢？再次对文件进行删除和修改测试。
[zjh@localhost tmp]$ ll test
-rw-rw-r--. 1 zjh zjh 0 May 10 12:32 test
[zjh@localhost tmp]$ chmod 667 test
[zjh@localhost tmp]$ ll test
-rw-rw-rwx. 1 zjh zjh 0 May 10 12:32 test
[zjh@localhost tmp]$ su zjd
# 删除
[zjd@localhost tmp]$ rm -rf test
rm: cannot remove ‘test’: Operation not permitted # 不允许这样操作
# 写入
[zjd@localhost tmp]$ echo 'aaa' > test
[zjd@localhost tmp]$ cat test # 成功写入
aaa 

```

>**结论：** `SBIT` 权限只能保护 **目录** 下的文件或目录不被 **其他用户** 删除。不能防止 **其他用户** 对文件或目录进行修改（如上方案例2，赋予文件或目录 **其他用户**拥有`w`权限）

### SUID、SGID、SBIT 权限设置

在 `Linux` 系统中，使用 `chmod` 命令给文件或目录手动设定 **特殊权限**。

#### 数字权限设置

手动设定 **特殊权限** 有两种方式，首先是使用 `chmod + 数字` 的形式给文件或目录设定权限。

`chmod` 命令设置 **普通权限** ，只需设定 `3`个数字权限位，即可实现设定 **普通权限**，如`chmod 755` 表示 **所有者** 的权限为 `rwx`，**所属组**的权限位`rx` 权限，**其他人**的权限位为 `rx` 权限。

`chmod` 目录设置 **特殊权限**，只需在 `3` 个数字之前添加一个数字位，用来放置给文件或目录设定特殊权限。`SUID、SGID、SBIT`分别对应的数字如下：

- `4` —> `SUID`
- 







<br />



## 3. ACL 访问控制权限

内容暂时用不上，待补充，关于[学习链接](http://c.biancheng.net/view/3120.html)

















































