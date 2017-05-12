---
layout: post  
title: "postgresql使用安装"
date: 2017-05-10
author: hunterhug
desc: "postgresql使用安装"
categories: [代码]
tags: ["sql"]
permalink: "/tool/postgresql.html"
--- 

# 安装

ubuntu安装
```
# 安装客户端
sudo apt-get install postgresql-client
# 安装服务器
sudo apt-get install postgresql
# 安装图形界面
sudo apt-get install pgadmin3
```

安装后默认生成一个名为postgres的数据库和一个名为postgres的数据库用户。这里需要注意的是，同时还生成了一个名为postgres的Linux系统用户。

# 登录

```
# 先切换到postgres用户
sudo su postgres
```

以下命令相当于系统用户postgres以同名数据库用户的身份

```
psql
```

完整登录命令

```
psql -U dbuser -d exampledb -h 127.0.0.1 -p 5432
```

上面命令的参数含义如下：-U指定用户，-d指定数据库，-h指定服务器，-p指定端口。

# 授权其他用户

授权其他用户登录
```
# 新建系统用户
sudo adduser jinhan

# 进入控制台
psql
```

先改下命令
```
\password postgres
```

创建用户，和新建系统用户一样
```
CREATE USER jinhan WITH PASSWORD 'password';
```

新建数据库并授权
```
CREATE DATABASE exampledb OWNER jinhan;
GRANT ALL PRIVILEGES ON DATABASE exampledb to jinhan;
```

控制台输入\q退出
```
\q
```

退出后,重新登录，命令需完整！
```
exit
psql -U jinhan -d exampledb -h 127.0.0.1 -p 5432
```

# 命令

除了前面已经用到的\password命令（设置密码）和\q命令（退出）以外，控制台还提供一系列其他命令。

        \h：查看SQL命令的解释，比如\h select。
        \?：查看psql命令列表。
        \l：列出所有数据库。
        \c [database_name]：连接其他数据库。
        \d：列出当前数据库的所有表格。
        \d [table_name]：列出某一张表格的结构。
        \du：列出所有用户。
        \e：打开文本编辑器。
        \conninfo：列出当前数据库和连接的信息。

基本的数据库操作，就是使用一般的SQL语言。

    # 创建新表
    CREATE TABLE user_tbl(name VARCHAR(20), signup_date DATE);

    # 插入数据
    INSERT INTO user_tbl(name, signup_date) VALUES('张三', '2013-12-22');

    # 选择记录
    SELECT * FROM user_tbl;

    # 更新数据
    UPDATE user_tbl set name = '李四' WHERE name = '张三';

    # 删除记录
    DELETE FROM user_tbl WHERE name = '李四' ;

    # 添加栏位
    ALTER TABLE user_tbl ADD email VARCHAR(40);

    # 更新结构
    ALTER TABLE user_tbl ALTER COLUMN signup_date SET NOT NULL;

    # 更名栏位
    ALTER TABLE user_tbl RENAME COLUMN signup_date TO signup;

    # 删除栏位
    ALTER TABLE user_tbl DROP COLUMN email;

    # 表格更名
    ALTER TABLE user_tbl RENAME TO backup_tbl;

    # 删除表格
    DROP TABLE IF EXISTS backup_tbl;



