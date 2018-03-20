## 安装配置sshd服务

1. 安装sshd

    # apt-get install openssh-server

2. 配置使root用户可远程SSH登录

    编辑/etc/ssh/sshd_config文件：

    FROM:
    PermitRootLogin prohibit-password
    TO:
    PermitRootLogin yes

3. 重启sshd服务：

     # service sshd restart

> 以后可以在宿主机上使用ssh工具进行登录。Windows中建议使用Xshell。

> 为了操作方便，后续所有操作全部在超级用户（root）下进行。

