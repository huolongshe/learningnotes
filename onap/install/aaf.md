## 编译部署AAF项目

> 参考： https://wiki.onap.org/display/DW/AAF+Installation+Guide


1. 编译：

        root@ubuntu1:~/onap/aaf/authz# mvn clean install -DskipTests

2. Build Docker Image

        root@ubuntu1:~/onap/aaf/authz/authz-service#
        root@ubuntu1:~/onap/aaf/authz/authz-service# mvn clean install docker:build

3. Path to docker-compose folder

        # cd src/main/resources/docker-compose
        # chmod +x *.sh
        # docker-compose up -d  （如果docker-compose未安装，执行apt-get install docker-compose安装）

4. To check running containers 

        #docker ps

    应该看到attos/aaf和cassandra:2.1.16 两个docker镜像已经拉起。

5. To check container logs

        # docker logs 9926416b9ebc  （用CONTAINER_ID）
        # docker logs dockercompose_aaf_container_1  （用NAME）

6. To access files inside the container

        # docker exec -it dockercompose_aaf_container_1 bash

7. access the cassandra command line from bash

        # docker exec -it dockercompose_cassandra_container_1 bash
        root@3cd75649f828:/#
        root@3cd75649f828:/# cqlsh -u root -p root
        Connected to Test Cluster at 127.0.0.1:9042.
        [cqlsh 5.0.1 | Cassandra 2.1.16 | CQL spec 3.2.1 | Native protocol v3]
        Use HELP for help.
        root@cqlsh> use authz;
        root@cqlsh:authz>describe tables;
        run_lock  role   user_role  perm  future  delegate   approval  cred   
        x509      cache  artifact   cert  notify  ns_attrib  ns        history
        root@cqlsh:authz>select * from ns;
        root@cqlsh:authz>
        root@cqlsh:authz> quit
        root@3cd75649f828:/# exit
        exit
        #

8. AAF Command line to  create & grant permissions

        # docker exec -it dockercompose_aaf_container_1 bash
        root@9926416b9ebc:/# cd opt/app/aaf/authz-service/2.0.15
        root@9926416b9ebc:/opt/app/aaf/authz-service/2.0.15# wget https://raw.githubusercontent.com/att/AAF/master/authz/authz-service/src/main/resources/docker-compose/runaafcli.sh
        root@9926416b9ebc:/opt/app/aaf/authz-service/2.0.15# sh runaafcli.sh
        aaf_id: dgl@openecomp.org
        aaf_password: ecomp_admin
        aafcli >
        aafcli > ns list name org.openecomp
        ns list name org.openecomp
        List Namespaces by Name[org.openecomp]
        --------------------------------------------------------------------------------
        org.openecomp
            Administrators
        dgl@openecomp.org
            Roles
                org.openecomp.admin                                                     
                org.openecomp.owner                                                     
            Permissions
                org.openecomp.access           *                        *              
                org.openecomp.access           *                        read           
            Credentials
        dgl@openecomp.org              U/P    2020/12/31 00:00 UTC    
        aafcli >
        aafcli > perm list user dgl@openecomp.org
        perm list user dgl@openecomp.org
        List Permissions by User[dgl@openecomp.org]
        --------------------------------------------------------------------------------
        PERM Type                      Instance                       Action    
        --------------------------------------------------------------------------------
        com.access                     *                              *         
        com.access                     *                              read      
        com.att.aaf.access             *                              *         
        com.att.aaf.access             *                              read      
        com.att.access                 *                              *         
        com.att.access                 *                              read      
        org.access                     *                              *         
        org.access                     *                              read      
        org.openecomp.access           *                              *         
        org.openecomp.dmaapBC.access   *                              *         
        org.openecomp.dmaapBC.access   *                              read      
        org.openecomp.dmaapBC.mr.topic :topic.org.openecomp.dmaapBC.newtopic pub       
        org.openecomp.dmaapBC.mr.topic :topic.org.openecomp.dmaapBC.newtopic sub       
        org.openecomp.dmaapBC.topicFactory :org.openecomp.dmaapBC.topic:org.openecomp.dmaapBC create    
        aafcli >
        aafcli > perm create org.openecomp.dmaapBC.mr.topic :topic.org.openecomp.dmaapBC.mytopic1 pub org.openecomp.dmaapBC.admin org.openecomp.dmaapBC.access
        perm create org.openecomp.dmaapBC.mr.topic :topic.org.openecomp.dmaapBC.mytopic1 pub org.openecomp.dmaapBC.admin org.openecomp.dmaapBC.access
        Created Permission
        Granted Permission [org.openecomp.dmaapBC.mr.topic|:topic.org.openecomp.dmaapBC.mytopic1|pub] to Role [org.openecomp.dmaapBC.admin]
        aafcli >
        aafcli > perm create org.openecomp.dmaapBC.mr.topic :topic.org.openecomp.dmaapBC.mytopic1 sub org.openecomp.dmaapBC.admin org.openecomp.dmaapBC.access
        perm create org.openecomp.dmaapBC.mr.topic :topic.org.openecomp.dmaapBC.mytopic1 sub org.openecomp.dmaapBC.admin org.openecomp.dmaapBC.access
        Created Permission
        Granted Permission [org.openecomp.dmaapBC.mr.topic|:topic.org.openecomp.dmaapBC.mytopic1|sub] to Role [org.openecomp.dmaapBC.admin]
        aafcli >
        aafcli > help
        --help
        AAF Command Line Tool
        ---------------------
          --help [-d (more details)] [command]
          ------------------------------------------------------------------------------
          --version
          ------------------------------------------------------------------------------
          perm create <type> <instance> <action> [role[,role]* (to Grant to)]
               delete <type> <instance> <action>
               <grant|ungrant|setTo> <type> <instance> <action> [role[,role]* (!REQ S)]
               rename <type> <instance> <action> <new type> <new instance> <new action>
               describe <type> <instance> <action> <description>
               list user <id>
                    name <root perm name>
                    ns <name>
                    role <name>
                    activity <type>
          ------------------------------------------------------------------------------
          role <create|delete> <name>
               user <add|del|setTo|extend> <role> [id[,id]* (not required for setTo)]
               describe <name> <description>
               list user <id>
                    role <role>
                    ns <name>
                    name <name>
                    perm <type> <instance> <action>
                    activity <name>
          ------------------------------------------------------------------------------
          user role <add|del|setTo|extend> <user> [role[,role]* (!REQ S)]
               cred <add|del|reset|extend> <id> [password (! D|E)] [entry# (if multi)]
               delegate <add|upd|del> <from> [to REQ A&U] [until (YYYY-MM-DD) REQ A]
               list role <role>
                    perm <type> <instance> <action>
                    cred <ns|id> <value>
                    delegates <user|delegate> <id>
                    approvals <user|approver|ticket> <value>
                    activity <user>
          ------------------------------------------------------------------------------
          ns create <name> <responsible (id[,id]*)> [admin (id[,id]*)]
             delete <name>
             admin <add|del> <name> <id[,id]*>
             responsible <add|del> <name> <id[,id]*>
             describe <name> <description>
             attrib <add|upd|del> <ns> <key> [value]
             list name <ns>
                  <admin|responsible> <user>
                  activity <name>
                  user perm <ns>
                       role <ns>
                  children <ns>
                  keys <attrib>
          ------------------------------------------------------------------------------
          mgmt cache clear <name[,name]*>
               deny ip <add|del> <ipv4or6[,ipv4or6]*>
                    id <add|del> <identity[,identity]*>
               log <add|del> <id[,id]*>
               dbsession clear <machine>
        aafcli >

