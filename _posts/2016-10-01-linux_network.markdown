---
layout: post
title:  "Linux环境若干网络连通性问题解决办法"
date:   2016-10-01 10:00:00 +0800
categories: jekyll update
---

网络连通性(Connectivity)是在Linux环境下进行开发的重要前提条件。许多高频地工作，如系统包管理、项目依赖库管理、代码仓库操作等，都依赖于网络的连通。一旦网络出现状况，各项工作基本都会受阻(Block)。在日常工作中，网络问题的Trouble Shooting是高优先级的事情。网络连通性的问题和现象五花八门。依靠搜索引擎，通常能够解决大部分问题。但是有些问题，网络能够找到的解决方案有限或不明确的，通常需要经过一系列尝试之后才能找到办法。笔者在工作中，就遇到了一些这样的问题。现在将部分问题和最后经过探索行之有效的解决办法总结并分享出来，仅供读者参考。

# 代理设置
- 查询：echo $http_proxy ;  echo $https_proxy
- 取消：unset http_proxy； unset https_proxy
- 设置：export http_proxy=http://10.11.12.13:8080 ; export https_proxy=https://10.11.12.13:8080


# Windows Host + Linux VM, 网络采用NAT方式，`nslookup`无法找到任何机器的DNS
1. `sudo vi /etc/NetworkManager/NetworkManager.conf`, 将dns=dnsmasq屏蔽
2. `sudo service network-manager restart`

# 当网络代理均设置好之后，sudo apt-get update依然提示网络不通的解决办法：
In such cases, one solution is to add a file in /etc/apt/apt.conf.d with specific proxy configuration for apt (this will be used by apt-get, aptitude, synaptic and Ubuntu software center).
Follow the below steps:
- Create /etc/apt/apt.conf.d/40proxy
- gksudo gedit /etc/apt/apt.conf.d/40proxy
- Put the following contents into it - modify the contents to suit your situation.
  > Acquire::http::Proxy "http://10.11.12.13.14:8080";

  If you have a user-name & password you could encode the same in the proxy url (like so, http://username:password@10.11.12.13:8080) or you can use something like ntlmaps for better control.

# Windows Host+Linux VM, VM访问Internet,采用NAT(网络地址转换)的方式最简单，但是会导致host和VM的连接性和VM之间的连接性不好
此时，host通过SSH访问VM的方式是：
1. 在VBOX中，设置好端口转发规则:

![端口转发](/imgs/forward.png)
2. 在host中，执行命令: `ssh -p 2222 username@127.0.0.1`

# Linux批量插入DNS的命令：
1. 用sudo vim打开/etc/hosts
2. 执行 :for i in range(1,1500) | put ='127.0.0.1   MRBTS-'.i | endfor

