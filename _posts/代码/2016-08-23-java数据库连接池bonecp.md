---
layout: post
title: "转载:java数据库连接池bonecp"
date: 2016-08-23
author: hunterhug
desc: "据BoneCP网站官方报告称，BoneCP比性能排名第二的Java数据库连接池性高高25倍以上，并且支持Hibernate和DataNucleus这样的数据持久框架（当然支持JDBC这种直接方式了）。"
categories: [代码]
tags: ["数据库","Java"]
permalink: "/code/java-bonecp.html"
--- 

原创作品，允许转载，转载时请务必以超链接形式标明文章 原始出处 、作者信息和本声明。否则将追究法律责任。http://zhoufoxcn.blog.51cto.com/792419/438277

在2006年8月的时候我在项目中使用过Proxool这个Java数据库连接池，在当时的使用过程中遇到了一些问题，为此曾写过一篇名为《关于Proxool使用的一点问题》的博客，网址是http://blog.csdn.net/zhoufoxcn/archive/2006/08/30/1142685.aspx，博文发布以后有很多朋友在博文下面留言，因为它们也遇到了类似的问题。我记得我在2006年使用Proxool的时候版本就已经是0.8.3了，最近在Hibernate中发现它也带了这个Java数据库连接池实现，它的版本依然是0.8.3，应该是这些年来没有更新了。前些天研究一个项目的时候发现了项目中使用了BoneCP这个Java的数据库连接池，抱着好奇的态度学习了一下，觉得还不错，所以写了这篇博文跟大家分享一下。BoneCP也是一个开源的Java数据库连接池，它的官方网站网址是：http://jolbox.com/。据BoneCP网站官方报告称，BoneCP比性能排名第二的Java数据库连接池性高高25倍以上，并且支持Hibernate和DataNucleus这样的数据持久框架（当然支持JDBC这种直接方式了）。

使用BoneCP的必备条件

    使用BoneCP需要如下类库支持：
    被连接的数据库的JDBC驱动程序，这个可以到该数据库厂商网站下载；
    Google的集合框架Guava，它的网址是：http://code.google.com/p/guava-libraries/，这个需要说明的是BoneCP官方网站说的必备框架是Google Collect框架，但是这个框架已经不再支持了，而是转为新的集合框架Guava；
    SLF4J日志类库（在早期的BoneCP版本中直接使用了Log4J类库）；
    JDK1.5及更高版本。
 

也就是需要了如下Jar包：

    bonecp-0.7.0.jar
    mysql-connector-java-5.1.13-bin.jar
    slf4j-log4j12-1.6.1.jar
    slf4j-api-1.6.1.jar
    log4j-1.2.16.jar
    guava-r07.jar
    
