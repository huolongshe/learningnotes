## 安装Google输入法

### Googles输入法

1. 安装

        # apt-get install fcitx-googlepinyin

2. 搜索并运行fcitx配置（fcitx configuration），点击左下角“+”号，找到“Google Pinyin”并添加。

3. 之后就可以用Shilt键或ctl+空格键来切换输入法了。

4. 如果输入中文时不出现输入框，则删除多余的fcitx-module-kimpanel组件：

        # apt remove fcitx-module-kimpanel
        # apt autoremove


### 安装搜狗输入法
>在使用Xmanager远程登录Linux图形界面时，搜狗输入法会出现异常，这种情况不建议安装搜狗输入法。

1. 添加fcitx键盘输入法系统

    1. 先添加以下apt源

            # add-apt-repository ppa:fcitx-team/nightly

    2. 添加源之后需要更新一下系统
    
            # apt-get update
            
    3. 然后安装fcitx（如已安装则跳过）
    
            # apt-get install fcitx 
            
    4. 接着安装fcitx的配置工具（如已安装则跳过）
    
            # apt-get install fcitx-config-gtk 
            
    5. 然后安装fcitx的table-all软件包及其他
    
            # apt-get install fcitx-table-all

    6. 安装im-switch切换工具
    
            # apt-get install im-switch
            
    7. 搜索fcitx应用程序，看fcitx是否安装完成

2. 安装sogou输入法

    1. 选择你操作系统对应的sogou输入法版本下载就可以了，例如sogoupinyin_2.2.0.0102_amd64.deb
    
    2. 用dpkg命令来安装搜狗输入法资源包
    
            # dpkg -i sogoupinyin_2.2.0.0102_amd64.deb
            
    3. 可能会失败，提示未安装fcitx-libs等，安装：
    
            # apt-get install fcitx-libs
            # apt-get -f install
            
    4. 然后再次安装sogoupinyin
    
            # dpkg -i sogoupinyin_2.2.0.0102_amd64.deb

3. 设置语言选项

    1. 到系统设置（System Settings）->语言支持（Language Support），将键盘输入法系统（Keyboard input method system）由设置为fcitx

    2. 这个时候如果看不到效果，则重启系统

    3. 搜索出fcitx配置（fcitx configuration），点击左下角“+”号，找到“Sogou Pinyin”并添加

    4. 之后就可以用Shilt键或ctl+空格键来切换输入法了。
