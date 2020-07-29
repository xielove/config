# Deepin命令行使用shadowsocks代理

## 1. 安装shadowsocks客户端：

`sudo apt-get install showsocks-qt5`

## 2. 登录ss账号

## 3. 安装privoxy

`sudo apt-get install privoxy -y`

注意这里可能存在一个问题：安装时终端的尺寸不能太小，否则不能打开dconf配置界面，privoxy服务不能运行，如果产生错误需要卸载重装。我使用下拉式终端yakuake就出现了错误。

## 4. 编辑privoxy配置文件

```
先备份原配置文件: sudo mv /etc/privoxy/config /etc/privoxy/config.bak
在新建一个配置文件: sudo vim /etc/privoxy/config
```
找到下面的部分内容，去掉注释：

```
# 转发地址
forward-socks5t   /               127.0.0.1:1080 .
# 监听地址
listen-address  localhost:8118
# local network do not use proxy
forward         192.168.*.*/     .
forward            10.*.*.*/     .
forward           127.*.*.*/   
```
其他配置不用修改，最后重新启动
```
启动    systemctl restart privoxy 
查看状态 systemctl status privoxy
```
## 5. 编辑终端配置文件

如果使用的shell是zsh则修改.zshrc，如果是bash则修改.bashrc，总之和终端shell有关

添加以下内容：

```
unset https_proxy
unset http_proxy

alias useproxy="export http_proxy=http://127.0.0.1:8118; export https_proxy=http://127.0.0.1:8118"
alias noproxy="unset https_proxy; unset http_proxy"
```

## 6. 测试：

在终端运行：`curl google.com`，若成功则配置完成，以下是命令返回的结果。

```
<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>301 Moved</TITLE></HEAD><BODY>
<H1>301 Moved</H1>
The document has moved
<A HREF="http://www.google.com/">here</A>.
</BODY></HTML>

```

