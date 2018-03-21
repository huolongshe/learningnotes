## 安装配置MariaDB数据库

### Linux系统

> 参考：https://mariadb.com/kb/en/library/installing-mariadb-binary-tarballs/

1. 下载

    http://mirrors.tuna.tsinghua.edu.cn/mariadb/mariadb-10.2.13/bintar-linux-x86_64/mariadb-10.2.13-linux-x86_64.tar.gz

2. 安装

	拷贝至：/usr/local，解压缩，创建目录符号链接：

        # tar -zxvpf mariadb-10.2.13-linux-x86_64.tar.gz
        # ln -s mariadb-10.2.13-linux-x86_64 mysql


3. 创建mysql组和用户，安装数据库：

        # groupadd mysql
        # useradd -g mysql mysql
        # cd mysql
        # ./scripts/mysql_install_db --user=mysql （如果安装失败，执行“apt-get install libaio1”后重新安装）
        # chown -R root .
        # chown -R mysql data

4. 配置环境变量

    在/etc/profile文件末尾追加一行：

        文件/etc/profile：
        export PATH=$PATH:/usr/local/mysql/bin/
        
    然后重启或运行 source /etc/profile 使其生效：
    
         # source /etc/profile

5. 配置参数

    Ubuntu中MariaDB的配置文件位于：/etc/mysql/my.cnf，如果没有则手动创建。

    在/etc/mysql/my.cnf文件的[mysqld]配置项下加入如下配置，以支持小写表名：
    
        文件/etc/mysql/my.cnf：
        [mysqld]
        lower_case_table_names = 1

6. 设置启动时自动运行：

        # cp /usr/local/mysql/support-files/mysql.server /etc/init.d
        # cd /etc/init.d
        # update-rc.d mysql.server defaults 99
        # init 6

7. 修改root用户口令：

        # cd /usr/local/mysql/bin
        # ./mysqladmin -u root password root

8. 修改配置使可以远程访问：

        # mysql -uroot -proot
        MariaDB [(none)]> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%'IDENTIFIED BY 'root' WITH GRANT OPTION;


9. 在宿主机中使用HeiDiSQL等客户端工具进行测试。


