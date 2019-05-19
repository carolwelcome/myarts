---
title: shiro设置免密登录
date: 2019-05-12 18:38:22
categories:  #分类
    - attendance
tags:   #标签
    - ARTS
    - tips 
description: 
    shiro
---
### shiro介绍
Apache Shiro是一个强大且易用的Java安全框架,执行身份验证、授权、密码和会话管理。使用Shiro的易于理解的API,您可以快速、轻松地获得任何应用程序,从最小的移动应用程序到最大的网络和企业应用程序。

### shiro主要组件
Subject：即“当前操作用户”。但是，在Shiro中，Subject这一概念并不仅仅指人，也可以是第三方进程、后台帐户（Daemon Account）或其他类似事物。它仅仅意味着“当前跟软件交互的东西”。但考虑到大多数目的和用途，你可以把它认为是Shiro的“用户”概念。Subject代表了当前用户的安全操作，SecurityManager则管理所有用户的安全操作。
SecurityManager：它是Shiro框架的核心，典型的Facade模式，Shiro通过SecurityManager来管理内部组件实例，并通过它来提供安全管理的各种服务。
Realm： Realm充当了Shiro与应用安全数据间的“桥梁”或者“连接器”。也就是说，当对用户执行认证（登录）和授权（访问控制）验证时，Shiro会从应用配置的Realm中查找用户及其权限信息，从这个意义上讲，Realm实质上是一个安全相关的DAO：它封装了数据源的连接细节，并在需要时将相关数据提供给Shiro。当配置Shiro时，你必须至少指定一个Realm，用于认证和（或）授权。配置多个Realm是可以的，但是至少需要一个。Shiro内置了可以连接大量安全数据源（又名目录）的Realm，如LDAP、关系数据库（JDBC）、类似INI的文本配置资源以及属性文件等。如果缺省的Realm不能满足需求，你还可以插入代表自定义数据源的自己的Realm实现。
[Shiro权限管理框架详解](https://www.cnblogs.com/jpfss/p/8352031.html)

### 免密登录
至于为什么会用到免密登录这用场景相信，你用到的时候自然会想到，这里就不多写。开始之前先说一个，现在有很多网站都是提供了多种多样的登录认证方式，有短信验证码的，也有第三方应用登录的，还有传统用户名密码+验证码的。如果后面权限管理框架用的是shiro或者是其他的你又该如何去实现这种需求呢？其他的就先不说了，先说说使用shiro的实现，经常用shiro的人肯定都知道UsernamePasswordToken这个关于token的类，这个类实现了RememberMeAuthenticationToken和HostAuthenticationToken，一般情况下传统用户名密码的验证方式直接通过该类新建一个token即可，这个也提供了各种各样的构造方法，但是大家都知道，这个token也就是个token而矣，他只是负责生成，为什么这么说呢，因为具体对密码或者账户的验证的工作不是由它做的，而是由HashedCredentialsMatcher这个家伙做的，而这个也是可以重写的，说白了也就是怎么认证，认证是否通过还是您老人家说了算。

说了那么多没一句有用的，直接上代码吧
自定义一个这样的token从UsernamePasswordToken
```Java
public class NoPasswordToken extends UsernamePasswordToken {

    private static final long serialVersionUID = -2564928913725078138L;

    private LoginType loginType;


    /**
     * 账号密码登录
     *
     * @param username
     * @param password
     */
    public NoPasswordToken(String username, String password) {
        super(username, password);
        this.loginType = LoginType.PASSWORD;
    }

    /**
     * 免密登录
     */
    public NoPasswordToken(String username) {
        super(username, "");
        this.loginType =  LoginType.NOPASSWD;
    }

    public LoginType getType() {
        return loginType;
    }


    public void setType(LoginType type) {
        this.loginType = type;
    }
}
```

再来一个matcher
```Java
public class RetryLimitCredentialsMatchers extends HashedCredentialsMatcher {

    public RetryLimitCredentialsMatchers() {
        super();
        setHashAlgorithmName(Sha256Hash.ALGORITHM_NAME);
    }


    @Override
    public boolean doCredentialsMatch(AuthenticationToken authcToken, AuthenticationInfo info) {
        NoPasswordToken tk = (NoPasswordToken) authcToken;
        if(tk.getType().equals(LoginType.NOPASSWD)){
            return true;
        }
        boolean matches = super.doCredentialsMatch(authcToken, info);
        return matches;
    }
}
```
改一下配置文件shiro.ini
```bash
sha256Matcher = com.js.bigdata.web.commons.shiro.matcher.RetryLimitCredentialsMatchers
sha256Matcher.storedCredentialsHexEncoded = false
sha256Matcher.hashIterations = 1024
sha256Matcher.hashSalted = true
```

其实就这就可以了，其它要注意的点可能就是那个shiroDbRealm文件了吧，只需要跟你新创建的token类型能匹配上就可以了。