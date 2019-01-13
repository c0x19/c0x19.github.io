---
title: cookiejar
date: 2019-01-12 20:35:06
tags:
---

cookiejar是http库的一个模块，其中有4个主要的类，CookieJar，FileCookieJar，MozillaCookieJar，LWPCookieJar

- CookieJar()：管理HTTP cookie值，是存储cookie的对象
- FileCookieJar(filename, delayload=None, policy=None) : 由CooikeJar派生的，增加了将cookie与文件交互的功能
- MozillaCookieJar(filename, delayload=None, policy=None)：由FileCookieJar派生的，创建与Mozilla浏览器 cookies.txt兼容的FileCookieJar实例
- LWPCookieJar(filename, delayload=None, policy=None) : 由FileCookieJar派生的，创建与libwww-per标准的Set-Cookie3文件格式兼容的FileCookieJar实例

大多情况只用CookieJar(), 如果需要和本地文件交互，就用MozillaCookieJar()或LWPCookieJar()


根据urllib的学习

1. 用urllib.request.urlopen()可以请求一个url链接，并可以加上data和tiemout
2. 但1没法解决设置头部数据，这里就有了urllib.request.Request()，可以自己定义一个request对象，用这个可以设置头部和数据，然后用urllib.request.urlopen()来发送该对象

如果要设置cookie呢，上面这些都无法完成

# CookieJar #
---
这里就用到了http库中的cookiejar，设置带cookie请求具体步骤如下

1. 创建一个存储cookie的cookiejar实例

	`cookie = http.cookiejar.CookieJar()`

2. 创建对上面cookie实例的处理器

	`handler = urllib.request.HTTPCookieProcessor(cookie)`

3. 创建关于该处理器的opener

	`opener = urllib.request.build_opener(handler)`

opener是我们自己制定的一个和cookie关联的opener实例，以前用的urlopen也是一种opener实例，opener和以前的urlopen一样，可以打开url或者request对象，但完成了和cookie的关联

	import urllib.request
	import urllib.error
	import urllib.parse
	import http.cookiejar

	url = 'http://www.baidu.com'
	req = urllib.request.Request(url)
	cookie = http.cookiejar.CookieJar()
	handler = urllib.request.HTTPCookieProcessor(cookie)
	opener = urllib.request.build_opener(handler)
	try:
    	rep = opener.open(req)
    	print(req.get_method())
    	print(rep.info())
    	for item in cookie:
        	print('name:' + item.name)
        	print('value:' + item.value)
	except urllib.error.URLError as e:
    	print(e.reason)


# cookie与本地文件交互 #
---

可以用MozillaCookieJar或LWPCookieJar来把获取的cookie保存在文件中

## 写入文件 ##
    # 将cookie保存到文件中
	import urllib.request
	import urllib.error
	import urllib.parse
	import http.cookiejar

	url = 'http://www.baidu.com'
	req = urllib.request.Request(url)
	filename = 'cookie.txt'  # 同目录的文件名，需要自己创建好，写入时会覆盖已有的数据
	cookie = http.cookiejar.MozillaCookieJar(filename)
	handler = urllib.request.HTTPCookieProcessor(cookie)
	opener = urllib.request.build_opener(handler)
	try:
    	rep = opener.open(req)
    	print(req.get_method())
    	print(rep.info())
    	for item in cookie:
    	    print('name:' + item.name)
    	    print('value:' + item.value)
    	cookie.save(ignore_discard=True, ignore_expires=True)
    	# discard表示即使cookies将被丢弃也将它保存下来，expires表示覆盖原文件写入
	except urllib.error.URLError as e:
    	print(e.reason)


## 从文件中读取 ##


	# 从文件中读取cookie
	urllib.request
	import urllib.error
	import urllib.parse
	import http.cookiejar

	url = 'http://www.baidu.com'
	req = urllib.request.Request(url)
	filename = 'cookie.txt'
	cookie = http.cookiejar.MozillaCookieJar()
	cookie.load(filename, ignore_discard=True, ignore_expires=True)
	handler = urllib.request.HTTPCookieProcessor(cookie)
	opener = urllib.request.build_opener(handler)
	try:
    	rep = opener.open(req)
    	print(rep.read().decode('utf-8'))
	except urllib.error.URLError as e:
    	print(e.reason)
