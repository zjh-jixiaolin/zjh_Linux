# Git 入门和实践

[TOC]



GitHub 入门文档：[https://docs.github.com/zh/get-started](https://docs.github.com/zh/get-started)



## 0. Git的简介与安装

### Git 简介

Git  是 `Linus Torvalds` 为了帮助管理 `Linux` 内核开发而开发的一个开放源码的`版本控制`软件。它是一个`开源`的`分布式版本控制系统`，用于敏捷高效地处理任何或小或大的项目。

​	如下图是以`Git` 为代表的分散型的示意图。`Github` 将仓库`Fork（分配）`给了每一个用户，也就是将`GitHub`的某个特定仓库复制一份到用户的本地上。`Fork（分配）`出的仓库与原仓库互不影响，开发者可以随意更改内容。分散型拥有多个仓库，相对而言稍显复杂。不过由于本地的开发环境就有仓库，所以开发者不必连接远程仓库就可以进行开发。

![image-20230328192155834](https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202303281922391.png)

> **版本控制是什么 ?** 
>
> ​	版本控制就是管理更新的历史记录。它为我们提供了一些在软件开发过程中必不可少的功能。例如记录一款软件添加或更改源代码的过程，回滚到特定阶段，恢复误删除的文件等。



<br />



### Git 工作流程图

![image-20230818142657205](https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/image-20230818142657205.png)

![image-20230904173800126](https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/image-20230904173800126.png)

**命令如下：**

1. `clone（克隆） ` :  从远程仓库中克隆代码到本地仓库
2. `checkout（检出）` :  从本地仓库中检出一个仓库分支然后进行修订
3. `add（添加）` :  在提交前先将代码提交到暂存区
4. `commit（提交）` :   提交到本地仓库，本地仓库中保存修改的各个历史版本
5. `fetch（抓取）` :  从远程库，抓取到本地仓库，不进行任何的合并动作，一般操作比较少
6. `pull（拉取）` :  从远程库拉到本地库，自动进行`合并（merge）`，然后放到工作区，相当于 `fetch+merge`
7. `push（推送）` :  修改完成后，需要和团队成员共享代码时，将代码推送到远程仓库



<br />



### Git 安装

---

#### Windows 安装

Git 版本控制工具官网：[https://git-scm.com/downloads](https://git-scm.com/downloads)

#### Linux — Ubantu安装

```bash
# 安装依赖并更新软件包
sudo apt-get install libcurl4-gnutls-dev libexpat1-dev gettext libz-dev libssl-dev
sudo apt-get update
sudo apt-get upgrade

# 安装 git
sudo apt-get install git
# 检验是否成功（顺便查看版本）
git --version
```

**备注：**

​	`Git GUI`： `Git` 提供的图形界面工具

​    `Git Bash`：`Git` 提供的命令行工具

<br />



## 1. Git 使用

### 1.1 Git 基本配置

---

当安装完 `Git`后首先要做的事情是设置`用户名称`和`email地址`，因为每次`Git`提交都会使用到该用户信息。

#### 1.1.1 *查看 Git 版本：

```bash
git --version

```

#### 1.1.2 *设置本地仓库的账号和邮箱：

  ```bash
# 作用：① 提交记录的可追溯性   ② 电子邮件通知  ③ 代码贡献的准确性

git config --global user.name "Firstname Lastname"  # 设置姓名
git config --global user.email "username@example.com"  # 设置邮箱

  ```

#### 1.1.3  *设置代码可读性

```bash
# 作用：为Git命令启用一些额外的颜色，方便阅读

git config --global color.ui auto  # 使用color.ui 让命令的输出拥有更高的可读性

```

#### 1.1.4 *查看 Git 全局配置文件

```bash
# Git 用户的配置文件位于 ~/.gitconfig
cat ~/.gitconfig
----------------------------------
[user]
        email = 34595010812@qq.com
        name = zjh-jixiaolin
[color]
        ui = auto
----------------------------------

# 查看配置文件
git config --list # 显示当前系统上正在使用的 Git 配置设置，包括全局和用户特定的配置选项

```

初始化设置的值，会在 `~/.gitconfig` 中以如下形式输出设置文件：

<p><img src="https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202303291012598.png" align="middle" /></p>

#### 1.1.5 *bashrc个性化命令

`.bashrc`是 `home` 目录下的一个`shell`文件，用于存储用户的`个性化设置`（简化使用命令）。

```bash
touch ~/.bashrc # 打开用户目录，创建 .bashrc文件

vim ~/.bashrc # 在 .bashrc文件输入个性化内容（~/.是用户目录下的隐藏文件）
----------------------------------
# alias：别名（设置个性化命令）
alias ll='ls -al' 
alias git-log='git log --all --pretty=oneline --abbrev-commit --graph'

----------------------------------

# 加载个性化命令
source ~/.bashrc # source：重新加载shell环境（使个性化命令生效）

```



<br />




### 1.2 Git 初始化

---

想使用 `Git` 对代码进行版本控制，首先需要`初始化`本地仓库：

1.  创建空目录（作为本地仓库）
2.  进入目录，执行命令 `git init` 对仓库进行初始化
3.  初始化完成后出现隐藏的`.git`目录

![image-20230406105020330](https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202304061342462.png)

>初始化本地仓库后，生成一个 `.git` 隐藏目录，这是 `Git` 用来跟踪管理版本，进行仓库的版本控制。



<br />



### 1.3 Git 本地仓库流程 — 图解

---

`Git` 工作目录下对于文件的`修改，增加，删除，更新`会存在几个状态，修改的状态会随着执行 `Git` 命令发生变化：

1.  未追踪`(Untracked)`：指在工作目录（初始化后的目录）中`新增`的文件，`Git` 并不会自动将其纳入版本控制，需要通过`git add`命令将其添加到暂存区。
2.  已暂存`(Staged)`：指通过`git add`命令将工作目录中的修改添加到了暂存区，但尚未提交到版本库中。
3.  已修改`(Modified)`：指工作目录中已有的文件被`修改`过，但还未被添加到暂存区或提交到版本库中。
4.  已提交`(Committed)`：指文件已被提交到 `Git` 版本库中，使用`git commit`提交文件，它们的状态是最新版本。

>总结：① 执行`git add` 会将 未追踪 或 已修改的文件添加到`暂存区`，变为已暂存状态；
>
>②  执行`git commit` 命令将暂存区中的文件提交到`本地Git版本库`中，变为已提交状态；

**Git 本地仓库流程：工作区 ——> 暂存区 ——> 版本库**

1. 工作区：电脑中实际的目录（通过 `git init `初始化的目录即为工作区）；
2. 暂存区：相当于中转站（缓存区域），用来暂时存放工作区中修改的内容，内容存放在 **.git** 目录下的 index 文件内；
3. 版本库：相当于记录库，将暂存区内容提交到本地仓库并生成记录放在版本库中，**.git** 目录存放着版本库的信息；

具体步骤如图：

![image-20230406134744242](https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202304061347068.png)

使用 `Git` 命令来控制这些状态之间的转换：

1.  `git add`（工作区  —> 暂存区）
2.  `git commit` （暂存区 —>本地 Git 仓库）

>**流程：**
>
>1.  使用 `git init` 后创建了一个本地仓库的工作区，工作区没有包含任何提交记录（空 Git 仓库）。
>2.  当对文件进行`修改`或`编辑`后，使用 `git add .` 将文件加入`暂存区` 。
>3.  确认无误后，通过 `git commit -m`将暂存区的内容提交到`本地Git仓库`，此过程会产生`提交记录`，并生成`日志文件`。
>4.  通过 `git-log`可以查看日志文件。从而对每次提交的`记录`进行`版本控制`。
>
>**重点：**
>
>1. `.git` 为`Git`版本库的核心目录，是`Git` 版本库中所有数据的存储位置，包括`Git`版本库中的所有`历史记录`、`分支信息`等。



<br />



### 1.4 Git 常用命令

---

#### 1.4.1 *查看Git仓库状态（status）

作用：查看当前分支的状态（`暂存区、工作区`）

```bash
# 显示 Git 仓库状态
git status 

```

#### 1.4.2 *添加工作区到暂存区（add）

作用：添加工作区`一个`或`多个`文件的修改到暂存区

```bash
# 命令形式： git add 单个文件名 | 通配符
git add test.md # 添加单个文件到暂存区

git add *.txt # 通配符，将工作区所有.txt文件加入暂存区

git add . # 将所有修改加入暂存区

```

#### 1.4.3 *提交暂存区到本地仓库（commit）

作用：提交`暂存区内容`到`本地仓库`的当前分支

```bash
# 命令形式：git commit -m "注释内容"

git commit -m "update file" # 暂存区内容 ——> 本地仓库

```

#### 1.4.4 *查看提交日志（log）

作用：查看`提交日志`

```bash
# 命令形式: git log [option]   

git log # 显示 当前分支的提交历史记录

git log --all # 显示 所有分支（本地分支和远程分支）的提交历史记录

git log --pretty=oneline # 将提交信息显示为一行（只显示 commitID）

git log --abbrev-commit # 使输出的 commit id 更简短（7位字符）

git log --graph # 以图的形式显示
```

#### 1.4.5 *版本回退（reset）

作用：`版本切换`

```bash
# 命令形式：git reset [option] <commitID>

git reset --soft 77a44d5 # 回退分支到指定的提交版本；但不更改工作目录和暂存区内容。

git reset --mixed 77a44d5 #（默认模式）回退分支到指定的提交版本，并将暂存区重置为指定提交相同的状态。

git reset --hard 77a44d5 # 回退分支到指定的提交版本，并将暂存区和工作目录都重置为指定提交相同的状态

```

```bash
# 使用 hard 回滚后，当前版本记录会被永久删除。

git reflog #查看已经删除的记录

```

>`git reset`命令有三种模式：`--soft`、`--mixed`和`--hard`。这些模式控制了回退操作对Git工作目录和暂存区的影响：
>
><br />
>
>- `--soft`模式：回退分支到指定的提交版本，但不更改工作目录和暂存区的内容。这意味着您的更改仍然保留在暂存区和工作目录中，可以使用`git commit`命令将它们提交到新的提交中。
>
>
>
>- `--mixed`模式（默认模式）：回退分支到指定的提交版本，并将暂存区重置为与指定提交相同的状态。这意味着您的更改仍然保留在工作目录中，但不会被提交。
>
>
>
>- `--hard`模式：回退分支到指定的提交版本，并将暂存区和工作目录都重置为与指定提交相同的状态。这意味着您的更改将被永久删除。

#### 1.4.6 *添加文件至忽略列表

一般情况下总有些文件无需纳入 `Git` 管理，比如`自动生成`的日志文件、编译过程中的临时文件等。在此情况下，我们可以在工作目录中创建一个 `.gitignore` 文件，列出需要`忽略`的文件模式，以下实例：

```bash
vim .gitignore
----------------------------------
# .txt文件都不纳入Git版本控制管理
*.txt

# doc目录下的.pdf文件不纳入Git版本控制系统
doc/*.pdf
----------------------------------

```



<br />



### 1.5 分支

---

几乎所有的版本控制系统都以某种形式支持分支。使用分支意味着你可以把你的工作从`主线上分离出去`来进重大Bug的修改、开发新的功能，以免影响开发主线。

#### 1.5.1 查看本地分支

```bash
# 查看分支 
git branch

```

#### 1.5.2 创建本地分支

```bash
# 创建分支—命令：git branch 分支名
git branch dev01

```

#### 1.5.3 *切换分支（checkout）

```bash
# 切换分支—命令：git checkout 分支名
git checkout dev01

# 另一种方式：切换到不存在的分支（创建并切换）
git checkout -b 分支名
```

#### 1.5.4 *合并分支（merge）

一般位于`master`分支上进行合并

```bash
# 多分支进行合并: git merge 分支名称
git merge dev01

```

#### 1.5.5 删除分支

**不能删除当前分支，只能删除其他分支**

```bash
# 删除分支时，做各种检查
git branch -d 分支

# 强制删除，不做检查
git branch -D 分支

```

>**注意：** 使用 `git-log` 查看提交日志时，`HEAD ->`指向的是`当前分支`。

#### 1.5.6 解决冲突

当两个分支上对文件的修改可能会存在`冲突`，例如修改了`同个文件的同一行`，这就需要`手动解决`冲突，解决冲突步骤如下：

1.  处理文件中`冲突`的地方
2.  将解决完冲突的文件`加入暂存区（add）`
3.  `提交`到`本地Git仓库(commit)`

冲突部分的内容例子如下：

```bash
# 首先 — 初始化本地仓库
git init

# 创建文件并上传到Git仓库（工作区 —— 暂存区 —— 本地Git仓库）
# 此操作在 master 分支
echo 'aaa' > test.txt
git add .
git commit -m 'master file01 aaa' # 产生记录
—————————————————————————————— 上方为测试环境部署，下方才是演示冲突情况。

# 创建并切换新分支(dev1 分支) —— 修改文件内容为bbb —— 提交产生记录
git checkout -b dev1
echo 'bbb' > test.txt 
git add .
git commit -m 'dev1 file01 bbb' # 产生记录（在dev1分支上将文件内容修改为bbb）

# 回到 master分支 —— 修改文件内容为ccc —— 提交产生记录
git checkout master
echo 'ccc' > test.txt 
git add .
git commit -m 'master file01 ccc 好' # 产生记录（在master分支上将文件内容修改为ccc）
—————————————————————————————— 上方为创建的两个分支，未合并前，见下图1

# 进行合并，此时会产生冲突。系统产生错误提示要 手动修复冲突
git merge dev1
> Auto-merging test.txt 
> CONFLICT (content): Merge conflict in test.txt # 冲突(内容):合并test.txt中的冲突
> Automatic merge failed; fix conflicts and then commit the result. # 自动合并失败;修复冲突，然后提交结果。

# 查看 test.txt文件
cat test.txt
> <<<<<<< HEAD # 告诉你，当前分支的值
> ccc
> =======
> bbb 
> >>>>>>> dev1 # 告诉你，合并分支的值

# 解决冲突（手动解决：选择想要的值）—— 上传暂存区 —— 上传本地仓库(解决冲突完成合并)
echo 'ccc' > test.txt 
git add .
git commit # 按esc — 输入:wq
> [master 6c6e4b1] Merge branch 'dev1'

# 验证 （过程如图2）
git-log
cat test.txt
> ccc

```

图1：

![image-20230409171901129](https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202304091719646.png)

图2：

![image-20230409173843901](https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202304091739473.png)

#### 1.5.7 开发中分支使用原则与流程

几乎所有的版本控制系统都以某种形式支持分支。`使用分支`意味着你可以把工作从`开发主线`上分离开来进行重大`Bug修改、开发新功能，以免影响开发主线。`在开发中，一般有如下分支使用原则与流程：

- `master（生产）分支`

  线上分支，主分支，中小规模项目作为线上运行的应用对应的分支；

  

- `develop（开发）分支`

  是从 master 创建的分支，一般作为`开发部`部门的主要开发分支，如果没有其他并行开发不同期上线要求，都可以在此版本进行开发

  

- `feature/xxxx分支`

  从`develop` 创建的分支，一般是同期并行开发，但不同期上线时创建的分支，分支上的研发任务完成后合并到`develop`分支

  

- `hotfifix/xxxx分支`

  从master派生的分支，一般作为`线上bug修复`使用，修复完成后需要合并到master、test、develop分支。

  

- 还有一些其他分支，例如`test分支（用于代码测试）`、`pre分支（预上线分支）`等。

<img src="https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202304091959720.png" alt="image-20230409195952037" style="zoom: 67%;" />

### 1.6 总结

>1.  工作区 —> 暂存区：`git add .`
>2.  暂存区 —> 本地仓库 ： `git commit -m 'commit message 01'`
>3.  查看状态  ： `git status`
>4.  查看提交记录 ： `git log` 或 自定义命令`git-log`
>5.  查看分支 ： `git branch`
>6.  创建并切换分支  ： `git checkout -b 分支名`
>7.  分支合并：`git merge 分支名`  — 首先切换到目标分支上（master）



<br />



## 2. Git 远程仓库

>目前已知两种 `Git` 类型仓库，即`本地仓库`和`远程仓库`。那么如何搭建`Git 远程仓库`，我们可以借助
>
>互联网上提供的一些`代码托管服务` 来实现，比较常用的有[GitHub](https://github.com/)、[Gitee （码云）](https://gitee.com/)、[GitLab](https://about.gitlab.com/)等。
>
>- [GitHub](https://github.com/) 是一个面向`开源及私有`软件项目的托管平台，服务器在国外，需要挂国外节点才能访问。
>
>- [Gitee （码云）](https://gitee.com/) 是国内的一个代码托管平台，服务器在国内。
>- [GitLab](https://about.gitlab.com/) 是一个用于仓库管理系统的开源项目，使用`Git` 作为代码管理工具，并在此基础上搭建起来的`Web`服务，一般用于在企业、学校等`内部网络`搭建 `git私服`。

### 2.1 创建远程仓库

首先，注册 `GitHub` 账户，然后新建一个仓库，如下图所示：

![image-20230410183140279](https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202304101831780.png)

![image-20230410183423017](https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202304101834698.png)

### 2.2  配置 SSH 公钥

当使用`Git`进行`版本控制`时，可以将`代码仓库`存储在`远程服务器上`，如GitHub、GitLab 或 Gitee。使用`SSH密钥对`进行`身份验证`可以确保代码仓库的`安全性`。具体过程如下：

1. 在`本地`计算机上生成一对`SSH密钥`对：`公钥`和`私钥`。

   ```bash
   # 生成密钥对命令
   ssh-keygen -t rsa
   
   # 运行后会产生一对SSH密钥对（公钥和私钥）
   # 公钥（存储在 ~/.ssh/id_rsa.pub文件中）、私钥（存储在 ~/.ssh/id_rsa文件中）
   ```

   

2. 将`公钥`添加到要使用的`Git托管服务（如GitHub）`的账户中，以便进行`身份验证`。`私钥`则必须`保密`，只能存储在本地计算机中。

   ```bash
   # 获取公钥（上方已经生成密钥对）
   cat ~/.ssh/id_rsa.pub
   
   # 获取到公钥后，需要将公钥提交到自己的GitHub账户上（GitHub仓库为远程服务器）
   # 这样才能确保持有私钥的用户进行访问和操作GitHub上的代码仓库，如果不添加公钥，
   # 每次访问或者推送新版本代码到GitHub上的代码仓库时，都需要输入用户名和密码。
   # 添加SSH密钥后，既能保证安全性，也能减少每次推送或拉取都要输入密码的繁琐操作。
   ```

3. 在连接到`远程Git仓库`时，使用`私钥`进行`身份验证`，以避免输入密码。

   ```bash
   添加SSH公钥后，在本地仓库验证是否配置成功
   # 使用ssh密钥对身份进行验证来访问GitHub
   ssh -T git@github.com
   
   > Hi zjh-jixiaolin! You've successfully authenticated, but GitHub does not provide shell access.
   ```

   

使用`SSH密钥对`进行`Git身份验证`可以提高`安全性`，并减少了在每次推送或拉取时都需要输入密码的繁琐。同时，私钥只保存在本地计算机上，不会被泄露，从而保证了代码仓库的安全性。（GitHub设置公钥如下图）

![image-20230410222851061](https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202304102231681.png)



### 2.3 操作远程仓库

---

#### 2.3.1 添加远程仓库

此操作是想`初始化`本地库，然后与已创建的远程库进行对接。

- 命令： `git remote add <远端名称> <仓库地址>`
  - 远端名称，默认是`origin（源头）`，取决于远端服务器设置
  - 仓库路径：从远端仓库获取此URL

```bash
# 告诉本地Git仓库所对应的远程仓库是哪个
git remote add origin git@github.com:zjh-jixiaolin/git_test

# 详解：添加远程仓库 —— 远程仓库名字 ——— 远程仓库地址
```

#### 2.3.2 查看远程仓库

- 命令：`git remote`

  <img src="https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202304110031527.png" alt="image-20230411000226986" style="zoom:67%;" />

#### 2.3.3 推送到远程仓库（push）

- 命令：`git push [-f] [--set-upstream][远端名称[本地分支名][:远端分支名]]`

  - `-f:` 表示强制覆盖（一般被禁用）

  - 如果`远端分支名`和`本地分支名`相同，则可以只写本地分支
    - `git push origin master:master` —> `git push origin master`
  - `--set-upstream`：推送到远端的同时指定`建立`和远端分支的`关联关系`
    - `git push --set-upstream origin master`
      - 如果**当前分支和远端分支关联**，则可省略分支名和远端名
        - `git push` 将`master`分支推送到已关联的远端分支

  ![image-20230411003156486](https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202304110031968.png)

#### 2.3.4 本地分支和远程分支的关联关系

- 查看`本地分支`和`远程分支`的`关联关系`可以使用`git branch -vv`命令

  ![image-20230411004039024](https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202304110040278.png)

#### 2.3.5 从远程仓库克隆

如果已经有一个远端仓库，可以直接`clone`到本地。

- 命令：`git clone <仓库路径> [本地目录]`

  - 本地目录可以省略，会自动生成一个目录

  ![image-20230411004843204](https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202304110048099.png)

#### 2.3.6 从远程仓库中抓取和拉取

`远程分支`和`本地分支`一样，我们可以进行`merge（合并）`操作。只需要先把`远程仓库`里的`更新`都下载到本地，再进行操作

- `抓取` 命令： `git fetch [remote name] [branch name]`
  - **抓取指令就是将仓库的更新都抓取到本地，`不会`进行`合并`**
  - 如果不指定`远端名称`和`分支名`，则抓取所有分支。

<br />



- `拉取`命令： `git pull [remote name] [branch name]`
  - **拉取指令就是将远端仓库的修改拉到本地并`自动进行合并`，等同于`fetch + merge`**
  - 如果不指定`远端名称`和`分支名`，则抓取所有`并更新`当前分支。

<br />

>实际开发中，`clone（克隆）`操作不会很频繁（实际开发中文件很大，如果每次克隆不现实）
>
>可以使用`抓取（fetch）` 或者 `拉取(pull)`来讲远端更新抓到本地，抓取fetch默认不合并，拉取pull更新并合并

