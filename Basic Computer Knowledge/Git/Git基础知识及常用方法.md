# Git基础知识及常用方法


---------------------

- [Git安装](#git安装)
- [初次运行前的配置](#初次运行前的配置)
  - [基础设置](#基础设置)
  - [额外设置](#额外设置)
- [通过SSH密钥连接GitHub](#通过ssh密钥连接github)
- [Git使用](#git使用)
  - [获取Git仓库](#获取git仓库)
    - [在工作目录中初始化新仓库，并链接到远程仓库](#在工作目录中初始化新仓库并链接到远程仓库)
    - [从现有的仓库拉取代码](#从现有的仓库拉取代码)
  - [----wait update----](#----wait-update----)
- [鉴权失败：添加GitHub个人访问令牌](#鉴权失败添加github个人访问令牌)

-----------------

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

## 初次运行前的配置

### 基础设置

- `git config`命令使用`--global`参数，可以设置全局的配置，如果不加`--global`参数，则只能设置当前目录的配置
- 这里我们设置为全局配置

```bash
#配置所有Git仓库的用户名和邮箱
git config --global user.name "Your Name" #输入你的用户名
git config --global user.email "youremail@example.com" #输入你的邮箱

#配置Git默认编辑器为vim（也可以是别的）
git config --global core.editor "vim"

#配置Git凭据存储模式为永久，防止GitHub提示鉴权失败
git config --global credential.helper store
```

- 可以通过`git config --list`查看你刚刚的配置

### 额外设置

```bash
#配置Git的默认分支，用以配合GitHub的变化（需要Git版本>=2.28）
git config --global init.defaultBranch main

#配置GitHub代理（需要魔法上网）
git config --global http.https://github.com.proxy socks5://127.0.0.1:1080 #1080改成自己的代理端口

#忽略SSL证书错误
git config http.sslVerify false
```

## 通过SSH密钥连接GitHub

1. 创建`SSH Key`
    ```bash
    ssh-keygen -t rsa -C "youremail@example.com" #输入你的邮箱
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

### 获取Git仓库

#### 在工作目录中初始化新仓库，并链接到远程仓库

1. 要对现有的某个项目开始用`Git`管理，只需到此项目所在的目录进行初始化，执行：
    ```bash
    git init
    ```
2. 添加文件到暂存区
    ```bash
    git add <file> # 如：git add README.md
    ```
    ps：使用`git add -A`或`git add .`可以提交当前仓库的所有改动
3. 提交文件到仓库
    ```bash
    git commit -m "description" # 添加备注信息 如：git commit -m "add README.md"
    ```
4. 本地仓库链接GitHub仓库
    ```bash
    git remote add origin <url> # 如：git remote add origin git@github.com:XIADENGMA/learngit.git
    ```
5. 提交代码到GitHub
    ```bash
    git push -u origin main
    ```
    - 如果显示鉴权失败，查看 [鉴权失败解决方法](#鉴权失败：添加GitHub个人访问令牌)

#### 从现有的仓库拉取代码

1. 从远程仓库上克隆项目到本地（到指定目录下运行）
    ```bash
    git clone <url> # 如：git clone https://github.com/XIADENGMA/MyNotes.git
    ```

### ----wait update----

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
