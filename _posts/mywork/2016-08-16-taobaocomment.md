---
layout: post  
title: "Python天猫淘宝评论爬虫"
date: 2016-08-16
author: hunterhug
categories: [科学]
desc: "一只淘宝Python爬虫,支持抓取商品评论，批量导出EXCEL"
tags: ["爬虫","Python","淘宝"]
permalink: "/mywork/taobaocomment.html"
--- 

搬砖的陈大师版权所有,转载请注明：www.lenggirl.com/mywork/taobaocomment.html

# 说明
由于Github 打包的exe某些文件上传被.gitignore了，所以不提供windows二进制包

<img src='https://raw.githubusercontent.com/hunterhug/taobaocomment/master/seeme0.jpg' />

>[https://github.com/hunterhug/taobaocomment](https://github.com/hunterhug/taobaocomment)<br/>
>一个抓取淘宝评论的Python爬虫<br/>
>一个抓取淘宝天猫评论的爬虫,使用python3.4，爬虫程序已经封装好<br/>
>支持抓取天猫/淘宝的评论

# 使用
安装python3 [下载链接](https://www.python.org/downloads/)  然后设置环境变量设置 

1.安装模块请使用

```
sudo pip3 install pymysql
sudo pip3 install xlsxwriter
```

2.打包windows二进制

从万能仓库 http://www.lfd.uci.edu/~gohlke/pythonlibs/#cx_freeze 下载对应版本的打包库,然后:

```
pip3 install cx_Freeze-4.3.4-cp35-none-win_amd64.whl
```

转到源代码文件夹

```
python setup.py build
```

3.运行

把exe.win32-3.4移到根目录taobaocomment，任意改名，以下改为exe，文件目录如下：

```
taobaocomment
    -------source
    -------exe
    -------excel
```

先往taobao.txt中按行写入商品链接

```
网址格式有如下几种(#开头的网址直接被忽视):
https://item.taobao.com/item.htm?spm=a1z10.1-c.w4004-7841214359.4.6lp4CP&id=525842199471
https://detail.tmall.com/item.htm?spm=a230r.1.14.6.o11GZI&id=41389303364&cm_id=140105335569ed55e27b&abbucket=3
https://item.taobao.com/item.htm?id=40066362090
https://rate.tmall.com/list_detail_rate.htm?itemId=45789209181&sellerId=2274061169&order=3&currentPage=2
https://item.taobao.com/item.htm?spm=a230r.1.14.37.B4SrL2&id=45804256292&ns=1&abbucket=3#detail
http://a.m.taobao.com/i44628690678.htm?&abtest=16&sid=ceff9ca9adb294b3459844e3640a76d1
```

执行
```
run.bat
或者
python taobaocomment.py
```

# 演示
<img src='https://raw.githubusercontent.com/hunterhug/taobaoscrapy/master/seeme1.jpg' />