为了得到比较详细的运行过程信息，需要添加一个log4j的配置文件log4j.properties，log4j.properties的文件位置如上图，文件内容如下：

    #log4j.rootLogger=DEBUG,CONSOLE,A1,im   
    log4j.rootLogger=DEBUG,CONSOLE  
    log4j.addivity.org.apache=true   
    # 应用于控制台   
    log4j.appender.CONSOLE=org.apache.log4j.ConsoleAppender   
    log4j.appender.Threshold=DEBUG   
    log4j.appender.CONSOLE.Target=System.out   
    log4j.appender.CONSOLE.layout=org.apache.log4j.PatternLayout   
    log4j.appender.CONSOLE.layout.ConversionPattern=[framework] %d - %c -%-4r [%t] %-5p %c %x - %m%n   
    #log4j.appender.CONSOLE.layout.ConversionPattern=[start]%d{DATE}[DATE]%n%p[PRIORITY]%n%x[NDC]%n%t[THREAD] n%c[CATEGORY]%n%m[MESSAGE]%n%n   
    #应用于文件   
    #log4j.appender.FILE=org.apache.log4j.FileAppender   
    #log4j.appender.FILE.File=file.log   
    #log4j.appender.FILE.Append=false   
    #log4j.appender.FILE.layout=org.apache.log4j.PatternLayout   
    #log4j.appender.FILE.layout.ConversionPattern=[framework] %d - %c -%-4r [%t] %-5p %c %x - %m%n   
    # Use this layout for LogFactor 5 analysis   
    # 应用于文件回滚   
    # log4j.appender.ROLLING_FILE=org.apache.log4j.RollingFileAppender   
    # log4j.appender.ROLLING_FILE.Threshold=ERROR   
    # log4j.appender.ROLLING_FILE.File=rolling.log  //????,??????${java.home}?rolling.log  
    # log4j.appender.ROLLING_FILE.Append=true       //true\:??  false\:??  
    # log4j.appender.ROLLING_FILE.MaxFileSize=10KB   //??????  
    # log4j.appender.ROLLING_FILE.MaxBackupIndex=1  //???  
    # log4j.appender.ROLLING_FILE.layout=org.apache.log4j.PatternLayout   
    # log4j.appender.ROLLING_FILE.layout.ConversionPattern=[framework] %d - %c -%-4r [%t] %-5p %c %x - %m%n   
    #应用于socket    
    #log4j.appender.SOCKET=org.apache.log4j.RollingFileAppender   
    #log4j.appender.SOCKET.RemoteHost=localhost   
    #log4j.appender.SOCKET.Port=5001   
    #log4j.appender.SOCKET.LocationInfo=true   
    # Set up for Log Facter 5   
    #log4j.appender.SOCKET.layout=org.apache.log4j.PatternLayout   
    #log4j.appender.SOCET.layout.ConversionPattern=[start]%d{DATE}[DATE]%n%p[PRIORITY]%n%x[NDC]%n%t[THREAD]%n%c[CATEGORY]%n%m[MESSAGE]%n%n   
    # Log Factor 5 Appender   
    #log4j.appender.LF5_APPENDER=org.apache.log4j.lf5.LF5Appender   
    #log4j.appender.LF5_APPENDER.MaxNumberOfRecords=2000   
    # 发送日志给邮件   
    #log4j.appender.MAIL=org.apache.log4j.net.SMTPAppender   
    #log4j.appender.MAIL.Threshold=FATAL   
    #log4j.appender.MAIL.BufferSize=10   
    #log4j.appender.MAIL.To\=web@www.wusetu.com  
    #log4j.appender.MAIL.SMTPHost=www.wusetu.com   
    #log4j.appender.MAIL.Subject=Log4J Message   
    #log4j.appender.MAIL.layout=org.apache.log4j.PatternLayout   
    #log4j.appender.MAIL.layout.ConversionPattern=[framework] %d - %c -%-4r [%t] %-5p %c %x - %m%n   
    # 用于数据库   
    log4j.appender.DATABASE=org.apache.log4j.jdbc.JDBCAppender   
    log4j.appender.DATABASE.URL=jdbc:mysql://localhost:3306/test  
    log4j.appender.DATABASE.driver=com.mysql.jdbc.Driver   
    log4j.appender.DATABASE.user=root   
    log4j.appender.DATABASE.password=jeri   
    log4j.appender.DATABASE.sql=INSERT INTO LOG4J (Message) VALUES ('[framework] %d - %c -%-4r [%t] %-5p %c %x - %m%n')   
    log4j.appender.DATABASE.layout=org.apache.log4j.PatternLayout   
    log4j.appender.DATABASE.layout.ConversionPattern=[framework] %d - %c -%-4r [%t] %-5p %c %x - %m%n   
    #log4j.appender.A1=org.apache.log4j.DailyRollingFileAppender   
    #log4j.appender.A1.File=SampleMessages.log4j   
    #log4j.appender.A1.DatePattern=yyyyMMdd-HH'.log4j'   
    #log4j.appender.A1.layout=org.apache.log4j.xml.XMLLayout   
    #自定义Appender   
    #log4j.appender.im = net.cybercorlin.util.logger.appender.IMAppender   
    #log4j.appender.im.host = mail.cybercorlin.net   
    #log4j.appender.im.username = username   
    #log4j.appender.im.password = password   
    #log4j.appender.im.recipient = corlin@cybercorlin.net  
    #log4j.appender.im.layout=org.apache.log4j.PatternLayout   
    #log4j.appender.im.layout.ConversionPattern =[framework] %d - %c -%-4r [%t] %-5p %c %x - %m%n 


在JDBC中使用BoneCP

