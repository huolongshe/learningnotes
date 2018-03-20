## Ubuntu初始环境配置

1. 启动虚机，进入登录界面，输入用户名、口令登录。
2. 在菜单栏选择“设备 | 安装增强功能...”。安装完毕后重启系统，然后就可以在宿主机与虚机之间进行copy/paste操作了。
3. 配置超级用户（root）口令：

        $ sudo passwd root

4. 配置使可在普通用户与超级用户（root）之间自由切换：

    在/etc/sudoers文件中增加一行（其中ubuntu为当前用户名）： 
    
        $ sudo vi /etc/sudoers
        ...
        ubuntu ALL=(ALL) NOPASSWD:ALL`
                    
    之后在普通用户命令行中随时输入“sudo -i”，就可以切换至root登录状态。

        $ sudo -i
        # exit
        $
        
5. 增加一个本地DNS服务器地址，方便网络访问。

    修改/etc/resolv.conf配置文件，修改后内容如下（举例）:
    
        nameserver 127.0.1.1
        nameserver 202.101.46.151

7. 更新apt-get包索引，并安装一些必需工具：

        # apt-get purge libappstream3
        # apt-get update
        # apt-get install aufs-tools
        # apt-get install curl
        # apt-get install python-pip



10. 安装配置FTP服务

    1. 安装vsftpd
    
        `# apt-get install vsftpd`

    2. 修改/etc/vsftpd.conf文件，各配置项按如下进行设置：
    
        ```
        listen=NO
        local_enable=YES
        write_enable=YES
        userlist_deny=NO
        userlist_enable=YES
        userlist_file=/etc/allowed_users
        seccomp_sandbox=NO
        ```

    3. 新建/etc/allowed_users文件，在其中添加允许FTP访问的用户（root，ubuntu）。
    
    4. 编辑 /etc/ftpusers文件（不允许FTP访问的用户列表），将root删除。
    
    5. 重启vsftpd服务：

        '# service vsftpd restart'

    > 以后可以在宿主机上使用FTP工具进行访问，相互传递文件了。Windows中建议使用Xftp。
