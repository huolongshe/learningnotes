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
    
        文件/etc/resolv.conf：
        nameserver 127.0.1.1
        nameserver 202.101.46.151

7. 更新apt-get包索引，并安装一些必需工具：

        # apt-get purge libappstream3
        # apt-get update
        # apt-get install aufs-tools
        # apt-get install curl
        # apt-get install python-pip