在JDBC中使用BoneCP相当简单，设置一些关于数据库连接池的参数信息，比如连接池的最大、最小连接数等。下面是一个简单的例子：
 
    package com.netskycn;  
    import java.sql.Connection;  
    import java.sql.ResultSet;  
    import java.sql.SQLException;  
    import java.sql.Statement;  
     
    import com.jolbox.bonecp.BoneCP;  
    import com.jolbox.bonecp.BoneCPConfig;  
       
    /** 测试bonecp数据库连接池的工具  
     * @author zhoufoxcn 周公  
     * @version 0.1  
     * 说明：这是一个在JDBC中使用BoneCP的例子  
     * 2010-11-23  
     */ 
    public class MainClass {  
        public static void main(String[] args) {  
            BoneCP connectionPool = null;  
            Connection connection = null;  
       
            try {  
                //加载JDBC驱动  
                Class.forName("com.mysql.jdbc.Driver");  
            } catch (Exception e) {  
                e.printStackTrace();  
                return;  
            }  
              
            try {  
                //设置连接池配置信息  
                BoneCPConfig config = new BoneCPConfig();  
                //数据库的JDBC URL  
                config.setJdbcUrl("jdbc:mysql:///jobeet");   
                //数据库用户名  
                config.setUsername("root");   
                //数据库用户密码  
                config.setPassword("jeri");  
                //数据库连接池的最小连接数  
                config.setMinConnectionsPerPartition(5);  
                //数据库连接池的最大连接数  
                config.setMaxConnectionsPerPartition(10);  
                //  
                config.setPartitionCount(1);  
                //设置数据库连接池  
                connectionPool = new BoneCP(config);  
                //从数据库连接池获取一个数据库连接  
                connection = connectionPool.getConnection(); // fetch a connection  
                  
                if (connection != null){  
                    System.out.println("Connection successful!");  
                    Statement stmt = connection.createStatement();  
                    ResultSet rs = stmt.executeQuery("SELECT * FROM customer");   
                    while(rs.next()){  
                        System.out.println(rs.getInt(1)+":"+rs.getString("firstname")+","+rs.getString("lastname"));   
                    }  
                }  
                //关闭数据库连接池  
                connectionPool.shutdown();   
            } catch (SQLException e) {  
                e.printStackTrace();  
            } finally {  
                if (connection != null) {  
                    try {  
                        connection.close();  
                    } catch (SQLException e) {  
                        e.printStackTrace();  
                    }  
                }  
            }  
        }  
    } 


这个例子相当简单，但是是利用BoneCP提供的连接池而不是直接使用JDBC来管理连接的。

使用DataSource的话，代码如下：

    //加载数据库驱动  
    Class.forName("com.mysql.jdbc.Driver");  
    //创建一个DataSource对象  
    BoneCPDataSource ds = new BoneCPDataSource();  
    //设置JDBC URL  
    ds.setJdbcUrl("jdbc:mysql:///jobeet");  
    //设置用户名  
    ds.setUsername("sa");  
    //设置密码  
    ds.setPassword("jeri");  
    //下面的代码是设置其它可选属性  
    //ds.setXXXX(...);    
    Connection connection;  
    connection = ds.getConnection();  
              
    //这里操作数据库  
    //...  
    //关闭数据库连接  
    connection.close();  
    ds.close(); 

在Hibernate中使用BoneCP

在Hibernate中使用BoneCP除了需要上面提到的jar包之外，还需要下载一个名为bonecp-provider-0.7.0.jar的bonecp-provider的jar包，它的下载位置是：http://jolbox.com/bonecp/downloads/maven/com/jolbox/bonecp-provider/0.7.0/bonecp-provider-0.7.0.jar。
除此之外，还需要做如下配置：
 
    <!-- Hibernate SessionFactory --> 
    <bean id="sessionFactory" class="org.springframework.orm.hibernate.LocalSessionFactoryBean" autowire="autodetect"> 
        <property name="hibernateProperties"> 
            <props> 
                <prop key="hibernate.connection.provider_class">com.jolbox.bonecp.provider.BoneCPConnectionProvider</prop> 
                <prop key="hibernate.connection.driver_class">com.mysql.jdbc.Driver</prop> 
                <prop key="hibernate.connection.url">jdbc:mysql://127.0.0.1/yourdb</prop> 
                <prop key="hibernate.connection.username">root</prop> 
                <prop key="hibernate.connection.password">abcdefgh</prop> 
                <prop key="bonecp.idleMaxAge">240</prop> 
                <prop key="bonecp.idleConnectionTestPeriod">60</prop> 
                <prop key="bonecp.partitionCount">3</prop> 
                <prop key="bonecp.acquireIncrement">10</prop> 
                <prop key="bonecp.maxConnectionsPerPartition">60</prop> 
                <prop key="bonecp.minConnectionsPerPartition">20</prop> 
                <prop key="bonecp.statementsCacheSize">50</prop> 
                <prop key="bonecp.releaseHelperThreads">3</prop> 
            </props> 
        </property> 
    </bean> 

这样就可以在Hibernate中使用BoneCP了。

周公没有真实去测试BoneCP的性能，个人觉得它的用法确实比较简单，并且和不再维护的Proxool相比至少它还在一直更新和维护，不失为一个不错的备选方案。附件是本文提到的一些必备的jar包。