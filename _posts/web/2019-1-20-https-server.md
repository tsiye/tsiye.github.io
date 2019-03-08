---
layout: post
title: 网络-Https服务器部署-Certbot
category: 应用向
tags: [web,https]
---

1. HTTPS的信任继承基于预先安装在浏览器中的证书颁发机构（如VeriSign、Microsoft等）（意即“我信任证书颁发机构告诉我应该信任的”）。因此，一个到某网站的HTTPS连接可被信任，当且仅当：
2. 用户相信他们的浏览器正确实现了HTTPS且安装了正确的证书颁发机构；
3. 用户相信证书颁发机构仅信任合法的网站；
4. 被访问的网站提供了一个有效的证书，意即，它是由一个被信任的证书颁发机构签发的（大部分浏览器会对无效的证书发出警告）；
5. 该证书正确地验证了被访问的网站（如，访问https://example时收到了给“Example Inc.”而不是其它组织的证书）；
6. 或者互联网上相关的节点是值得信任的，或者用户相信本协议的加密层（TLS或SSL）不能被窃听者破坏。
7. HTTPS不应与在 RFC 2660 中定义的安全超文本传输协议（S-HTTP）相混淆。

## 我们需要一张CA证书
你只需要有一张被信任的**CA （ Certificate Authority ）也就是证书授权中心颁发的 SSL 安全证书**，并且将它部署到你的网站服务器上。一旦部署成功后，当用户访问你的网站时，浏览器会在显示的网址前加一把小绿锁，表明这个网站是安全的，当然同时你也会看到网址前的前缀变成了 https ，不再是 http 了。

理论上，我们自己也可以签发 SSL 安全证书，但是我们自己签发的安全证书不会被主流的浏览器信任，所以我们需要被信任的证书授权中心（ CA ）签发的安全证书。而一般的 SSL 安全证书签发服务都比较贵，比如 Godaddy 、 GlobalSign 等机构签发的证书一般都需要20美金一年甚至更贵，不过为了加快推广 https 的普及， EEF 电子前哨基金会、 Mozilla 基金会和美国密歇根大学成立了一个公益组织叫 ISRG （ Internet Security Research Group ），这个组织从 2015 年开始推出了 Let’s Encrypt 免费证书。这个免费证书不仅免费，而且还相当好用，所以我们就可以利用 Let’s Encrypt 提供的免费证书部署 https 了.

## Let’s Encrypt 及 Certbot 简介
Let’s Encrypt(又叫Certbot) 是 一个叫 ISRG （ Internet Security Research Group ，互联网安全研究小组）的组织推出的免费安全证书计划。参与这个计划的组织和公司可以说是互联网顶顶重要的先驱，除了前文提到的三个牛气哄哄的发起单位外，后来又有思科（全球网络设备制造商执牛耳者）、 Akamai 加入，甚至连 Linux 基金会也加入了合作，这些大牌组织的加入保证了这个项目的可信度和可持续性。

## 安装过程
[ubuntu nginx 安装 certbot(letsencrypt)](http://www.mamicode.com/info-detail-1782380.html)

