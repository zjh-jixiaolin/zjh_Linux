# Linux 主机状态与环境变量


[TOC]

文档：[link](http://c.biancheng.net/view/705.html)

## 0. 查看系统资源占用

简介：在 `Linux` 中，通过 `top` 命令查看 `CPU、内存`的使用情况，类似 `Windoes` 的任务管理器，默认 **每  5 秒刷新** 一次。

### top 命令详解

功能：查看主机运行状态。

语法：`top [选项]`

| 选项 | 功能                                             |
| ---- | ------------------------------------------------ |
| `-p` | 显示某个进程信息                                 |
| `-d` | 设置刷新时间，默认`5s`                           |
| `-c` | 显示产生进程的完整命令信息，默认为进程名         |
| `-i` | 不显示任何闲置（`idle`）或无用（`zombie`）的进程 |
| `-u` | 查看 **特定用户** 启动的进程                     |

| 交互式选项（进入 top 后） | 功能                                         |
| ------------------------- | -------------------------------------------- |
| `c` 键                    | 显示进程的完整命令信息                       |
| `f` 键                    | 可选择需要展示的项目（过滤到不需要的选项）   |
| `M` 键                    | 根据 内存大小（RES）排序                     |
| `P` 键                    | 根据 `CPU` 使用百分比大小排序                |
| `T` 键                    | 根据 **累计时间** 排序                       |
| `E` 键                    | 切换 **顶部内存** 显示单位（KB\MB\GB\TB\PB） |
| `e` 键                    | 切换 **进程内存** 显示单位（KB\MB\GB\TB\PB） |
| `i` 键                    | 不显示 **闲置或无用** 的进程                 |
| `t` 键                    | **进度条** 显示 `CPU` 状态信息               |
| `m` 键                    | **进度条** 显示 **内存** 状态信息            |

**第一行信息**：![image-20230521144147892](https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202305211453289.png)

-  `top`：命令名称 。
- `14:40:31`：当前系统时间 。
- `up 17 min`：启动了`17`分钟。
- `2 users`：两个用户登录系统。
- `load`：1 — 5 — 15分钟的系统负载信息，如果为`1`代表有`1颗CPU`百分百繁忙。

**第二行信息**： ![image-20230521144906457](https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202305211453274.png) 

- `Tasks`：共 `125` 个进程。
- `1 running`：`1`个进程在运行。
- `124 sleeping`：`174`个进程处于睡眠。
- `0 stopped`：`0`个停止进程。
- `0 zombie`：`0`个僵尸进程。

**第三行信息**：![image-20230521151302878](https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202305211518564.png)

- `%Cpu(s)`：`CPU` 使用率。
- `us`：用户 `CPU` 使用率 **（重要）**。
- `sy`：系统 `CPU` 使用率 **（重要）**。
- `ni`：高级优先级进程占用 `CPU` 时间百分比。
- `id`：空闲 `CPU` 率。
- `wa`：`IO` 等待 `CPU` 占用率。
- `hi`：`CPU` 硬件中断率。
- `si`：`CPU` 软件中断率。
- `st`：强制等待占用 `CPU` 率。

**第四行信息**：![image-20230521151755995](https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202305211518315.png)

- `KiB Mem`：物理内存。
- `total`：内存总量。
- `free`：内存空闲量 **（重要）**。
- `used`：内存使用量 **（重要）**。
- `buff/cache`：系统缓存了多少内存。

**第五行信息**：![image-20230521152310094](https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202305211528776.png)

- `KibSwap`：虚拟内存（交换空间）。
- `total`：虚拟内存总量。
- `free`：虚拟内存空闲量。
- `used`：虚拟内存使用量。
- `avail Men`：系统可分配的虚拟内存大小。

### 进程信息![image-20230521153029404](https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202305211531835.png)

- `PID`：进程 `id`。
- `USER`：进程所属用户。
- `PR`：进程优先级，越小越高。
- `NI`：负值表示高优先级，正表示低优先级。
- `VIRT`：进程使用虚拟内存，单位 `KB`。
- `RES`：进程使用物理内存，单位 `KB`。
- `SHR`：进程使用共享内存，单位 `KB`。
- `S`：进程状态（`S` 休眠、`R`运行、`Z` 僵死状态、`N`负数优先级、`I`空闲状态）
- `%CPU`：进程占用 `CPU` 率。
- `%MEM`：进程占用内存率。
- `TIME+`：进程使用`CPU`时间总计，单位`10`毫秒。
- `COMMAND`：进程的命令或名称或程序文件路径。



<br />



## 1. 磁盘信息监控

简介：使用 `df` 命令，可查看硬盘的使用情况。

### df 命令

功能：查看磁盘占用。

语法：`df [-Th]`

- 参数：`-h` 将输出的磁盘大小、已使用大小和可用大小等信息以更 **人性化** 方式显示。
- 参数：`-T` 增加显示 **文件系统** 类型。

```bash
[zjh@centos ~]$ df -h
# 文件系统				  容量   已用  可用 已用%  挂载点
> Filesystem               Size  Used Avail Use% Mounted on
> devtmpfs                 1.9G     0  1.9G   0% /dev
> tmpfs                    1.9G     0  1.9G   0% /dev/shm
> tmpfs                    1.9G   12M  1.9G   1% /run
> tmpfs                    1.9G     0  1.9G   0% /sys/fs/cgroup
> /dev/mapper/centos-root   36G  2.2G   33G   7% /
> /dev/sda1               1014M  234M  781M  24% /boot
> tmpfs                    378M     0  378M   0% /run/user/0
> tmpfs                    378M     0  378M   0% /run/user/1000
```



## 2. 环境变量

**环境变量** 是操作系统（`Windows、Linux、Mac`）在运行时，记录的一些关键性信息，用以辅助系统运行。

在 `Linux` 系统中执行：`env`命令即可查看当前系统中记录的 **环境变量**。

**环境变量** 是一种 `KeyValue` 型结构，即名称和值，如下图：

- `HOME：/home/zjh` —— 用户的 `HOME` 路径
- `USER：zjh`  —— 当前的操作用户
- `PWD：/homt/zjh` —— 当前的工作目录

![image-20230521221908612](https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202305212237625.png)



### 环境变量：PATH

在 `Linux` 中的命令本质上就是一个个的 **可执行程序**。如 `pwd` 命令的本体为 `/usr/bin/pwd` 程序文件。

无论当前的工作目录在哪，当我们执行 `pwd` 命令时都能准确运行，这就是借助 **环境变量** 中 `PATH`的值。

![image-20230521222851214](https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202305212238254.png)

`PATH` 记录了系统执行任何命令的搜索路径，如上记录了（路径之间用 `:` 隔开），如：

- `/usr/local/bin`
- `/usr/bin`
- `/usr/local/sbin`
- .........

当执行命令时，会按照顺序，从上述路径中**搜索**要 **执行程序** 本体。如 `pwd` 命令。从第二个目录 `/usr/bin` 搜索到 `cd`命令并执行。

### $ 符号

在 `Linux` 系统中，`$` 符号被用于 取 **变量**  的值。

取得 **环境变量** 的值可通过语法：**$环境变量名** 取得，如 `echo $PATH`即可获取到 `PATH`环境变量的值：

![image-20230521223750999](https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202305212238468.png)

或者：`echo ${PATH}ZJH`

![image-20230521224253190](https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202305212242240.png)

当和其他内容混合在一起时，可通过 `{}`来标注取的变量是哪个。

### 自行设置环境变量

`Linux` 环境变量允许用户自行设置，其中分为：

- 临时设置（重启会失效），语法：`export 变量名=变量值`
- 永久生效
  - 针对 **当前用户** 生效，配置在 **当前用户** 的 `~/bashrc`文件中，如 `export ZJH=666`
  - 针对 **所有用户** 生效，配置在 **系统** 的 `/etc/profile` 文件中，如 `export MYNAME=ZJH`
  - 通过语法：`source 配置文件` 进行立即生效 或 重启系统。

![image-20230521230043979](C:/Users/18279/AppData/Roaming/Typora/typora-user-images/image-20230521230043979.png)

### 自定义环境变量 PATH

环境变量 `PATH` 项目中记录了**系统执行命令**的搜索路径。搜索路径也可自行添加到 `PATH` 当中。具体如下：

- 修改 `PATH` 的值，将 `export PATH=$PATH:/home/python` 填入系统环境变量文件`/etc/profile` 中去。

  ```bash
  # 添加命令
  [root@centos ~]# mkdir python
  [root@centos python]# touch haha
  [root@centos python]# vim haha 
  > echo 'ENV_TEST'
  
  # 给命令执行权限
  [root@centos python]# chmod 755 haha 
  
  # 将命令搜索路径添加到系统环境变量PATH中
  vim /etc/profile
  > export PATH=$PATH:/root/python
  # 使配置文件立即失效
  source /etc/profile
  
  # 测试
  [root@centos ~]# haha
  > ENV_TEST
  ```

  































