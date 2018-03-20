## 安装配置FTP服务

1. 安装vsftpd

        # apt-get install vsftpd

2. 修改/etc/vsftpd.conf文件，各配置项按如下进行设置：

        文件/etc/vsftpd.conf：
        listen=NO
        local_enable=YES
        write_enable=YES
        # 新增
        userlist_deny=NO
        userlist_enable=YES
        userlist_file=/etc/allowed_users
        seccomp_sandbox=NO

3. 新建/etc/allowed_users文件，在其中添加允许FTP访问的用户（root，ubuntu）。

4. 编辑 /etc/ftpusers文件（不允许FTP访问的用户列表），将root删除。

5. 重启vsftpd服务：

        # service vsftpd restart
        
> 以后可以在宿主机上使用FTP客户端进行访问，相互传递文件了。Windows中建议使用Xftp。


