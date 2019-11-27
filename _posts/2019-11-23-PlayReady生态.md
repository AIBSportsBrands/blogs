---
layout: post
title:  "PlayReady生态"
categories: [视频, PlayReady]
tags: [PlayReady, DRM, 内容保护]
---

PlayReady系统主要由客户端和服务器两部分组成。这两部分通过微软的私有协议进行通信。一个视频打包服务会调用PlayReady来加密保护视频内容。客户端按照证书对加密的视频进行解密。这就是PlayReady基本的工作流程。

## PlayReady客户端

PlayReady客户端负责按照给定的证书来解密播放特定的被加密保护的视频内容，它可以是电脑上的媒体播放器，也可以是手机、平板电脑、电视上的应用。PlayReady客户端还必须保证证书中包含的权限（rights）和限制（restrictions）条款被严格执行。

![](/assets/posts/pr-eco/pr-devices.jpg)

## PlayReady服务器

可定制化的应用服务器负责和客户端交互。服务提供商可以使用PlayReady Server SDK来构建自己的业务逻辑。比如，订阅服务可以定制出自己的证书，证书中可以包含订阅的过期时间、证书发布者，客户端可以根据证书来管理订阅有效期和验证发布者。

PlayReady服务器包括证书服务、域名控制器、计量服务、Secure Stop服务和Secure Delete服务。这些服务都可以基于PlayReady Server SDK来开发。

除此以外，一个完整的内容后端还需要视频内容打包服务来负责加密编码视频，还需要流服务器和CDN来分发内容。

![](/assets/posts/pr-eco/pr-servers.jpg)

###### Note
PlayReady不需要一个特殊的Web服务器来分发内容。

## 内容保护流程

PlayReady系统中，视频内容打包服务负责加密视频并发布到Web服务器上，客户端通过串流或者下载方式获得加密的视频，客户端还要联系证书服务来获得相关视频的证书。

下图展示了协议获取（license acquisition，LA）的基本流程。灰色线表示清流，黑色线表示加密流，白色线表示证书。

![](/assets/posts/pr-eco/pr-flow.jpg)

整个流程包含下面几步：

1. 未加密视频通过打包服务进行加密编码。
2. 打包服务将加密的视频发布到Web服务器上。
3. 打包服务将相关的视频保护信息发送到证书服务上。
4. 客户端从Web服务器或者分发网络取得加密视频。
5. 当客户端尝试播放视频时，从视频文件头得知需要获取相关的证书。这是客户端需要发起一个协议获取（LA）到证书服务来申请这个证书。


