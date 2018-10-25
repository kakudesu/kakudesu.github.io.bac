---
layout: post
title: 使用OpenShift的免费服务器搭建V2Ray
author: suki
date: 2018-10-25
tags:
- openshift
- v2ray
english: false
---

## 使用OpenShift的免费服务器搭建V2Ray

### 前言


先感谢一下作者的 GitLab: [GitLab-V2ray](https://gitlab.com/cbwang/v2ray-openshift)

在 OpenShift 上搭建 V2raycore 之前，我们先要注册 OpenShift 账户。

点击 [OpenShift](manage.openshift.com) 进入官方站点，如果有账号请直接点击
`LOG IN WITH RED HAT`，如果没有账号请点击下方的`Sign up` ，然后使用邮箱注册激活账户

### 教程

#### 服务端

- 1.登陆 OpenShift ,添加一个新的免费Plan

> 一般来说，当时申请了会提示你多久之后部署成功，但不排除现在空间闲置，分分钟搞定！

![v2ray](/assets/img/posts/2018/v2ray/1.png "教程图解")

- 2.点击 `manage.openshift.com` 之后，你能看到上图一样，然后点击`Open Web Console`进入，点击右上角的 `Create Project`，名字随意，然后点击`Creat`,等待刷新出现刚刚创建的项目

![v2ray](/assets/img/posts/2018/v2ray/2.png "教程图解")

![v2ray](/assets/img/posts/2018/v2ray/3.png "教程图解")

- 3.点击刚刚创建的项目，点击`Deploy Image`

![v2ray](/assets/img/posts/2018/v2ray/4.png "教程图解")

点击 `Image Name`，输入 `alwalw/cundang`，然后点击搜索。

> 此处输入的docker镜像名为 `hub.docker.com` 创建好的镜像名称,如果想自行创建,可以注册`hub.docker.com`账号并绑定github账号,先到`https://github.com/wangyi2005/v2ray`里fork一份docker镜像代码,然后回到`hub.docker.com`,右上角头像左侧,点击`create => create automated build`,选择github刚刚fork的docker镜像仓库,创建完成后会自动跳转进刚刚创建的镜像,点击`build setting`, 点击`master` `branch` 后面的`Trigger`然后`save change`即可设置为自动部署

![v2ray](/assets/img/posts/2018/v2ray/5.png "教程图解")

- 4.搜索完成之后往下拉一点，找到下图位置，在 `Name` 那里填写 `CONFIG_JSON` ,在 `Value` 位置填写`V2ray`的配置信息，如下：

```

{
  "log": {
    "loglevel": "warning"
  },
  "inbound": {
    "protocol": "vmess",
    "port": 8080,
    "settings": {
      "clients": [
        {
          "id": "你自己的UUID",
          "alterId": 64,
          "security": "aes-128-gcm"
        }
      ]
    },
    "streamSettings": {
      "network": "ws"
    }
  },
  "inboundDetour": [],
  "outbound": {
    "protocol": "freedom",
   "settings": {}
  }
}

```

![v2ray](/assets/img/posts/2018/v2ray/6.png "教程图解")

> Ps：UUID的生成可以使用这里生产： www.uuidgenerator.net 生成后复制到上面配置中，填写到Value框里面！并且本地备份一下，后面使用服务要用到！

- 5.保存之后，点击如下操作：

`overview => deploy => edit`

![v2ray](/assets/img/posts/2018/v2ray/7.png "教程图解")

- 6.点击 `edit` 进入之后，如下图将 `25%` 都改为`1`。然后点击`Save`保存。

![v2ray](/assets/img/posts/2018/v2ray/8.png "教程图解")

- 7.上一步的操作 `保存` 之后，回到 `OverView`，如下图，点击箭头处的`pods`将值调整为 `2`

![v2ray](/assets/img/posts/2018/v2ray/9.png "教程图解")

然后点击上图箭头所指 `Create Route`，将里面的`Security`下的 唯一 一个选项打勾，然后点击`Create`。

至此,服务端的创建已经完成,现在, `Routes`处已经能够看到服务器地址了

#### 客户端

客户端下载地址: [V2Ray](https://www.v2ray.com/ui_client/)

##### 客户端配置

- Windows客户端配置如下:

![v2ray](/assets/img/posts/2018/v2ray/10.png "教程图解")

> 其中地址，就是第7步最后的原`Create Route`位置的`Routes – External Traffic`下的网址，去除`https://` ,而`用户ID`就是刚刚生成的`UUID`。

> 保存之后，右键托盘，点击启用`http代理`，然后选择`http代理模式`为`PAC模式`，稍等片刻即可。如果出错，请自行检测配置及步骤！

- 安卓客户端配置如下:

安卓端的APP挺多,我用的app是 `BifrostV` ,可根据需要自行找到上方地址下载

![v2ray](/assets/img/posts/2018/v2ray/11.jpg "教程图解")

### Done!

