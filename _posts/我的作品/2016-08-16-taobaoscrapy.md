---
layout: post  
title: "Python手机淘宝关键字爬虫"
date: 2016-08-16
author: hunterhug
categories: [我的作品,爬虫点滴]
desc: "一只淘宝关键字Python爬虫,支持抓取商品标题/商品价格/商品销量/商品图片等"
tags: ["淘宝关键字","爬虫","Python"]
permalink: "/mywork/taobaoscrapy.html"
--- 

伟大的程序员版权所有,转载请注明：www.lenggirl.com/mywork/taobaoscrapy.html

Github：[https://github.com/hunterhug/taobaoscrapy](https://github.com/hunterhug/taobaoscrapy)

# 说明
由于Github 打包的exe某些文件上传被.gitignore了，所以不提供windows二进制包
更多参考：[一只尼玛博客园](http://www.cnblogs.com/nima/p/5324490.html)

<img src='https://raw.githubusercontent.com/hunterhug/taobaoscrapy/master/seeme0.jpg' />


>一个抓取淘宝天猫关键字搜索商品的爬虫使用python3.4，爬虫程序已经封装好<br />
><p>支持抓取商品标题/商品价格/商品销量/商品图片等<br />
><p>使用请直接点击exe文件夹中后缀为exe的文件或者run.bat<br />

---

>A scarpy for catch taobao item info
><p>using python3
><p>run just click exe/*.exe
><p>more please watch the pdf

更多说明参考pdf

# 使用

安装python3 https://www.python.org/downloads/  然后设置环境变量设置 

1.安装模块请使用

```
sudo pip3 install pymysql
sudo pip3 install xlsxwriter
```

下载图形包：http://www.lfd.uci.edu/~gohlke/pythonlibs/

```
Pillow, a replacement for PIL, the Python Image Library, which provides image processing functionality and supports many file formats.
Use `from PIL import Image` instead of `import Image`.

    Pillow-3.3.0-cp27-cp27m-win32.whl
    Pillow-3.3.0-cp27-cp27m-win_amd64.whl
    Pillow-3.3.0-cp34-cp34m-win32.whl
    Pillow-3.3.0-cp34-cp34m-win_amd64.whl
    Pillow-3.3.0-cp35-cp35m-win32.whl
    Pillow-3.3.0-cp35-cp35m-win_amd64.whl

```

```
pip3 install Pillow-3.3.0-cp34-cp34m-win32.whl
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

把exe.win32-3.4移到根目录taobaoscrapy，任意改名，以下改为exe，文件目录如下：

```
taobaoscrapy
-------source
-------exe
-------exehelp
-------help
```

```
run.bat
或者
python mtaobao.py
```

4.程序出错
有时候程序运行中途断网或者其他原因,如误点下载图片,而图片几万张不耐烦终止程序,导致程序<br/>
运行没完成。不必担心,只要原始数据在,一切好办。<br/>
将 data 中的原始数据移到 help 文件夹中

```
runhelp.bat
或者
python help.py
```

# 演示
<img src='https://raw.githubusercontent.com/hunterhug/taobaoscrapy/master/seeme1.jpg' />
<img src='https://raw.githubusercontent.com/hunterhug/taobaoscrapy/master/seeme2.jpg' />
<img src='https://raw.githubusercontent.com/hunterhug/taobaoscrapy/master/seeme3.jpg' />
<img src='https://raw.githubusercontent.com/hunterhug/taobaoscrapy/master/seeme4.jpg' />


Do not understand?contact me.<br/>
author:hunterhug<br/>
2015/11

--------------------------------------------------------------

# 补充
1.2016/7/7改bug

请查看JSON.json，淘宝json数据字段变更，导致程序出错<br/>

淘宝需要验证时，请往subcookie.txt填东西，参考pdf<br/>

 '手机折扣'字段失效
 
```
Traceback (most recent call last):
  File "mtaobao.py", line 322, in <module>
    itemlist.append(item['mobileDiscount'])
KeyError: 'mobileDiscount'
```

'URL地址'字段失效

```
Traceback (most recent call last):
  File "mtaobao.py", line 328, in <module>
    itemlist.append(item['auctionURL'])
KeyError: 'auctionURL'
```

已经更正

参考JSON可以加更多字段，请自行增加修改
