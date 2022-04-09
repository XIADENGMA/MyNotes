# Git基础知识及常用方法


---------------------

- [Git安装](#git安装)
- [初次运行前的配置](#初次运行前的配置)
  - [基础设置](#基础设置)
  - [额外设置](#额外设置)
- [添加GitHub个人访问令牌](#添加github个人访问令牌)

-----------------

## Git安装

```text
sudo add-apt-repository ppa:git-core/ppa
sudo apt-get update
sudo apt install git
```

其他请参考
> [Git安装教程 - Linux](https://git-scm.com/download/linux)
  [Git安装教程 - Windows and MacOS](https://www.liaoxuefeng.com/wiki/896043488029600/896067074338496)

查看当前Git版本： `git --version`

## 初次运行前的配置

### 基础设置

- `git config`命令使用`--global`参数，可以设置全局的配置，如果不加`--global`参数，则只能设置当前目录的配置
- 这里我们设置为全局配置

```text
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

```text
#配置Git的默认分支，用以配合GitHub的变化（需要Git版本>=2.28）
git config --global init.defaultBranch main

#配置GitHub代理（需要魔法上网）
git config --global http.https://github.com.proxy socks5://127.0.0.1:1080 #1080改成自己的代理端口

#忽略SSL证书错误
git config http.sslVerify false
```


## 添加GitHub个人访问令牌

```text
#测试克隆GitHub仓库
git clone https://github.com/XIADENGMA/MyNotes.git
```

如果显示`fatal: 'https://github.com/XIADENGMA/MyNotes.git/' 鉴权失败`，则按照下面的步骤操作

0. 路径： 头像->`Settings`->`Developer settings`->`Personal access tokens`->点击`Generate new token`->设置token名字、过期时间和权限->复制token->再次运行`git push`，填写帐号和token
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
