## 编译调试Portal SDK项目

> 参考： https://onap.readthedocs.io/en/latest/submodules/portal.git/docs/tutorials/portal-sdk/setting-up.html

1. MariaDB数据库配置

        # mysql -uroot -proot
        MariaDB [(none)]> drop database if exists ecomp_sdk;
        MariaDB [(none)]> drop user if exists 'ecomp_sdk_user'@'localhost';
        MariaDB [(none)]> create database ecomp_sdk;
        MariaDB [(none)]> create user 'ecomp_sdk_user'@'localhost' identified by 'password';
        MariaDB [(none)]> grant all privileges on ecomp_sdk.* to 'ecomp_sdk_user'@'localhost';

2. 添加数据库表测试数据

        # mysql -proot -uroot < sdk/ecomp-sdk/epsdk-app-common/db-scripts/EcompSdkDDLMySql_1707_Common.sql
        # mysql -proot -uroot < sdk/ecomp-sdk/epsdk-app-common/db-scripts/EcompSdkDMLMySql_1707_Common.sql
        # mysql -proot -uroot < sdk/ecomp-sdk/epsdk-app-os/db-scripts/EcompSdkDDLMySql_1707_OS.sql
        # mysql -proot -uroot < sdk/ecomp-sdk/epsdk-app-os/db-scripts/EcompSdkDMLMySql_1707_OS.sql

3. 配置应用程序连接数据库

    修改文件sdk/ecomp-sdk/epsdk-app-os/src/main/webapp/WEB-INF/conf/system.properties：

        db.driver =  org.mariadb.jdbc.Driver
        db.connectionURL = jdbc:mariadb://localhost:3306/ecomp_sdk
        db.userName = ecomp_sdk_user
        db.password = password

4. Clone源代码

        # git clone http://gerrit.onap.org/r/portal/sdk 
        
    Clone指定分支版本：
    
        # git clone http://gerrit.onap.org/r/portal/sdk -b amsterdam

5. 编译
    
    >在编译ONAP项目之前，先下载“ https://jira.onap.org/secure/attachment/10829/settings.xml ”文件，拷贝至当前用户本地maven仓库/root/.m2/。
    
        # cd sdk/ecomp-sdk
        # mvn install

6. 将生成的war包拷贝至tomcat安装目录，启动Tomcat

        # cp target/epsdk-app-os.war  /opt/tomcat/webapps
        # cd /opt/tomcat/bin
        # ./startup.sh

7. 测试，在浏览器中打开：

        http://localhost:8080/epsdk-app-os/login.htm



---

## 在Idea中编译Portal SDK项目

1. 打开Idea集成开发环境

        # cd /opt/idea/bin
        # ./iead.sh

2. 选择“Import Project”
	
3. 选择导入： .../sdk/ecomp-sdk/pom.xml文件

4. 一路next，选择Open Project Structure after import

5. 添加SDK：

6. 等待后台加载、更新依赖包，结束后界面左侧显示项目目录结构

7. 添加构建目标。点击右上侧“Edit Configurations...”：

    1. 点击左上角“+”号，选择“Tomcat Server”

    2. 配置“Application Server”，选择“/opt/apache-tomcat-9.0.5

    3. 在“Before Launch: Build, ...”下方，点击“+”号

    4. 选择“Build Artifacts”

    5. 勾选“epsdk-app-os:war”

    6. 在“Deployment”中，添加server启动后的部署项

8. 单击构建项后面的播放按钮编译运行程序，debug按钮调试程序

