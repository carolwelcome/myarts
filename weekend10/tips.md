---
title: tomcat多host配置
date: 2019-05-16 18:38:22
categories:  #分类
    - attendance
tags:   #标签
    - ARTS
    - tomcat 
description: 
    tomcat多host配置
---


### 配置内容
挺有意思，记录一下。学习tomcat源码的实践
```xml
<?xml version="1.0" encoding="UTF-8"?>

<Server port="8105" shutdown="SHUTDOWN">
  <Listener className="org.apache.catalina.startup.VersionLoggerListener" />
  <!-- Security listener. Documentation at /docs/config/listeners.html
  <Listener className="org.apache.catalina.security.SecurityListener" />
  -->
  <!--APR library loader. Documentation at /docs/apr.html -->
  <Listener className="org.apache.catalina.core.AprLifecycleListener" SSLEngine="on" />
  <!-- Prevent memory leaks due to use of particular java/javax APIs-->
  <Listener className="org.apache.catalina.core.JreMemoryLeakPreventionListener" />
  <Listener className="org.apache.catalina.mbeans.GlobalResourcesLifecycleListener" />
  <Listener className="org.apache.catalina.core.ThreadLocalLeakPreventionListener" />

  <!-- Global JNDI resources
       Documentation at /docs/jndi-resources-howto.html
  -->
  <GlobalNamingResources>
    <!-- Editable user database that can also be used by
         UserDatabaseRealm to authenticate users
    -->
    <Resource name="UserDatabase" auth="Container"
              type="org.apache.catalina.UserDatabase"
              description="User database that can be updated and saved"
              factory="org.apache.catalina.users.MemoryUserDatabaseFactory"
              pathname="conf/tomcat-users.xml" />
  </GlobalNamingResources>

 
  <Service name="Catalina">

    
    <Connector port="80" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
    

    <!-- Define an AJP 1.3 Connector on port 8009 -->
    <Connector port="8109" protocol="AJP/1.3" redirectPort="8443" />


    
    <Engine name="Catalina" defaultHost="localhost">

      

      <!-- Use the LockOutRealm to prevent attempts to guess user passwords
           via a brute-force attack -->
      <Realm className="org.apache.catalina.realm.LockOutRealm">
        <!-- This Realm uses the UserDatabase configured in the global JNDI
             resources under the key "UserDatabase".  Any edits
             that are performed against this UserDatabase are immediately
             available for use by the Realm.  -->
        <Realm className="org.apache.catalina.realm.UserDatabaseRealm"
               resourceName="UserDatabase"/>
      </Realm>

      <Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true">
      </Host>
	  <Host name="www.domain1.com"  appBase="webapps1"
            unpackWARs="true" autoDeploy="true">
      </Host>
	  <Host name="www.domain2.com"  appBase="webapps2"
            unpackWARs="true" autoDeploy="true">
      </Host>
	  <Host name="www.domain3.com"  appBase="webapps3"
            unpackWARs="true" autoDeploy="true">
      </Host>
    </Engine>
  </Service>
</Server>

```