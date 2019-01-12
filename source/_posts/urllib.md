---
title: urllib
date: 2019-01-12 17:59:58
tags:
---

python2中的urllib和urllib2合在一起了，统一成python3中的urllib库，其中有4个模块：request、error、parse、robotparser

- request模块用来打开和读取url
- error模块包括一些request产生的错误，用try来捕捉处理
- parse模块用来解析url
- robotparser模块用来解析robots.txt文件


# request #

---
打开并读取URL，可以

- 打开直接一个url链接，
- 也可以由自己制定一个request对象来打开


### url.request.urlopen() ###

- 作用：打开一个url请求， 并得到返回的响应

- 参数
	- url : 网址链接
	- data ：post提交的参数，可以不写，默认为None，用字典存储，请求发送前要转换成bytes格式
	- timeout : 设置访问超时时间
	

- 返回对象: HTTPResponse对象

- 返回对象提供的方法
	- read() ：读取网页源代码，数据格式为bytes，需要用decode()转成str格式，类似还有readline(), readlines(), fileno(), close()
	- geturl() : 得到自己开始请求的url
	- getcode() : 得到http状态码，200代表成功
	- info() : 返回HTTPMessage对象，表示服务器响应返回的头信息



虽然可以发送请求，但不可以自己设置请求头部中的数据，urllib.request模块提供了Request()方法来自定义一个request请求对象，然后用上面的urllib.request.urlopen()来发送该request对象

### url.request.Request() ###

- 作用：产生一个request对象，可以用上面的urlopen()来请求这个对象

- 参数
	- url : 网址链接
	- data : post请求的参数,可以不写，默认为None，用字典存储，请求发送前要转换成bytes格式，也可以先不处理，然后在urlopen()时添加data
	- headers : 用字典来存储头部数据，默认为{}, 发送请求前不需要转换数据格式

- 返回对象: HTTPRequest

- 返回对象提供的方法
	- add_data(data) : 设置追加data
	- has_data() : 查看是否有了data参数
	- get_data() : 获取data参数的数据
	- add_header(key, value) : 添加头部信息，即按字典格式，key和value
	- get\_full_url() : 获取请求的url
	- get_method() : 请求方法，一般返回GET或者POST
	- set_proxy(host, type) : 设置代理，第一参数是socket（ip：端口）,第二个参数是代理类型，http或https

创建好request对象后即可用urllib.request.urlopen()来请求发送

PS：data参数设置了即为post，没有设置即为get，get传递参数需要先把参数数据字典格式转成bytes然后追加到url上




# error #
---

### urllib.error.URLError ###

所有异常的基类，所有请求过程中的异常都可以被这个捕获到，通过reason属性来查看异常原因


    try:
		re = urllib.request.urlopen("http://www.baidu.com")
	except urllib.error.URLError as e:
		print(e.reason)

### urllib.error.HTTPError ###

http错误，具体没搞清楚，反正用上面的基类URLError就好了


# parse #
---

常用bytes和str的数据转换

data发送前，需要将字典str格式转成url的bytes格式

	data = urllib.parse.urlencode(data).encode('utf-8')

对于收到的HTTPResponse对象，读取的bytes格式要转成str

	re = re.read.decode('utf-8')

还有其他对url的处理,需要的时候再系统学吧

