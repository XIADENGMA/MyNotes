# Git基础知识及常用命令

---------------------

- [Git基础知识](#git基础知识)
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
  - [基础使用：已有一个项目，但之前未使用Git，将之推送到GitHub](#基础使用已有一个项目但之前未使用git将之推送到github)
- [鉴权失败：添加GitHub个人访问令牌](#鉴权失败添加github个人访问令牌)

-----------------

## Git基础知识

<----wait to update---->

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

# 这里美化git log显示
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
    <img src="/Image/Basic%20Computer%20Knowledge/Git/git_07.png" title="NAME" height="100%" width="1100%">
3. 路径： 头像->`Settings`->`SSH and GPG Keys`->点击`New SSH Key`->设置key名字和内容
    1. 头像->`Settings`
        <img src="/Image/Basic%20Computer%20Knowledge/Git/git_01.png" title="NAME" height="30%" width="30%">
    2. `Settings`->`SSH and GPG Keys`
        <img src="/Image/Basic%20Computer%20Knowledge/Git/git_08.png" title="NAME" height="50%" width="50%">
    3. 点击`New SSH Key`->设置key名字和内容
        <img src="/Image/Basic%20Computer%20Knowledge/Git/git_09.png" title="NAME" height="100%" width="100%">
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

### 基础使用：已有一个项目，但之前未使用Git，将之推送到GitHub
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

## 鉴权失败：添加GitHub个人访问令牌

如果`git push`显示`fatal: 'https://github.com/XIADENGMA/MyNotes.git/' 鉴权失败`，则按照下面的步骤操作：

- 注意：[基础设置：设置Git凭据存储模式为永久，防止GitHub提示鉴权失败](#基础设置)

1. 路径： 头像->`Settings`->`Developer settings`->`Personal access tokens`->点击`Generate new token`->设置token名字、过期时间和权限->复制token->再次运行`git push`，填写帐号和token
   1. 头像->`Settings`
       <img src="/Image/Basic%20Computer%20Knowledge/Git/git_01.png" title="NAME" height="30%" width="30%">
   2. `Settings`->`Developer settings`
       <img src="/Image/Basic%20Computer%20Knowledge/Git/git_02.png" title="NAME" height="50%" width="50%">
   3. `Developer settings`->`Personal access tokens`
       <img src="/Image/Basic%20Computer%20Knowledge/Git/git_03.png" title="NAME" height="50%" width="50%">
   4. 点击`Generate new token`->设置token名字、过期时间和权限->复制token
       <img src="/Image/Basic%20Computer%20Knowledge/Git/git_04.png" title="NAME" height="100%" width="100%">
   5. 复制token->运行`git push`，填写帐号和token
       <img src="/Image/Basic%20Computer%20Knowledge/Git/git_05.png" title="NAME" height="100%" width="100%">
