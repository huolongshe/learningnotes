## 安装配置Tomcat

### Linux系统

1. 下载安装文件：

	http://mirror.bit.edu.cn/apache/tomcat/tomcat-9/v9.0.5/bin/apache-tomcat-9.0.5.tar.gz

2. 安装

    拷贝至/opt目录，然后解压：

        #tar -zxf apache-tomcat-9.0.5.tar.gz
        #ln -s apache-tomcat-9.0.5 tomcat

3. 配置环境变量
    在/etc/profile文件末尾追加一行：

        文件/etc/profile：
        export CATALINA_HOME=/opt/apache-tomcat-9.0.5
            
    然后重启或运行 source /etc/profile 使其生效：

        # source /etc/profile


4. 测试

    1. 缺省在8080端口启动Tomcat服务器
    
            # cd /opt/tomcat/bin
            # ./startup.sh

    2. 在浏览器中输入http://127.0.0.1:8080进行验证。
