## Root登录图形界面
	
1. 安装lightdm和lightdm-gtk-greeter

        # apt-get install lightdm
        # apt-get install lightdm-gtk-greeter

2. 安装xubuntu-desktop

        # apt-get install xubuntu-desktop

3. 修改lightdm配置参数

    修改/usr/share/lightdm/lightdm.conf.d/50-unity-greeter.conf配置文件，修改后内容如下：：

        [Seat:*]
        greeter-session=unity-greeter 
        #added by myself
        greeter-show-manual-login=true
        xserver-allow-tcp=true
        allow-guest=false
        
        [XDMCPServer]
        enabled=true
        port=177

4. 修改root用户启动脚本

        修改文件/root/.profile，将最后一行 ：
        mesg n || true
        改为：
        tty -s && mesg n || true

5. 配置防火墙，使能177端口

        # ufw allow 177

6. 卸载gnome-shell

        # apt-get remove gnome-shell

7. reboot，在登录界面选择other users，输入root的用户名口令登录

8. 检查XDMCPServer服务启动情况：

        # lsof -i:177

9. 在Windows中运行XManager组件中的Xbrowser，远程登录Linux图形界面。

