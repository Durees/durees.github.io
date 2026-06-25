---
title: AX6000刷OpenWrt配置lan和wan
pubDate: 2026-02-13
description: 路由器救砖实录：SSH 通过 IPv6 连接重置、LAN/WAN 网段冲突排查与修复
tags: [OpenWrt, 路由器, 网络]
---

# AX6000刷OpenWrt后设置错误

上次接入当地网络，后台设置错误，路由器直接变成"砖"，后台进不去了。

# 修好了

通过lanscan扫描wifi和端口，发现ipv4没有，route没有，只有个ipv6和mac地址。（wifi至少要能连接上）

然后，通过ssh连接ipv6接入后台重置+重启，感谢gemini和chatgpt排除问题。*之前直接按reset没有效果，怀疑是没有拔掉lan网线*

最后，原有的后台ip地址使用默认用户名和密码：root password 进入后台

# 配置lan和wan

现在要配置ronte。问题是上游的路由器是192.168.1.1，openwrt默认使用192.168.1.4作为自己的IP地址。
**两者都在192.168.1.x网段中，相互冲突。这也就是之前为什么不能正确配置网络的原因。**
