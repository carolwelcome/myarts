---
title: openssh升级
date: 2019-06-09 18:38:22
categories:  #分类
    - attendance
tags:   #标签
    - ARTS
    - 升级 
description: 
    openSSH升级
---

### openSSH是做什么的
想用一句话说清楚openSSH是什么？openssh就空气和水一样，大家每天都在用，但却忽略了人家的存在。openSSH是远程系统登录与文件传输的一个经典应用，主是用来加密远程控制和文件传输过程中的数据，加密采用的是密钥对的方式。也就说在传输数据之前，双方要交换密钥，当收到对方的数据时，再利用密钥和相应的程序对数据进行解密。在linux上用过ssh命令的同志感受可能更深一点。我了解的也就这么一小丢丢了，坏了坏了，现在这毛病是落下了，什么技术总想着刨根问底

### openSSH升级步骤
时间：2019年6月份
事件：今年是国家七十年大庆，从国务院到各级政府都将信息安全提升到战略性的地位，这不才6月份，就开始责令公安部进行全国范围内的“护网行动”了，我这算不算泄密？就当不算吧，组织黑客对各大部委网站进行攻击，有问题主管领导一律……不扯了，说白了就是漏洞扫描扫出问题了，openSSH有重大的安全漏洞，需要升级openSSH可用版本（一般指7.8以上），现在环境一般都是5.3可见是有多少年头了
1、检查依赖 gcc/pam-devel/zlib-devel/openssl 可以用rpm -qa |grep xx包名来检测
2、如果没有安装的话，先装依赖吧（联网的环境还好说，就某某部机房堡垒机都快不让用了，这这这哪说理去）
3、下载最新版本，xxx.tar.gz，解压`tar -zxvf xxx.tar.gz`
4、编译源代码 
```
./configure --prefix=/usr --sysconfdir=/etc/ssh --with-pam --with-zlib --with-md5-passwords --with-tcp-wrappers

```
5、安装
```
make && make install
```
6、检查
```
ssh -V
```
7、修改配置
```

PermitRootLogin yes
#如果想远程能够远程登录root用户，需要把这一行的注释给去掉

```
8、重启
```
service sshd reload ##建议不用要restart，不然你可能就再也联不上了，强烈建议用reload,然后再start这样会省很多事

service sshd start
```
如果出现以下异常
/etc/ssh/sshd_config line 81: Unsupported option GSSAPIAuthentication
/etc/ssh/sshd_config line 83: Unsupported option GSSAPICleanupCredentials
只需要在/etc/sshd/sshd_config里面注释掉相关行数就可以啦




