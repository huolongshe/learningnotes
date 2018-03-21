## 安装配置maven

### Linux系统

1. 安装maven

        # apt-get install maven
        
2. 查看maven版本号

        # mvn --version
        Apache Maven 3.3.9
        Maven home: /usr/share/maven
        Java version: 1.8.0_151, vendor: Oracle Corporation
        Java home: /usr/lib/jvm/java-8-openjdk-amd64/jre
        Default locale: en_US, platform encoding: UTF-8
        OS name: "linux", version: "4.4.0-21-generic", arch: "amd64", family: "unix"

3. 配置本地仓库
    
    Maven缺省使用当前用户登录目录下的.m2目录作为本地仓库。
    
    对于root用户，在root登录目录下新建子目录：/root/.m2/。

