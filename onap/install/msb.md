## 以docker方式部署MSB项目

> 参考： https://onap.readthedocs.io/en/latest/submodules/msb/apigateway.git/docs/platform/installation.html

1. Run the Consul dockers

        # docker run -d --net=host --name msb_consul consul:0.9.3

2. Run the MSB dockers

    Login the ONAP docker registry first:

        # docker login -u docker -p docker nexus3.onap.org:10001
        WARNING! Using --password via the CLI is insecure. Use --password-stdin.
        2018-03-09 20:12:12 INFO     connecting nexus3.onap.org:10001 from 127.0.0.1:39022
        2018-03-09 20:12:15 INFO     connecting nexus3.onap.org:10001 from 127.0.0.1:39026
        Login Succeeded
        root@ubuntu1:~#

    Run MSB dockers：

        # docker run -d --net=host --name msb_discovery nexus3.onap.org:10001/onap/msb/msb_discovery
            
        # docker run -d --net=host -e "ROUTE_LABELS=visualRange:1" --name msb_internal_apigateway nexus3.onap.org:10001/onap/msb/msb_apigateway

3. Browse the registered services

    Open MSB Web GUI portal in your browser: 
    
            http://127.0.0.1/msb
    
    you can see all the registered services. 
    If the registered service support swagger, you can see the REST API documentation and test the registered services via the swagger UI integrated in MSB.

