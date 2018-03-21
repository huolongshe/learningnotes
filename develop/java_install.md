
## 安装配置Java开发环境

### Linux系统

1. 安装

        # apt install openjdk-8-jdk

2. 配置环境变量： 

        在/etc/profile文件末尾追加：
        export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64
        export PATH=$PATH:$JAVA_HOME/bin:$JAVA_HOME/jre/bin
        
3. 然后reboot，或者执行如下命令使新配置的环境变量生效：

        # source /etc/profile
