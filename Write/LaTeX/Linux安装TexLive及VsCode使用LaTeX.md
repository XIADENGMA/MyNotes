# Linux安装TexLive及VsCode使用LaTeX

---------------------

- [安装TexLive](#安装texlive)
- [VsCode使用LaTeX](#vscode使用latex)
- [在线编辑LaTeX](#在线编辑latex)
- [参考资料](#参考资料)

---------------------

## 安装TexLive

1. 先下载texlive镜像文件（2022，你可以换成你想要的版本）
    - [清华镜像（推荐）](https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/Images/texlive2022-20220321.iso)：<https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/Images/texlive2022-20220321.iso>
    - [HK镜像（境外使用）](https://mirror-hk.koddos.net/CTAN/systems/texlive/Images/texlive2022.iso)：<https://mirror-hk.koddos.net/CTAN/systems/texlive/Images/texlive2022.iso>
    - [中科大镜像](https://mirrors.ustc.edu.cn/CTAN/systems/texlive/Images/texlive2022.iso)：<https://mirrors.ustc.edu.cn/CTAN/systems/texlive/Images/texlive2022.iso>
2. 挂载镜像文件，并进入挂载点
   1. 首先你要知道你镜像文件下载在了哪里
        ```bash
        $ cd /home/xiadengma/download #这里应该是你的下载地址
        $ ls #显示文件
        texlive2022-20220321.iso
        ```
   2. 挂载镜像
        ```bash
        $ mkdir ~/iso #创建iso文件夹
        $ mount -o loop /home/xiadengma/download/texlive2022-20220321.iso ~/iso #挂载镜像
        ```
3. 安装TexLive
    ```bash
    cd ~/iso #进入iso文件夹
    sudo ./install-tl #执行安装命令
    ```
    - 等待命令行出现`Enter command：`，然后输入`I`，并按下回车键
    - 等待安装即可
    - 直到出现`欢迎进入 TeX Live 的世界`或者`welcome to texlive`即为安装成功
4. 添加环境变量
    - 打开当前用户配置文件
        ```bash
        sudo gedit ~/.bashrc #进入bashrc文件，如果你用的是zsh，换成～/.zshrc
        ```
    - 然后在末尾添加以下内容（这个是2022版本，如果不是，换成对应的版本）：
        ```text
        export PATH=/usr/local/texlive/2022/bin/x86_64-linux:$ PATH
        export MANPATH=/usr/local/texlive/2022/texmf-dist/doc/man:$MANPATH
        export INFOPATH=/usr/local/texlive/2022/texmf-dist/doc/info:$INFOPATH
        ```
    - 同样，打开`man`配置文件
        ```bash
        sudo gedit /etc/manpath.config
        ```
    - 在对应位置添加以下内容（这个是2022版本，如果不是，换成对应的版本）：
        ```text
        MANPATH_MAP /usr/local/texlive/2022/bin/x86_64-linux /usr/local/texlive/2022/texmf-dist/doc/man
        ```
        <img src="/Image/Write/LaTeX/latex_01.png" title="NAME" height="70%" width="70%">
    - 保存退出
5. 验证安装

- 重新打开一个终端，输入
    ```bash
    tex -v
    ```
- 得到如下界面，则表示安装成功
    - <img src="/Image/Write/LaTeX/latex_02.png" title="NAME" height="70%" width="70%">

## VsCode使用LaTeX

## 在线编辑LaTeX

- [Slager]()
- [Overleaf]()

## 参考资料
- [texlive-for-linux](https://ljguo1020.github.io/2021/12/28/texlive-for-linux/)
- [使用 Vscode 配置 LaTeX](https://ljguo1020.github.io/2022/02/25/LaTeX-vscode/)
- [使用 VSCode 编写 LaTeX](https://zhuanlan.zhihu.com/p/38178015)
