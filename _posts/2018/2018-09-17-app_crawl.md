---
layout: post
title: app crawl (APP 爬虫)
author: suki
date: 2018-09-17
tags:
- Python
- app-crawal
- APP 爬虫
- Charles
- mitmproxy
categories: Tutorial
english: false
---

## app crawl (APP 爬虫)

### Way do we need app-crawal

很多企业数据是通过app展示的，如滴滴打车
app爬取相比web端更加容易
    
### app-crawl 思路

使用charles查看具体的request和response内容
使用mitmproxy对request和response进行处理
使用appium+mitmproxy进行自动化处理

### The tools used by app-crawl

常用的抓包工具有：WireShark, Fiddler, Charles, mitmproxy, AnyProxy等
App自动化使用的是appium
主要介绍Charles, mitmproxy, Appium

### charles 使用

#### 介绍：

是一个HTTP代理服务器,HTTP监视器,反转代理服务器，当浏览器连接Charles的代理访问互联网时，Charles可以监控浏览器发送和接收的所有数据。它
允许一个开发者查看所有连接互联网的HTTP通信，这些包括request, response和HTTP headers （包含cookies与caching信息）。
#### 主要功能

支持SSL代理。可以截取分析SSL的请求。
支持流量控制。可以模拟慢速网络以及等待时间（latency）较长的请求。
支持AJAX调试。可以自动将json或xml数据格式化，方便查看。
支持AMF调试。可以将Flash Remoting 或 Flex Remoting信息格式化，方便查看。
支持重发网络请求，方便后端调试。
支持修改网络请求参数。
支持网络请求的截获并动态修改。
检查HTML，CSS和RSS内容是否符合W3C标准。

#### 原理：

首先charles运行在自己的PC上，charles运行的时候会在PC开启一个代理服务，实际上是一个HTTP/HTTPS的代理
设置手机代理为charles的代理地址，这样手机访问互联网的数据包就会流经charles，charles再转发这些数据包到真实的服务器
服务器返回的数据包再由charles转发回手机，charles就起到了中间人的作用，所有流量包都可以捕捉到，因此所有http请求和响应都可以捕获 到。
同时charles还有权力对请求和响应进行修改

> 关于https的请求：

https是一种基于SSL/TLS的http，所有的http数据都是在SSL/TLS协议封装之上进行传输的。
https协议是在http协议的基础上，添加了ssl/tls握手以及数据加密传输，属于应用层协议
![https](/assets/img/posts/2018/app_crawl/app_crawl.jpg)

#### Charles抓取HTTPS流程:

- 客户端向服务器发起HTTPS请求

- Charles拦截客户端的请求,伪装成客户端向服务器进行请求

- 服务器向“客户端”(实际上是Charles)返回服务器的CA证书

- Charles拦截服务器的响应,获取服务器证书公钥,然后自己制作一张证书,将服务器证书替换后发送给客户端。(这一步,Charles拿到了服务器证书的公钥)

- 客户端接收到“服务器”(实际上是Charles)的证书后,生成一个对称密钥,用Charles的公钥加密,发送给“服务器”(Charles)

- Charles拦截客户端的响应,用自己的私钥解密对称密钥,然后用服务器证书公钥加密,发送给服务器。(这一步,Charles拿到了对称密钥)

- 服务器用自己的私钥解密对称密钥,向“客户端”(Charles)发送响应

- Charles拦截服务器的响应,替换成自己的证书后发送给客户端

至此,连接建立,Charles拿到了 服务器证书的公钥 和 客户端与服务器协商的对称密钥,之后就可以解密或者修改加密的报文了。

#### 安装

`apt-key adv --keyserver pgp.mit.edu --recv-keys 1AD28806`

`sudo sh -c'echo deb https://www.charlesproxy.com/packages/apt/ charles-proxy main> /etc/apt/sources.list.d/charles.list`

`sudo apt-get update`

`apt-get install charles-proxy`

#### 使用
    
    
    
### 安装的坑
> ssl 
    
### python mitmproxy 库的使用

### mitmproxy介绍：

mitmproxy 是用 Python 和 C 开发的一个中间人代理软件（man-in-the-middle proxy），它可以用来拦截、修改、重放和保存 HTTP/HTTPS 请求。

它提供了两个命令行工具：

- mitmproxy 具备交互界面
- mitmdump 不具备交互界面，类似 tcpdump

#### mitmproxy的五种代理模式

- 1、正向代理（regular proxy）启动时默认选择的模式
是一个位于客户端和原始服务器(origin server)之间的服务器，为了从原始服务器取得内容，客户端向mitmproxy代理发送一个请求并指定目标(原始服务器)，然后代理向原始服务器转交请求并将获得的内容返回给客户端。客户端必须要进行一些特别的设置才能使用正向代理。
- 2、反向代理（reverse proxy）启动参数 `-R host`
跟正向代理正好相反，对于客户端而言它就像是原始服务器，并且客户端不需要进行任何特别的设置。客户端向mitmproxy代理服务器发送普通请求，mitmproxy转发请求到指定的服务器，并将获得的内容返回给客户端，就像这些内容 原本就是它自己的一样。
- 3、上行代理（upstream proxy）启动参数 `-U host`
mitmproxy接受代理请求，并将所有请求无条件转发到指定的上游代理服务器。这与反向代理相反，其中mitmproxy将普通HTTP请求转发给上游服务器。
- 4、透明代理（transparent proxy）启动参数 `-T`
当使用透明代理时，流量将被重定向到网络层的代理，而不需要任何客户端配置。这使得透明代理非常适合那些无法更改客户端行为的情况 - 代理无聊的Android应用程序是一个常见的例子。
要设置透明代理，我们需要两个新的组件。第一个是重定向机制，可以将目的地为Internet上的服务器的TCP连接透明地重新路由到侦听代理服务器。这通常采用与代理服务器相同的主机上的防火墙形式。比如Linux下的iptables, 或者OSX中的pf,一旦客户端初始化了连接，它将作出一个普通的HTTP请求(注意，这种请求就是客户端不知道代理存在)请求头中没有scheme(比如http://或者https://), 也没有主机名(比如example.com)我们如何知道上游的主机是哪个呢？路由机制执行了重定向，但保持了原始的目的地址．

* iptable设置

`iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 80 -j REDIRECT --to-port 8080`

`iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 443 -j REDIRECT --to-port 8080`

* 启用透明代理

mitmproxy -T

- 5、socks5 proxy 启动参数 `--socks`

采用socks协议的代理服务器

#### mitmproxy 安装

使用pip安装： `pip install mitmproxy`

#### mitmproxy

#### mitmproxy 过滤：

Name | Academy | score 
- | :-: | -: 
Harry Potter | Gryffindor| 90 
Hermione Granger | Gryffindor | 100 
Draco Malfoy | Slytherin | 90


启动时可以使用-s参数导入外部的脚本进行拦截处理
`mitmproxy -s script.py`


### mitmdump

同mitmproxy
        
    
