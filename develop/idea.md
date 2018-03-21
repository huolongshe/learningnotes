## 安装配置Idea IntelliJ集成开发环境

### Linux系统

1. 下载

    下载Idea IntelliJ企业版：
    https://download.jetbrains.8686c.com/idea/ideaIU-2017.3.4.tar.gz
        
2. 安装
        
    将下载文件拷贝至/opt目录，然后解压：

        # tar -zxf ideaIU-2017.3.4.tar.gz
        
3. 创建目录符号链接，以方便访问：

        # ln -s idea-IU-173.4548.28 idea

4. 破解

    1. 将破解文件JetbrainsCrack-2.7-release-str.jar拷贝至/opt/idea/bin目录
    
    2. 修改/opt/idea/bin下的idea.vmoptions和idea64.vmoptions两个文件，分别在末尾加入如下一行：
    
            -javaagent:/opt/idea-IU-173.4548.28/bin/JetbrainsCrack-2.7-release-str.jar
         
5. 运行

        # /opt/idea/bin/idea.sh

    选择Activation Code，随便输入一些内容，点“OK”，然后一路Next。
