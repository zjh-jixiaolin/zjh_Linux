## Git 命令汇总

```bash
---------------------------------------------------- 本地仓库
# 初始化本地仓库
git init

# 工作区 ——> 暂存区
git add .

# 暂存区 ——> 本地仓库（-m：提示消息）
git commit -m 'commit message 01'

# 查看提交状态(查看当前Git文件状态：修改，暂存的文件等)
git status

# 查看仓库提交历史（类似快照）
git log / git-log

# 版本回退（HEAD：此版本、 HEAD^：上个版本 、 HEAD^^：上上个版本 、 HEAD~2：回退两个版本）
git reset --hard <commit>

# 查看删除的版本记录
git reflog

# 放弃工作区的修改（还没git add .前）
git checkout -- file

# 放弃暂存区的修改（还没 git commit -m前）
git reset HEAD file

# 查看分支
git branch

# 创建分支
git branch <name>

# 创建并切换分支 
git checkout -b 分支名

# 分支合并（首先需要切换到目标分支上（例：master））
git merge 分支名

---------------------------------------------------- 远程仓库
# 添加远程仓库
git remote add <远端名字> <仓库地址>
> git remote add origin git@github.com:zjh-jixiaolin/git_test # origin：源地

# 查看远程仓库
git remote

# 推送
git push [-f] [--set-upstream][远端名称[本地分支名][:远端分支名]]
> git push --set-upstream origin master # 与远端分支简历关联
> git push # 建立关联后,则可以省略分支名和远端名

# 克隆
git clone <仓库路径> [本地目录]
> git clone https://github.com/zjh-jixiaolin/git_test.git GitHub

# 抓取
git fetch [remote name] [branch name]
> git fetch # 不指定则将远程仓库更新都抓到本地，不合并分支


# 拉取
git pull [remote name] [branch name] 
> git pull # 不指定则将远程仓库更新都抓到本地，并进行合并

```









































