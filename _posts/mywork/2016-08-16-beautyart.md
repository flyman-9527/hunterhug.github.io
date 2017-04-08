---
layout: post  
title: "Beego作品:缀美画室官网"
date: 2016-08-16
author: hunterhug
categories: [科学]
desc: "Golang画室网站，采用beego1.6框架所写，包含RBAC,多模板等功能，已经发布到beego官网，参见github"
tags: ["Golang","beego","Web"]
permalink: "/mywork/beautyart.html"
--- 

搬砖的陈大师版权所有,转载请注明：www.lenggirl.com/mywork/beautyart.html

# 项目介绍
Github:[https://github.com/hunterhug/beautyart](https://github.com/hunterhug/beautyart)

>项目名：广州缀美美术学校|缀美画室官网<br/>
>开发语言:Go!!!!


    // ░░░░░░░░▌▒█░░░░░░░░░░░▄▀▒▌
    // ░░░░░░░░▌▒▒█░░░░░░░░▄▀▒▒▒▐
    // ░░░░░░░▐▄▀▒▒▀▀▀▀▄▄▄▀▒▒▒▒▒▐
    // ░░░░░▄▄▀▒░▒▒▒▒▒▒▒▒▒█▒▒▄█▒▐ wordpress的多模板切换，能行的，加油！
    // ░░░▄▀▒▒▒░░░▒▒▒░░░▒▒▒▀██▀▒▌
    // ░░▐▒▒▒▄▄▒▒▒▒░░░▒▒▒▒▒▒▒▀▄▒▒
    // ░░▌░░▌█▀▒▒▒▒▒▄▀█▄▒▒▒▒▒▒▒█▒
    // ░▐░░░▒▒▒▒▒▒▒▒▌██▀▒▒░░░▒▒▒▀▄一个支持多用户权限控制，拥有基本博客功能，可扩展的网站应用
    // ░▌░▒▄██▄▒▒▒▒▒▒▒▒▒░░░░░░▒▒▒▒暂时有用户管理，文章管理，相册管理，评论，可扩展式内容管理系统
    // ▀▒▀▐▄█▄█▌▄░▀▒▒░░░░░░░░░░▒▒▒


# 项目规划
>环境要求：golang=1.6,mysql=5.6
><p>框架：	beego
><p>起始时间：2016.6.10
><p>结束时间：2016.8.8

# 项目托管
>阿里云服务器： Ubuntu
><p>软件:ngnix
><p>域名：http://www.beautyart.top ,http://beauty.lenggirl.com

# 文件目录

    beautyart
    ----conf 配置文件夹
    
        ----app.conf 		应用配置文件
        ----local_**.ini 	国际化文件
    
    ----controllers 控制器
        ----admin	后台控制器
            ----blog 博客模块
            ----rbac 权限模块
        ----home 	前台控制器
    
    -----lib 公共库
    -----file 上传文件保存地址
    -----models ORM模型
        ----admin RBAC主要数据库
        ----blog 博客主要数据库
        ----home 
    
    ----routers 路由
    ----static  静态文件
    ----views	视图
        ----admin 	后台视图
            ----default 默认主题
        ----home 	前台视图
            ----default 默认主题
    
    ----log 日志
    ----doc 说明文档
    ----test 测试文件夹


# 运行步骤
1. 运行init.sh进行包初始化或者根据提示go install
2. 接着

```
	git clone https://www.github.com/hunterhug/beautyart
	cd beautyart
	go build main.go
	./main -s
	./main
```

# 项目约定
>RBAC权限相关的models统一放在admin文件夹，其他都放在home文件夹.
	前台控制相关的controllers统一放在home文件夹，其他都放在admin文件夹
	URL router统一M/C/A方式，该正则url需要验证权限，如rbac/public/index，其他如public/index不验证。

>登录说明
	登陆过的用户只能注销后登录，支持定义cookie登录。进入后台时验证session，session不存在则验证cookie，如果用户未被冻结，增加session，
	同时更改用户登录时间、登录IP等，cookie与登录IP绑定。

>系统时间默认数据库本地时间为东八区，北京时间。

>后台模板在views/admin，前台模板在views/home，子文件夹为主题，默认主题为default

>所有配置在conf文件夹conf/app.conf，支持国际化

>数据库数据填充在models/*/*Init.go中定义

>视图模板均放在static中

>图片上传参考文档doc/文件上传说明.md(待写)

>Category模型说明

```
	Siteid    int64  //0缀美   1其他网站
	Type int64     //0表示文章 1表示相册
```

>前台首页配置（可动态调整首页）

```
{"1":{"name":"每日动态","limit":6},
"2":{"name":"画室动态","limit":6},
"3":{"name":"招生动态","limit":6},
"4":{"name":"美术资讯","limit":6},
"5":{"name":"高考喜报","limit":6},
"6":{"name":"学员风采","limit":3},
"7":{"name":"教师风采","limit":3},
"8":{"name":"学生作品","limit":6}}
```

# 温馨提示
1. 注意休息，程序猿朋友们
2. 请多参考beego官网及多看框架源代码

# 项目进展
1. 开发手脚架搭建完毕，RBAC模块完成，2016/7/2
2. 文章相册模块合二为一，完成一半，2016/7/15
3. 文章模块完成，待copy相册模块，2016/7/24
4. 轮转模块、网站配置模块完成，后台功能完成，准备前端设计，2016/7/28
5. 前端准备仿照http://www.caa.edu.cn/index.html
6. 首页改造完成，修复后台bug，差其余前端 2016/7/31
7. 前端开发完毕  2016/8/2
8. 内容填充完毕　2016/8/5

# 待做
1. 图片裁剪或者调用七牛云，阿里云等（一周时间）
2. 文章历史记录归档，方便查看，否则乱啊（一周时间）
3. APP应用，前端判断设备进行前端页面切换（一周时间）
4. 域名备案（愁啊。。。。无限延期）

# 特别说明
>平台使用说明参见doc文件夹
><p>可自由修改源代码，但必须保留友好链接
><p>[http://beauty.lenggirl.com](广州缀美美术学校官网|缀美画室)

# 参考
>1. 基于角色的访问控制（Role-Based Access Control）作为传统访问控制
>2. 使用beego框架和大量javascript脚本ajax调用
>3.　Amaze UI v2.7.0和jQuery EasyUI 1.4.2、Bootstrap混合（xx)
>4. 图片延迟加载

# 前端展示
<img src='https://raw.githubusercontent.com/hunterhug/beautyart/master/seeme.jpg' />
<img src='https://raw.githubusercontent.com/hunterhug/beautyart/master/seemok.png' />