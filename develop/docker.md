## 安装配置Docker

### Linux系统

> Docker社区版安装，参考：https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-ce

1. 卸载旧版本（如果非初次安装）

        # apt-get remove docker docker-engine docker.io

2. 配置仓库

	1. 更新apt-get包索引

            # apt-get update

	2. Install packages to allow apt to use a repository over HTTPS:

            # apt-get install  apt-transport-https  ca-certificates  curl  software-properties-common

	3. Add Docker’s official GPG key:

            # curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

    4. Verify that you now have the key with the fingerprint 9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88, by searching for the last 8 characters of the fingerprint.

            # apt-key fingerprint 0EBFCD88
            pub   4096R/0EBFCD88 2017-02-22
                  Key fingerprint = 9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
            uid                  Docker Release (CE deb) <docker@docker.com>
            sub   4096R/F273FCD8 2017-02-22

	5. Use the following command to set up the stable repository. You always need the stable repository, even if you want to install builds from the edge or test repositories as well. To add the edge or test repository, add the word edge or test (or both) after the word stable in the commands below.

            # sudo add-apt-repository  "deb [arch=amd64] https://download.docker.com/linux/ubuntu  $(lsb_release -cs)  stable"

3. 安装DOCKER CE

	1. Update the apt package index.

            # apt-get update

	2. Install the latest version of Docker CE

            # apt-get install docker-ce

	3. 安装docker-compose

            # apt-get install docker-compose

	4. Verify that Docker CE is installed correctly by running the hello-world image.

            # docker run hello-world

4. 搭建docker代理
      
    由于网络情况，可能存在docker镜像下载慢的问题。可通过搭建和配置Sock5代理解决。
    
    

