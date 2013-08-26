---
layout: post
title: "g++: Internal error: Killed (program cc1plus)"
date: 2013-08-26 16:22
comments: true
categories: nginx
---

买了阿里云服务，系统为centos 6.3，内存512M.通过passenger安装nginx:　rvmsudo Passenger-install-nginx-module 在编译nginx时总是报错：`g++: Internal error: Killed (program cc1plus)`google很多终于找到问题：<http://www.linlook.com/index/article/articleid/1350/>
以下引自原处：

```
内存不足， 在linux下增加临时swap空间
step 1:
　　#dd if=/dev/zero of=/home/swap bs=1024 count=500000
　　注释：of=/home/swap,放置swap的空间; count的大小就是增加的swap空间的大小，1024就是块大小，这里是1K，所以总共空间就是bs*count=500M
step 2:
　　# mkswap /home/swap
　　注释：把刚才空间格式化成swap各式
step 3:
　　#swapon /home/swap
　　注释：使刚才创建的swap空间

      如果想关闭刚开辟的swap空间，只需命令：#swapoff

 
如上操作以后，你的安装过程就少了这道阻拦拉拉拉！！！ 当然对于公司工作估计很难遇到此种情况啦啦，你们配置都很牛逼滴！
```
