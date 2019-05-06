---
title: SQL Server数据库sa账户解锁
date: 2019-05-05 23:28:22
categories:  #分类
    - attendance
tags:   #标签
    - ARTS
    - SQL Server 
    - Windows身份禁用
    - sa解锁
description: 
    sqlserver windows账户禁用，sa账户锁定，如何解决？https://blog.csdn.net/yenange/article/details/79637599

### 背景
有台服务器放在某zw云环境上，是做SqlServer数据库用的，当初为了开发方便所以所有用户都直接用的最大权限的用户sa,后面着急试运行就直接给上了，时间一久反正也没人攻击就干脆一直这样了。最近组织安保测试，把所有的云主机，数据库、应用服务加密策略都改了一遍。数据库禁用了windows账户登录，还设置了所有账户的密码强制过期的策略。上回到期的时候没注意，直接了改了一下也没发生啥问题。这不又到期了，老套路一改，发现坏了菜了，密码改是改了，只是sa账户锁定了，所有的应用都连不上了。这家伙还怎么了的。

### 解决过程
1、一般情况下如果Windows没有禁用的话，解决这个问题那简直的，四年级小孩子都能搞定。直接用windows身份登录进去找到对应账户把锁定的框框一勾就完事了。
2、面对我这种情况你咋整？重装数据库？这也太不甘心了。想想这sqlserver也太弱了点吧，没关系，这块弱整个操作系统都是我的，还怕这？重点来了：
* 创建一个新的Windows 用户帐户, 名称为 dba， 类型为管理员;
* 以 dba 登录Windows,并停止SQL Server服务;
* "以管理员身份运行"net start 服务名 /m  , 进入单用户模式;(`重点`)
* sqlcmd -S  .\实例名 -A, 以DAC（专用管理员连接）方式进入sqlcmd(`重点`)；
* 执行SQL， 创建一个新的 sysadmin 权限的SQL账户， 脚本如下：
```Sql
USE [master]
GO
CREATE LOGIN [admin] WITH PASSWORD=N'admin', DEFAULT_DATABASE=[master], CHECK_EXPIRATION=OFF, CHECK_POLICY=OFF
GO
ALTER SERVER ROLE [sysadmin] ADD MEMBER [admin]
GO
```
* 正常重启SQL Server服务（非单用户模式）；
* 到此为止，你可以用SQL Server的新账户admin登录进入了。进去之后，将其它账户改为启用即可。

### 警告
呀，后来回想一下这事儿还真的挺惭愧的，但是既然发生了还是有些总结，好吧。希望也给大家能少留点坑。
* sa用户是超级用户，不能用于连接账号，测试账号也不行；
* 好的开发习惯、运维习惯都会让那些傻逼的问题远离你，不缠着你；
* 欢迎大家留言吧
