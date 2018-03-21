## 安装配置Sock5代理

### 安装Shadowsocks

1. 安装

        # pip install shadowsocks

2. 运行
	
        # sslocal -s <SERVER_ADDR> -p <SERVER_PORT> -k <PASSWORD> -m <ENCRYPTION> &
    
     可在/etc/profile文件中追加一行，以使系统reboot时启动Sock5服务：
      
        文件/etc/profile：
        sslocal -s <SERVER_ADDR> -p <SERVER_PORT> -k <PASSWORD> -m <ENCRYPTION> &

---

### 配置Maven使用Sock5代理
	
- 运行maven时带命令行参数，例如：

        # mvn install  -DsocksProxyHost=127.0.0.1 -DsocksProxyPort=1080

- 或者，通过将参数配置为环境变量
  
  在/etc/profile文件中追加一行，然后reboot：

        文件/etc/profile文件：
        export MAVEN_OPTS="-DsocksProxyHost=127.0.0.1 -DsocksProxyPort=1080"

---

### 配置git使用socks5代理

- 编辑文件：/usr/local/bin/gitproxy，内容如下：

        文件/usr/local/bin/gitproxy：
        #!/bin/bash
        case $1 in
        on)
        git config --global http.proxy 'socks5://127.0.0.1:1080'
        git config --global https.proxy 'socks5://127.0.0.1:1080'
        ;;
        off)
        git config --global --unset http.proxy
        git config --global --unset https.proxy
        ;;
        status)
        git config --get http.proxy
        git config --get https.proxy
        ;;
        esac
        exit 0
        
- 增加可执行权限：

        # chmod +x /usr/local/bin/gitproxy

- 使用说明

    - 打开socks5代理：

            # gitproxy on

    - 关闭socks5代理

            # gitproxy off

    - 查看socks5状态

            # gitproxy status

---

### 配置docker使用socks5代理

1. 在/etc/systemd/system目录下，新建docker.service.d子目录：

        root@ubuntu1:/etc/systemd/system# mkdir docker.service.d

2. 在/etc/systemd/system/docker.service.d目录下新建http-proxy.conf文件，并在其中写入：

        文件/etc/systemd/system/docker.service.d/http-proxy.conf
        [Service]
        Environment="HTTP_PROXY=socks5://127.0.0.1:1080/" "HTTPS_PROXY=socks5://127.0.0.1:1080/" "NO_PROXY=localhost,127.0.0.1"

