# Git基础知识及常用命令

---------------------

- [Git基础知识](#git基础知识)
  - [本地版本控制系统](#本地版本控制系统)
  - [集中化的版本控制系统](#集中化的版本控制系统)
  - [分布式版本控制系统](#分布式版本控制系统)
- [Git安装](#git安装)
- [Git初次使用的配置](#git初次使用的配置)
  - [基础设置](#基础设置)
  - [额外设置](#额外设置)
- [通过SSH密钥连接GitHub](#通过ssh密钥连接github)
- [Git使用](#git使用)
  - [查看Git信息](#查看git信息)
    - [`git reflog` 显示所有的操作记录](#git-reflog-显示所有的操作记录)
    - [`git log`的点线图](#git-log的点线图)
  - [Git常用命令](#git常用命令)
  - [Git常用命令详解](#git常用命令详解)
    - [`git add`：将工作区的文件添加到暂存区](#git-add将工作区的文件添加到暂存区)
    - [`git status`：查看工作区和暂存区的状态](#git-status查看工作区和暂存区的状态)
    - [`git commit`：将暂存区的文件提交到本地仓库并添加提交说明](#git-commit将暂存区的文件提交到本地仓库并添加提交说明)
    - [`git push`&`git pull`：将本地仓库推送到远程仓库，或者从远程仓库拉取本地仓库](#git-pushgit-pull将本地仓库推送到远程仓库或者从远程仓库拉取本地仓库)
    - [`git branch`：创建、删除、切换分支](#git-branch创建删除切换分支)
    - [`git merge`：合并分支](#git-merge合并分支)
    - [`git rebase`：分支变基](#git-rebase分支变基)
    - [`git stash`：保存和恢复进度](#git-stash保存和恢复进度)
    - [`git diff`：查看工作区和暂存区的差异](#git-diff查看工作区和暂存区的差异)
    - [`git remote`：操作远程仓库](#git-remote操作远程仓库)
    - [`git tag`：操作标签](#git-tag操作标签)
    - [`git rm`：删除文件](#git-rm删除文件)
    - [`git checkout`：切换分支](#git-checkout切换分支)
    - [`git reset`：回退版本](#git-reset回退版本)
    - [`git revert`：回退版本但不回退之前的提交](#git-revert回退版本但不回退之前的提交)
    - [`git checkout`&`git reset`&`git revert`：版本切换&重设&撤销的区别](#git-checkoutgit-resetgit-revert版本切换重设撤销的区别)
    - [`git cherry-pick`：遴选](#git-cherry-pick遴选)
    - [`git bisect`：调试](#git-bisect调试)
    - [`git submodule`：子模块](#git-submodule子模块)
  - [常用操作](#常用操作)
    - [已有一个项目，但之前未使用Git，将之推送到GitHub](#已有一个项目但之前未使用git将之推送到github)
    - [已有一个项目，并且已经推送到GitHub，现在更新了代码，再次推送](#已有一个项目并且已经推送到github现在更新了代码再次推送)
- [Git分支管理规范](#git分支管理规范)
- [Git 钩子](#git-钩子)
  - [`pre-commit`](#pre-commit)
    - [安装`pre-commit`](#安装pre-commit)
    - [常用指令](#常用指令)
    - [添加第三方 hooks](#添加第三方-hooks)
    - [跳过`pre-commit`继续提交代码](#跳过pre-commit继续提交代码)
- [Git忽略文件`.gitignore`的使用](#git忽略文件gitignore的使用)
- [常见问题](#常见问题)
  - [鉴权失败：添加GitHub个人访问令牌](#鉴权失败添加github个人访问令牌)
  - [error: 远程 origin 已经存在](#error-远程-origin-已经存在)
  - [fatal: 当前分支 main 没有对应的上游分支](#fatal-当前分支-main-没有对应的上游分支)
  - [error: 无法推送一些引用到 'https://github.com/xxxxx/xxxxx.git'](#error-无法推送一些引用到-httpsgithubcomxxxxxxxxxxgit)
- [参考资料](#参考资料)

-----------------

## Git基础知识

在这个问题前，我们需要了解版本控制的基本概念。

你可能在生活和工作中会遇到一个问题，那就是如果有一个文件，在使用过程中，我们会不断的修改它，但是有时候我们需要查看之前的版本，或者这次误删除了某个内容却由于各种原因无法撤销操作，需要恢复到之前的版本，那么我们该怎么办呢？

我们需要使用版本控制。

### 本地版本控制系统

许多人习惯用复制整个项目目录的方式来保存不同的版本，或许还会改名加上备份时间以示区别。这么做唯一的好处就是简单，但是特别容易犯错。有时候会混淆所在的工作目录，一不小心会写错文件或者覆盖意想外的文件。

为了解决这个问题，人们很久以前就开发了许多种本地版本控制系统，大多都是采用某种简单的数据库来记录文件的历次更新差异。

<img src="/Image/Basic%20Computer%20Knowledge/Git/git_12.png" title="NAME" height="70%" width="70%">

其中最流行的一种叫做`RCS`，现今许多计算机系统上都还看得到它的踪影。`RCS`的工作原理是在硬盘上保存补丁集（补丁是指文件修订前后的变化）；通过应用所有的补丁，可以重新计算出各个版本的文件内容。

### 集中化的版本控制系统

接下来人们又遇到一个问题，如何让在不同系统上的开发者协同工作？ 于是，集中化的版本控制系统（`Centralized Version Control Systems`，简称`CVCS`） 应运而生。 这类系统，诸如`CVS`、`Subversion`以及`Perforce`等，都有一个单一的集中管理的服务器，保存所有文件的修订版本，而协同工作的人们都通过客户端连到这台服务器，取出最新的文件或者提交更新。多年以来，这已成为版本控制系统的标准做法。

<img src="/Image/Basic%20Computer%20Knowledge/Git/git_13.png" title="NAME" height="70%" width="70%">

这种做法带来了许多好处，特别是相较于老式的本地`VCS`来说。现在，每个人都可以在一定程度上看到项目中的其他人正在做些什么。而管理员也可以轻松掌控每个开发者的权限，并且管理一个`CVCS`要远比在各个客户端上维护本地数据库来得轻松容易。

事分两面，有好有坏。这么做最显而易见的缺点是中央服务器的单点故障。如果宕机一小时，那么在这一小时内，谁都无法提交更新，也就无法协同工作。如果中心数据库所在的磁盘发生损坏，又没有做恰当备份，毫无疑问你将丢失所有数据——包括项目的整个变更历史，只剩下人们在各自机器上保留的单独快照。本地版本控制系统也存在类似问题，只要整个项目的历史记录被保存在单一位置，就有丢失所有历史更新记录的风险。

### 分布式版本控制系统

于是分布式版本控制系统（`Distributed Version Control System`，简称`DVCS`）面世了。在这类系统中，像`Git`、`Mercurial`、`Bazaar`以及`Darcs`等，客户端并不只提取最新版本的文件快照，而是把代码仓库完整地镜像下来，包括完整的历史记录。这么一来，任何一处协同工作用的服务器发生故障，事后都可以用任何一个镜像出来的本地仓库恢复。因为每一次的克隆操作，实际上都是一次对代码仓库的完整备份。

<img src="/Image/Basic%20Computer%20Knowledge/Git/git_14.png" title="NAME" height="70%" width="70%">

更进一步，许多这类系统都可以指定和若干不同的远端代码仓库进行交互。籍此，你就可以在同一个项目中，分别和不同工作小组的人相互协作。 你可以根据需要设定不同的协作流程，比如层次模型式的工作流，而这在以前的集中式系统中是无法实现的。

## Git安装

`ubuntu`安装最新版本的`git`：

```bash
sudo add-apt-repository ppa:git-core/ppa
sudo apt-get update
sudo apt install git
```

其他请参考
-  [Git安装教程 - Linux](https://git-scm.com/download/linux)
-  [Git安装教程 - Windows and MacOS](https://www.liaoxuefeng.com/wiki/896043488029600/896067074338496)

查看当前Git版本： `git --version`

## Git初次使用的配置

### 基础设置

- `git config`命令使用`--global`参数，可以设置全局的配置，如果不加`--global`参数，则只能设置当前目录的配置
- 这里我们设置为全局配置

```bash
# 配置所有Git仓库的用户名和邮箱
git config --global user.name "Your Name" # 输入你的用户名
git config --global user.email "youremail@example.com" # 输入你的邮箱

# 配置Git凭据存储模式为永久，防止GitHub提示鉴权失败
git config --global credential.helper store
```

- 可以通过`git config --list`查看你刚刚的配置

### 额外设置

```bash
# 配置Git默认编辑器为vim（也可以是别的，请了解Vim的基本使用）
git config --global core.editor "vim"

# 配置Git的默认分支，用以配合GitHub的变化（需要Git版本>=2.28）
git config --global init.defaultBranch main

# 配置GitHub代理（需要魔法上网）
git config --global http.https://github.com.proxy socks5://127.0.0.1:1080 # 1080改成自己的代理端口

# 忽略SSL证书错误
git config http.sslVerify false

# git 命令彩色显示
git config --global color.ui auto

# git 命令起别名，这里美化git log显示
# 实际使用用git lg替代git log
# 如： git lg -10 #显示最近10条提交
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

## 通过SSH密钥连接GitHub

1. 创建`SSH Key`
    ```bash
    ssh-keygen -t rsa -C "youremail@example.com" # 输入你的邮箱
    ```
2. 创建后会在`~/.ssh`目录下生成`id_rsa`和`id_rsa.pub`两个文件
    这两个就是生成的秘钥对，其中`id_rsa`是私钥，保存在自己设备上即可
    <img src="/Image/Basic%20Computer%20Knowledge/Git/git_07.png" title="NAME" height="50%" width="50%">
3. 路径： 头像->`Settings`->`SSH and GPG Keys`->点击`New SSH Key`->设置key名字和内容
    1. 头像->`Settings`
        <img src="/Image/Basic%20Computer%20Knowledge/Git/git_01.png" title="NAME" height="20%" width="20%">
    2. `Settings`->`SSH and GPG Keys`
        <img src="/Image/Basic%20Computer%20Knowledge/Git/git_08.png" title="NAME" height="50%" width="50%">
    3. 点击`New SSH Key`->设置key名字和内容
        <img src="/Image/Basic%20Computer%20Knowledge/Git/git_09.png" title="NAME" height="70%" width="70%">
4. 测试设置是否成功
    ```bash
    ssh -T git@github.com
    ```

## Git使用

- Git页面操作
    1. `D`：向下翻一行
    2. `F`：向下翻页
    3. `B`：向上翻页
    4. `Q`：退出

### 查看Git信息

```bash
# 查看系统配置
git config --list

# 查看用户配置
cat ~/.gitconfig 

# 查看当前项目的 git 配置
cat .git/config

# 查看暂存区的文件
git ls-files

# 查看本地 git 命令历史
git reflog

# 查看所有 git 命令
git --help -a 

# 查看当前 HEAD 指向
cat .git/HEAD

# 查看工作区和暂存区的状态
git status

# 查看提交历史
git log --oneline  
        --grep="关键字"
        --graph 
        --all      
        --author "username"     
        --reverse 
        -num
        -p
        --before=  1  day/1  week/1  "2023-01-01" 
        --after= "2023-01-01"
        --stat 
        --abbrev-commit 
        --pretty=format:"xxx"
        
# oneline -> 将日志记录一行一行的显示
# grep="关键字" -> 查找日志记录中(commit提交时的注释)与关键字有关的记录
# graph -> 记录图形化显示
# all -> 将所有记录都详细的显示出来
# author "username" -> 查找这个作者提交的记录
# reverse -> commit 提交记录顺序翻转      
# num -> git log -10 显示最近10次提交 
# p -> 显示每次提交所引入的差异（按 补丁 的格式输出）
# before -> 查找规定的时间(如:1天/1周)之前的记录   
# after -> 查找规定的时间之后的记录
# stat -> 显示每次更新的文件修改统计信息，会列出具体文件列表 
# abbrev-commit -> 仅显示 SHA-1 的前几个字符，而非所有的 40 个字符
# pretty=format:"xxx" ->  可以定制要显示的记录格式
```

#### `git reflog` 显示所有的操作记录
`git reflog`是显示所有的操作记录，包括提交，回退的操作
一般用来找出操作记录中的版本号，进行回退操作

- 显示的是一个`HEAD`指向发生改变的时间列表。
  在你切换分支、用`git commit`进行提交、以及用`git reset`撤销commit时，`HEAD`指向会改变，但当你进行`git checkout -- <filename>`撤销或者`git stash`存储文件等操作时，`HEAD`并不会改变，这些修改从来没有被提交过，因此`git reflog`也无法帮助我们恢复它们
- `git reflog`不会永远保持，Git会定期清理那些“用不到的”对象，不要指望几个月前的提交还一直在那里

#### `git log`的点线图

- git中一条分支就是一个指针，新建一条分支就是基于当前指针新建一个指针
- 切换至某个分支 ，就是将 HEAD 指向某条分支（指针）
- 切换至某个 commit ，就是将 HEAD 指向某个 commit
- 符号解释：
    1. `*`：表示一个 commit
    2. `|`：表示分支前进
    3. `/`：表示分叉
    4. `\`：表示合入
    5. `|/`：表示新分支


### Git常用命令
```bash
# 查看工作区和暂存区的状态
git status 

# 将工作区的文件提交到暂存区
git add .  

# 提交到本地仓库
git commit -m "本次提交说明"

# add和commit的合并，便捷写法（未追踪的文件无法直接提交到暂存区/本地仓库）
git commit -am "本次提交说明"  

# 将本地分支和远程分支进行关联
git push -u origin <branch_name> #填写分支名称，如：main

# 将本地仓库的文件推送到远程分支
git push

# 拉取远程分支的代码
git pull origin <branch_name> 

# 合并分支
git merge <branch_name> 

# 查看本地拥有哪些分支
git branch

# 查看所有分支（包括远程分支和本地分支）
git branch -a 

# 切换分支
git checkout <branch_name> 

# 临时将工作区文件的修改保存至堆栈中
git stash

# 将之前保存至堆栈中的文件取出来
git stash pop
```

### Git常用命令详解

#### `git add`：将工作区的文件添加到暂存区

```bash
# 添加指定文件到暂存区（追踪新增的指定文件）
git add <file1> <file2> <...> # 填写文件名

# 添加指定目录到暂存区，包括子目录
git add <dir> # 填写目录名

# 添加当前目录的所有文件到暂存区（追踪所有新增的文件）
git add .
git add -A
git add –all

# 提交所有被删除和修改的文件到数据暂存区
git add -u 
git add –update

# 删除工作区/暂存区的文件
git rm <file1> <file2> <...> # 填写文件名

# 停止追踪指定文件，但该文件会保留在工作区
git rm --cached <file> # 填写文件名

# 重命名工作区/暂存区的文件
git mv <file-original> <file-renamed> # 填写重命名之前的文件名和之后的文件名
```

- `git add .`及`git add -A`：
  - 操作的对象是“整个工作区”所有文件的变更，无论当前位于哪个目录下
- `git add -u`：
  - 操作的对象是整个工作区已经跟踪的文件变更，无论当前位于哪个目录下
    仅监控已经被`add`的文件（即`tracked file`），它会将被修改的文件（包括文件删除）提交到暂存区
    `git add -u`不会提交新文件（`untracked file`）

#### `git status`：查看工作区和暂存区的状态

```bash
# 查看工作区和暂存区的状态
git status 
```

#### `git commit`：将暂存区的文件提交到本地仓库并添加提交说明

```bash
# 将暂存区的文件提交到本地仓库并添加提交说明
git commit -m "本次提交的说明"   

# add 和 commit 的合并，便捷写法
# 和 git add -u 命令一样，未跟踪的文件是无法提交上去的
git commit -am "本次提交的说明"  

# 跳过验证继续提交
git commit --no-verify
git commit -n

# 编辑器会弹出上一次提交的信息，可以在这里修改提交信息
git commit --amend

# 修复提交，同时修改提交信息
git commit --amend -m "修改后的提交说明"

# 加入 --no-edit 标记会修复提交但不修改提交信息，编辑器不会弹出上一次提交的信息
git commit --amend --no-edit
```

- `git commit --amend`：
  - 既可以修改上次提交的文件内容，也可以修改上次提交的说明。会用一个新的 `commit` 更新并替换最近一次提交的`commit`
    如果暂存区有内容，这个新的`commit`会把任何修改内容和上一个`commit`的内容结合起来
    如果暂存区没有内容，那么这个操作就只会把上次的`commit`消息重写一遍
  - **永远不要修复一个已经推送到公共仓库中的提交，会拒绝推送到仓库**

#### `git push`&`git pull`：将本地仓库推送到远程仓库，或者从远程仓库拉取本地仓库

```bash
# 将本地仓库的文件推送到远程分支
# 如果远程仓库没有这个分支，会新建一个同名的远程分支
# 如果省略远程分支名，则表示两者同名
git push <远程主机名> <本地分支名>:<远程分支名>
git push origin <branch_name> #填写分支名称，如：main

# 如果省略本地分支名，则表示删除指定的远程分支
# 因为这等同于推送一个空的本地分支到远程分支。
git push origin :<branch_name>
# 等同于
git push origin --delete <branch_name>

# 建立当前分支和远程分支的追踪关系
git push -u origin <branch_name>

# 如果当前分支与远程分支之间存在追踪关系
# 则可以省略分支和 -u 
git push

# 不管是否存在对应的远程分支，将本地的所有分支都推送到远程主机
git push --all origin

# 拉取所有远程分支到本地镜像仓库中
git pull

# 拉取并合并项目其他人员的一个分支 
git pull origin <branch_name>  
# 等同于 fetch + merge
git fetch origin <branch_name>  
git merge origin/<branch_name>  

# 如果远程主机的版本比本地版本更新，推送时 Git 会报错，要求先在本地做 git pull 合并差异，
# 然后再推送到远程主机。这时，如果你一定要推送，可以使用 –-force 选项进行强制推送 
# ps. 尽量避免使用
git push -f origin
git push --force origin 
```

#### `git branch`：创建、删除、切换分支

```bash
# 查看本地分支
git branch
git branch -l 

# 查看远程分支
git branch -r 

# 查看所有分支（本地分支+远程分支）
git branch -a 

# 查看所有分支并带上最新的提交信息
git branch -av 

# 查看本地分支对应的远程分支
git branch -vv 

# 新建分支
# 在别的分支下新建一个分支，新分支会复制当前分支的内容
# 注意：如果当前分支有修改，但是没有提交到仓库，此时修改的内容是不会被复制到新分支的
git branch <branch_name> 
# 切换分支(切换分支时，本地工作区，仓库都会相应切换到对应分支的内容)
git checkout <branch_name> 

# 例子：创建一个 test 分支，并切换到该分支 （新建分支和切换分支的简写）
git checkout -b test
# 可以看做是基于 main 分支创建一个 test 分支，并切换到该分支
git checkout -b test main

# 新建一条空分支（详情请看问题列表）
git checkout --orphan empty<branch_name>
git rm -rf . 

# 删除本地分支,会阻止删除包含未合并更改的分支
git brnach -d <branch_name> 

# 强制删除一个本地分支，即使包含未合并更改的分支
git branch -D <branch_name>  

# 删除远程分支
# 推送一个空分支到远程分支，其实就相当于删除远程分支
git push origin  :远程分支名
# 或者
git push origin --delete 远程分支名 

# 修改当前分支名
git branch -m <branch_name> 
```

#### `git merge`：合并分支

```bash
# 默认 fast-forward ，HEAD 指针直接指向被合并的分支
git merge 

# 禁止快进式合并
git merge --no-ff 

git merge --squash 
```

- `fast-forward（默认）`：会在当前分支的提交历史中添加进被合并分支的提交历史
- `--no-ff`：会生成一个新的提交，让当前分支的提交历史不会那么乱
- `--squash`：不会生成新的提交，会将被合并分支多次提交的内容直接存到工作区和暂存区，由开发者手动去提交，这样当前分支最终只会多出一条提交记录，不会掺杂被合并分支的提交历史

#### `git rebase`：分支变基

<----wait to update---->

1. [rebase 用法小结](https://www.jianshu.com/p/4a8f4af4e803)
2. [Git分支-变基](https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%8F%98%E5%9F%BA)

#### `git stash`：保存和恢复进度

- 将修改的代码先暂存至堆栈中，让本地仓库回到最后一次提交时的状态，便于代码的更新管理，主要避免修改文件与最新代码的冲突
- 如果文件没有提交到暂存区（`git add`），使用该命令会提示`No local changes to save`，无法将修改保存到堆栈中
- 使用场景：当你接到一个修复紧急bug的任务时候，一般都是先创建一个新的bug分支来修复它，然后合并，最后删除。但是，如果当前你正在开发功能中，短时间还无法完成，无法直接提交到仓库，这时候可以先把当前工作区的内容`git stash`一下，然后去修复bug，修复后，再`git stash pop`，恢复之前的工作内容。

```bash
# 将所有未提交的修改（提交到暂存区）保存至堆栈中
git stash 

# 给本次存储加个备注，以防时间久了忘了
git stash save "存储"

# 存储未追踪的文件
git stash -u

# 查看存储记录
git stash list

# 恢复最新的进度到工作区
git stash pop
# 恢复后，stash 记录并不删除
git stash apply <stash@{index}>
# 恢复的同时把 stash 记录也删了
git stash pop <stash@{index}>
# 删除 stash 记录
git stash drop <stash@{index}>

# 删除所有存储的进度
git stash clear

# 查看当前记录中修改了哪些文件
git stash show <stash@{index}>
# 查看当前记录中修改了哪些文件的内容
git stash show -p <stash@{index}> 
```

#### `git diff`：查看工作区和暂存区的差异

```bash
# 查看工作区和暂存区所有文件的对比
git diff

# 查看工作区和暂存区单个文件的对比
git diff <file_name> 

# 查看工作区和暂存区所有文件的对比，并显示出所有有差异的文件列表
git diff --stat   

# 查看暂存区与上次提交到本地仓库的快照（即最新提交到本地仓库的快照）的对比
git diff --cached
git diff --staged

# 查看工作区与上次提交到本地仓库的快照（即最新提交到本地仓库的快照）的对比
git diff <branch_name>

# 查看工作区与 HEAD 指向（默认当前分支最新的提交）的对比
git diff HEAD   

# 查看两个本地分支中某一个文件的对比
git diff <branch_name>..<branch_name> <file_name>
# 查看两个本地分支所有的对比
git diff <branch_name>..<branch_name>

# 查看远程分支和本地分支的对比
git diff origin/<branch_name>..<branch_name>
# 查看远程分支和远程分支的对比
git diff origin/<branch_name>..origin/<branch_name>

# 查看两个 commit 的对比
git diff <commit1>..<commit2>
```

- 你修改了某个文件，但是没有提交到暂存区，这时候会有对比的内容
- 一旦提交到暂存区，就不会有对比的内容（因为暂存区已经更新）
- 如果你新建了一个文件，但是没有提交到暂存区，这时候`git diff`是没有结果的	

#### `git remote`：操作远程仓库

```bash
# 查看所有远程主机
git remote

# 查看关联的远程仓库的详细信息
git remote -v 

# 删除远程仓库的“关联”
git remote rm <project_name> #填写关联的仓库名称 如：origin

# 设置远程仓库的“关联”
git remote add <project_name> <url> #填写关联的仓库名称和仓库地址

# 修改仓库名称 
git remote rename <old_project_name> <new_project_name> # 如：origin test
```

#### `git tag`：操作标签

- 常用于发布版本

```bash
# 默认在 HEAD 上创建一个标签 
git tag <tag_name>
# 指定一个 commit id 创建一个标签 
git tag <tag_name> <commit_id>
# 创建带有说明的标签，用 -a 指定标签名，-m 指定说明文字
git tag -a <tag_name> -m "description" # 填写标签说明

# 查看所有标签
# 注意：标签不是按时间顺序列出，而是按字母排序的。
git tag
git tag -l

#查看远程所有标签
git ls-remote --tags origin

# 查看单个标签具体信息
git show <tag_name>

# 推送一个本地标签
git push origin <tag_name>
# 推送全部未推送过的本地标签
git push origin --tags

# 删除本地标签
# 因为创建的标签都只存储在本地，不会自动推送到远程。
# 所以，打错的标签可以在本地安全删除。
git tag -d v0.1
# 删除一个远程标签（先删除本地 tag ，然后再删除远程 tag）
git push origin :refs/tags/<tagname>

# 检出标签
git checkout <tag_name>
```

#### `git rm`：删除文件

```bash
# 删除暂存区和工作区的文件
git rm <file_name>  

# 只删除暂存区的文件，不会删除工作区的文件
git rm --cached <file_name> 
```

- 如果在配置`.gitignore`文件之前就把某个文件上传到远程仓库了，这时候想把远程仓库中的该文件删除，此时你配置`.gitignore`文件也没有用，因为该文件已经被追踪了，但又不想在本地删除该文件后再重新提交到远程仓库，这时候可以使用`git rm --cached filename`命令取消该文件的追踪，这样下次提交的时候，git就不会再提交这个文件，从而远程仓库的该文件也会被删除

#### `git checkout`：切换分支

```bash
# 恢复暂存区的指定文件到工作区
git checkout <file_name>

# 恢复暂存区的所有文件到工作区
git checkout .

# 回滚到最近的一次提交
# 如果修改某些文件后，没有提交到暂存区，此时的回滚是回滚到上一次提交
# 如果是已经将修改的文件提交到仓库了，这时再用这个命令回滚无效
# 因为回滚到的是之前自己修改后提交的版本
git checkout HEAD 
git checkout HEAD -- <file_name>

# 回滚到最近一次提交的上一个版本
git checkout HEAD^ 
# 回滚到最近一次提交的上2个版本
git checkout HEAD^^ 

# 切换分支，在这里也可以看做是回到项目「当前」状态的方式
git checkout <branch_name>

# 切换到某个指定的 commit 版本
git checkout <commit_id>

# 切换指定 tag 
git checkout <tag>
```

- 在开发的正常阶段，`HEAD`一般指向`main`或是其他的本地分支，但当你使用`git checkout <commit id>`切换到指定的某一次提交的时候，`HEAD`就不再指向一个分支了——它直接指向一个提交，`HEAD`就会处于`detached`状态（游离状态）
- 切换到某一次提交后，你可以查看文件，编译项目，运行测试，甚至编辑文件而不需要考虑是否会影响项目的当前状态，你所做的一切都不会被保存到主栈的仓库中。当你想要回到主线继续开发时，使用`git checkout <branch_name>`回到项目初始的状态（这时候会提示你是否需要新建一条分支用于保留刚才的修改）
- 哪怕你切换到了某一版本的提交，并且对它做了修改后，不小心提交到了暂存区，只要你切换回分支的时候，依然会回到项目的初始状态。（注意：你所做的修改，如果 commit 了，会被保存到那个版本中。切换完分支后，会提示你是否要新建一个分支来保存刚才修改的内容。如果你刚才解决了一个bug，这时候可以新建一个临时分支，然后你本地自己的开发主分支去合并它，合并完后删除临时分支）
- 一般使用`git checkout`回退版本，查看历史代码，测试bug在哪

#### `git reset`：回退版本

- `git reset [--hard|soft|mixed|merge|keep] [<commit>或HEAD]`：将当前的分支重设(`reset`)到指定的`<commit>`或者`HEAD`(默认，如果不显示指定`<commit>`，默认是`HEAD`，即最新的一次提交)，并且根据`[mode]`有可能更新索引和工作目录。
- mode的取值可以是`hard、soft、mixed、merged、keep`

```bash
# 从暂存区撤销特定文件，但不改变工作区。它会取消这个文件的暂存，而不覆盖任何更改
git reset <file_name>
# 重置暂存区最近的一次提交，但工作区的文件不变
git reset 
# 等价于 
git reset HEAD

# 重置暂存区与工作区，回退到最近一次提交的版本内容
git reset --hard 
# 重置暂存区与工作区，回退到最近一次提交的上一个版本
git reset --hard HEAD^ 

# 将当前分支的指针指向为指定 commit（该提交之后的提交都会被移除），同时重置暂存区，但工作区不变
git reset <commit_id>
# 等价于 
git reset --mixed  <commit_id>

# 将当前分支的指针指向为指定 commit（该提交之后的提交都会被移除），但保持暂存区和工作区不变
git reset --soft  <commit_id>

# 将当前分支的指针指向为指定 commit（该提交之后的提交都会被移除），同时重置暂存区、工作区
git reset --hard <commit_id>
```

- `git reset`有很多种用法。它可以被用来移除提交快照，尽管它通常被用来撤销暂存区和工作区的修改
    不管是哪种情况，它应该只被用于本地修改——你永远不应该重设和其他开发者共享的快照
- 当你用 reset 回滚到了某个版本后，那么在下一次 git 提交时，之前该版本后面的版本会被作为垃圾删掉
- 当我们回退到一个旧版本后，此时再用 git log 查看提交记录，会发现之前的新版本记录没有了。如果第二天，你又想恢复到新版本怎么办？找不到新版本的 `<commit_id>`怎么办？
  - 我们可以用`git reflog`查看历史命令，这样就可以看到之前新版本的 `<commit_id>`，然后`git reset --hard <commit_id>`就可以回到之前的新版本代码
  - 虽然可以用`git reflog`查看本地历史，然后回复到之前的新版本代码，但是在别的电脑上是无法获取你的历史命令的，所以这种方法不安全。万一你的电脑突然坏了，这时候就无法回到未来的版本

#### `git revert`：回退版本但不回退之前的提交

```bash
# 生成一个撤销最近的一次提交的新提交
 git revert HEAD 
# 生成一个撤销最近一次提交的上一次提交的新提交
 git revert HEAD^ 
# 生成一个撤销最近一次提交的上两次提交的新提交
 git revert HEAD^^ 
# 生成一个撤销最近一次提交的上n次提交的新提交
 git revert HEAD~num 

# 生成一个撤销指定提交版本的新提交
 git revert <commit_id>
# 生成一个撤销指定提交版本的新提交，执行时不打开默认编辑器，直接使用 Git 自动生成的提交信息
 git revert <commit_id> --no-edit
```

- `git revert`命令用来撤销某个已经提交的快照（和`git reset`重置到某个指定版本不一样）。它是在提交记录最后面加上一个撤销了更改的新提交，而不是从项目历史中移除这个提交，这避免了Git丢失项目历史
- 撤销（revert）应该用在你想要在项目历史中移除某个提交的时候。比如说，你在追踪一个bug，然后你发现它是由一个提交造成的，这时候撤销就很有用
- 撤销（revert）被设计为撤销公共提交的安全方式，重设（reset）被设计为重设本地更改
  - 因为两个命令的目的不同，它们的实现也不一样：重设完全地移除了一堆更改，而撤销保留了原来的更改，用一个新的提交来实现撤销。
  - 千万不要用`git reset`回退已经被推送到公共仓库上的提交，它只适用于回退本地修改（从未提交到公共仓库中）。如果你需要修复一个公共提交，最好使用`git revert`
- 发布一个提交之后，你必须假设其他开发者会依赖于它。移除一个其他团队成员在上面继续开发的提交在协作时会引发严重的问题。当他们试着和你的仓库同步时，他们会发现项目历史的一部分突然消失了。一旦你在重设之后又增加了新的提交，Git 会认为你的本地历史已经和`origin/main`分叉了，同步你的仓库时的合并提交(merge commit)会使你的同事困惑。

#### `git checkout`&`git reset`&`git revert`：版本切换&重设&撤销的区别

- `checkout`可以撤销工作区的文件，`reset`可以撤销工作区/暂存区的文件
- `reset`和`checkout`可以作用于`commit`或者文件，`revert`只能作用于`commit`
- `git revert`命令通过创建一次新的`commit`来撤销一次`commit`所做出的修改。这种撤销的方式是安全的，因为它并不修改commitm history, 比如下边的命令将会查出倒数第二次（即当前`commit`的往前一次）提交的修改，并创建一个新的提交，用于撤销当前提交的上一次`commit`）
- `git reset`操作之后的`commit`都不会被保留
- `git checkout`用于切分支或者`<commit_id>`

#### `git cherry-pick`：遴选
- 将指定的提交`commit`应用于当前分支（可以用于恢复不小心撤销（`git revert`/`git reset`）的提交）

```bash
git cherry-pick <commit_id>
git cherry-pick <commit_id> <commit_id>
git cherry-pick <commit_id>^..<commit_id>
```

#### `git bisect`：调试

- 快速找出有bug的`commit`
- 它的原理很简单，就是将代码提交的历史，按照两分法不断缩小定位。所谓"两分法"，就是将代码历史一分为二，确定问题出在前半部分，还是后半部分，不断执行这个过程，直到范围缩小到某一次代码提交

```bash
# "终点"是最近的提交，"起点"是更久以前的提交
git bisect start [终点] [起点]
git bisect start HEAD commit_id

# 标识本次提交没有问题
git bisect good
# 标识本次提交（第76）有问题
git bisect bad

# 退出查错，回到最近一次的代码提交
git bisect reset
```

#### `git submodule`：子模块

- 有种情况我们经常会遇到：某个工作中的项目需要包含并使用另一个项目。也许是第三方库，或者你独立开发的，用于多个父项目的库。 现在问题来了：你想要把它们当做两个独立的项目，同时又想在一个项目中使用另一个。如果将另外一个项目中的代码复制到自己的项目中，那么你做的任何自定义修改都会使合并上游的改动变得困难
- Git通过子模块来解决这个问题，允许你将一个Git仓库作为另一个Git仓库的子目录。它能让你将另一个仓库克隆到自己的项目中，同时还保持提交的独立

```bash
# 在主项目中添加子项目，url 为子模块的路径，path 为该子模块存储的目录路径
git submodule add <url> <path>

# 克隆含有子项目的主项目
git clone <url>

# 当你在克隆这样的项目时，默认会包含该子项目的目录，但该目录中还没有任何文件
# 初始化本地配置文件
git submodule init
# 从当前项目中抓取所有数据并检出父项目中列出的合适的提交
git submodule update
# 等价于 git submodule init && git submodule update
git submodule update --init

# 自动初始化并更新仓库中的每一个子模块， 包括可能存在的嵌套子模块
git clone --recurse-submodules <url>
```

### 常用操作

#### 已有一个项目，但之前未使用Git，将之推送到GitHub
1. 去GitHub上创建一个新的项目
   1. 路径：`Repositories`->`New`->填写仓库信息->点击`Create repository`
      1. `Repositories`->`New`
        <img src="/Image/Basic%20Computer%20Knowledge/Git/git_10.png" title="NAME" height="100%" width="100%">
      2. `New`->填写仓库信息
        <img src="/Image/Basic%20Computer%20Knowledge/Git/git_11.png" title="NAME" height="100%" width="100%">
      3. 点击`Create repository`
2. 设置本地仓库（需要在项目文件夹下）
    ```bash
    git init # 初始化仓库
    git add . # 添加所有文件
    git commit -m "first commit" # 提交，填写本次提交说明
    git remote add origin <url> # 填写第一步获得的链接 如：git remote add origin https://github.com/XIADENGMA/test.git
    git push -u origin main # 推送到远程仓库 
    ```
3. 至此，项目已经推送到GitHub，并且本地仓库已经设置好

#### 已有一个项目，并且已经推送到GitHub，现在更新了代码，再次推送

```bash
git add . # 添加所有文件
git commit -m "update code" # 提交，填写本次提交说明
git push
```


## Git分支管理规范

- 实际开发的时候，一人一条分支（个人见解：除非是大项目，参与的开发人员很多时，可以采用`feature`分支，否则一般的项目中，一个开发者一条分支够用了）。除此之外还要有一条`develop`开发分支，一条`test`测试分支，一条`release`预发布分支
  - `develop`：开发分支，开发人员每天都需要拉取/提交最新代码的分支
  - `test`：测试分支，开发人员开发完并自测通过后，发布到测试环境的分支
  - `release`：预发布分支，测试环境测试通过后，将测试分支的代码发布到预发环境的分支（这个得看公司支不支持预发环境，没有的话就可以不采用这条分支）
  - `main`：线上分支，预发环境测试通过后，运营/测试会将此分支代码发布到线上环境
- 大致流程：
  - 开发人员每天都需要拉取/提交最新的代码到 develop 分支；
  - 开发人员开发完毕，开始 集成测试，测试无误后提交到 test 分支并发布到测试环境，交由测试人员测试
  - 测试环境通过后，发布到`release`分支 上，进行预发环境测试
  - 预发环境通过后，发布到`master`分支上并打上标签（`tag`）
  - 如果线上分支出了bug，这时候相关开发者应该基于预发布分支（没有预发环境，就使用`master`分支），新建一个`bug`分支用来临时解决bug，处理完后申请合并到`release`分支。这样做的好处就是：不会影响正在开发中的功能
- 预发布环境的作用：预发布环境是正式发布前最后一次测试。因为在少数情况下即使预发布通过了，都不能保证正式生产环境可以100%不出问题；预发布环境的配置，数据库等都是跟线上一样；有些公司的预发布环境数据库是连接线上环境，有些公司预发布环境是单独的数据库；如果不设预发布环境，如果开发合并代码有问题，会直接将问题发布到线上，增加维护的成本

## Git 钩子

[在 Git 项目中使用 pre-commit 统一管理 hooks](https://blog.csdn.net/DoneSpeak/article/details/118469221)
[自定义 Git - Git 钩子](https://git-scm.com/book/zh/v2/%E8%87%AA%E5%AE%9A%E4%B9%89-Git-Git-%E9%92%A9%E5%AD%90)

- Git基本已经成为项目开发中默认的版本管理软件，在使用Git的项目中，我们可以为项目设置`Git Hooks`来帮我们在提交代码的各个阶段做一些代码检查等工作
- 钩子（Hooks）都被存储在`Git`目录下的`hooks`子目录中。也就是绝大部分项目中的`.git/hook`目录
- 钩子分为两大类，客户端的和服务器端的
  - 客户端钩子主要被提交和合并这样的操作所调用
  - 而服务器端钩子作用于接收被推送的提交这样的联网操作，这里主要介绍客户端钩子

### `pre-commit`

- `pre-commit`就是在代码提交之前做些东西，比如代码打包，代码检测，称之为钩子（hook）
- 在`commit`之前执行一个回调函数（`callback`）。这个函数成功执行完之后，再继续`commit`，但是失败之后就阻止`commit`
- 在`.git->hooks`内有个`pre-commit.sample*`，这个里面就是默认的函数(脚本)样本

#### 安装`pre-commit`

- 在系统中安装`pre-commit`
    ```bash
    pip install pre-commit
    ```
- 在项目中安装`pre-commit`
    ```bash
    cd <git-repo>
    pre-commit install
    # 卸载
    pre-commit uninstall
    ```

#### 常用指令
```bash
# 手动对所有的文件执行hooks，新增hook的时候可以执行，使得代码均符合规范。直接执行该指令则无需等到pre-commit阶段再触发hooks
pre-commit run --all-files

# 执行特定hooks
pre-commit run <hook_id>

# 将所有的hook更新到最新的版本/tag
pre-commit autoupdate

# 指定更新repo
pre-commit autoupdate --repo https://github.com/XIADNEMGA/gromithooks
```

#### 添加第三方 hooks

<----wait to update---->

#### 跳过`pre-commit`继续提交代码

```bash
# 跳过验证
git commit --no-verify
git commit -n
```

## Git忽略文件`.gitignore`的使用

<----wait to update---->

## 常见问题

### 鉴权失败：添加GitHub个人访问令牌

如果`git push`显示`fatal: 'https://github.com/XIADENGMA/MyNotes.git/' 鉴权失败`，则按照下面的步骤操作：

- 注意：[基础设置：设置Git凭据存储模式为永久，防止GitHub提示鉴权失败](#基础设置)

1. 路径： 头像->`Settings`->`Developer settings`->`Personal access tokens`->点击`Generate new token`->设置token名字、过期时间和权限->复制token->再次运行`git push`，填写帐号和token
   1. 头像->`Settings`
       <img src="/Image/Basic%20Computer%20Knowledge/Git/git_01.png" title="NAME" height="20%" width="20%">
   2. `Settings`->`Developer settings`
       <img src="/Image/Basic%20Computer%20Knowledge/Git/git_02.png" title="NAME" height="50%" width="50%">
   3. `Developer settings`->`Personal access tokens`
       <img src="/Image/Basic%20Computer%20Knowledge/Git/git_03.png" title="NAME" height="50%" width="50%">
   4. 点击`Generate new token`->设置token名字、过期时间和权限->复制token
       <img src="/Image/Basic%20Computer%20Knowledge/Git/git_04.png" title="NAME" height="100%" width="100%">
   5. 复制token->运行`git push`，填写帐号和token
       <img src="/Image/Basic%20Computer%20Knowledge/Git/git_05.png" title="NAME" height="100%" width="100%">

### error: 远程 origin 已经存在

可能是不小心将git远程地址配错了

- 解决方法：将远程配置删除，重新添加
    ```bash
    git remote rm origin
    git remote add origin https://github.com/xxxx/xxxx.git
    ```

### fatal: 当前分支 main 没有对应的上游分支

- 解决方法：
    ```bash
    git push --set-upstream origin <branch_name> #如：main
    ```

### error: 无法推送一些引用到 'https://github.com/xxxxx/xxxxx.git'

- 解决方法：强制推送
    ```bash
    git push -u origin +<branch_name> #如：main
    ```


<----wait to update---->


## 参考资料

- [多年 Git 使用心得 & 常见问题整理](https://juejin.cn/post/6844904191203213326#heading-6)
- [Git 教程-廖雪峰](https://www.liaoxuefeng.com/wiki/896043488029600)
- [Pro Git](https://git-scm.com/book/zh/v2)
