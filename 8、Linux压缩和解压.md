# Linux 压缩和解压

[TOC]

## 压缩格式

市面上有非常多的压缩格式，如下：

- `7zip`格式：`Windoes` 系统常用
- `rar`格式：`Windows` 系统常用

- `zip`格式：`Linux、Windows、MacOS` 系统常用
- `tar`格式：`Linux、MacOS`系统常用
- `tar`格式：`Linux、MacOS` 系统常用

在 `Windows` 中常用的压缩和解压缩的软件如：[Bandizip](https://www.bandisoft.com/bandizip/)、[WinRAR ](https://www.rarlab.com/download.htm) 等软件，都支持各类常用的压缩格式。

在 `Linux` 中操作`tar、gzip、zip`  三种压缩格式完成对文件的压缩、解压操作。

>`Linux` 和 `Mac` 系统常用有 `2` 中压缩格式，后缀名分别为：
>
>- `.tar`：归档文件，即简单的将文件组装到一个 `.tar`文件内，并没有太多 **文件体积的减小**，仅仅是简单的封装。
>- `.gz`：常见格式为`.tar.gz`，`gzip`格式压缩文件，即使用 `gzip` 压缩算法将文件压缩到一个文件内，可以**极大减少压缩后的体积**。



## tar 命令

针对 `.tar` 和 `.tar.gz` 两种压缩格式，均可用 `tar` 命令进行 **压缩** 和 **解压缩** 的操作

语法：`tar [-c -v -x -f -z -C] 参数1 参数2.....`，具体选项解析如下：

| 选项 |                             作用                             |
| :--: | :----------------------------------------------------------: |
| `-c` |               创建 **压缩** 文件，用于压缩模式               |
| `-v` |         显示 **压缩**、**解压缩**过程，用于查看进度          |
| `-x` |                        **解压** 模式                         |
| `-f` | 要创建的文件，或 **解压**的文件，`-f` 选项必须在所有选项中最后一位 |
| `-z` |         `gzip`模式，不使用即为`tar`模式（必用选项）          |
| `-C` |            选择**解压**目的地，用于 **解压** 模式            |

### 压缩

语法：`tar -zcvf 压缩包 被压缩1 被压缩2 ....`

- 选项：`-z`：表示使用`gzip`，压缩体积会更小（可不添加，默认`tar`格式）。

压缩过程：

```bash
[zjh@centos ~]$ ls
> 1.txt  2.txt
 # 压缩文件
[zjh@centos ~]$ tar -zvcf test.tar.gz 1.txt 2.txt 
 # 压缩成功
[zjh@centos ~]$ ls
> test.tar.gz 1.txt 2.txt
```

### 解压

语法：`tar -zxvf 被解压的文件 -C 要解压去的地方`

- 选项：`-z`：表示使用`gzip`，压缩体积会更小
- 选项：`-C`：指定要解压去的地方，不写解压到当前目录

解压过程：

```bash
# 查看当前路径：test文件夹、gz压缩包
[zjh@centos ~]$ ls
> test  test.tar.gz

# 解压到当前路径
[zjh@centos ~]$ tar -zxvf test.tar.gz
> 1.txt
> 2.txt

# 解压到指定路径
[zjh@centos ~]$ tar -zxvf test.tar.gz -C test
> 1.txt
> 2.txt
[zjh@centos ~]$ cd test
[zjh@centos test]$ ls
> 1.txt  2.txt
```



## zip、unzip命令

### zip 压缩

在 `Linux` 中，使用`zip` 命令，压缩文件为 `zip` 压缩包。

语法：`zip [-r] 压缩包 被压缩1 被压缩2`

- 选项：`-r`：被压缩文件包含文件夹时，使用 `-r` 选项。

`zip` 压缩过程：

```bash
# 压缩文件和文件夹
[zjh@centos ~]$ zip -r test.zip 1.txt 2.txt test
  adding: 1.txt (stored 0%)
  adding: 2.txt (deflated 67%) # 文件大小减少67%
  adding: test/ (stored 0%)
  adding: test/1.txt (stored 0%)
  adding: test/2.txt (stored 0%)
  

[zjh@centos ~]$ ls
1.txt  2.txt  test  test.zip
```

### unzip 解压

在 `Linux` 中，使用`unzip` 命令，解压文件为 `zip` 的压缩包。

语法：`unzip [选项] 压缩包文件`

- 选项：`-d` 指定解压去的位置。如 `unzip  test.zip -d test`
- 参数，被解压的 `zip` 压缩包文件。

`unzip`解压过程：

```bash
[zjh@centos ~]$ ls
> test.zip testzip # testzip为文件夹，.zip为压缩包

# 解压到当前路径
[zjh@centos ~]$ unzip test.zip

# 解压到指定文件夹
[zjh@centos ~]$ unzip test.zip -d testzip/
> Archive:  test.zip
>  extracting: testzip/1.txt
>  inflating: testzip/2.txt
>   creating: testzip/test/
> extracting: testzip/test/1.txt
> extracting: testzip/test/2.txt
```

















