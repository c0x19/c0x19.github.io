---
title: python实现贴吧自动签到
date: 2019-01-13 20:31:13
tags:
---

前面学urllib，学cookie，学re，都是为了这个小脚本，结果用urllib获取关注的贴吧列表时，发送请求会有个重定向，导致重定向后没有cookie，也就得到一个空列表，本以为requests库以urllib为基础建立，urllib应该更强大一点，特地系统学了学，结果还不如老老实实用requests库，requests库处理爬虫是真的厉害

# 签到的请求 #
---

打开一个贴吧签到页面，在签到前，F12，然后点击签到，即可得到签到时发送的请求，点击Network，要的是request请求，筛选XHR，有个add请求，明显就是我们要找的

![](https://i.imgur.com/6gxAuD3.png)

得到了请求的url `http://tieba.baidu.com/sign/add`

方法是POST，即还有参数，往下看，发现了参数格式

![](https://i.imgur.com/6KYGbCE.png)

tbs可以复用，即每个贴吧可以共用，现在只需要得到所有贴吧的名字，然后做好对应的data，post发送即可，cookie后面获取名字时再弄，毕竟先弄好名字列表，才能签到


# 获取关注的贴吧列表 #
---

从贴吧里找到我的贴吧界面
![](https://i.imgur.com/NsCxF09.png)

用F12打开后，点击Network，按一下F5重新加载一下网页，得到一堆结果，我们要的是request请求，筛选选择XHR后就简单多了

![](https://i.imgur.com/2V8If2H.png)



发现有个mylike请求，双击点开发现正是关注贴吧的列表

具体看一下该请求

![](https://i.imgur.com/oGDhEPF.png)
 
得到了请求的url为  `http://tieba.baidu.com/f/like/mylike?v=1547438620259`

请求方法为 GET

下面也有请求头部的信息，里面包含了cookie，不管那么多，直接全部复制头部信息，进入python代码里，处理只需加一下单引号和逗号就能构造好带有自己cookie的头部

![](https://i.imgur.com/GbthU4A.png)

代码里的头部构造，复制过来后修改的地方非常少，看着很花里胡哨，其实很省心

![](https://i.imgur.com/srwvyeV.png)

这里请求贴吧列表时有个重定向，用urllib中的opener不行，这里用requests的Session来处理，即可重定向时照样保存cookie信息

	like_url = 'http://tieba.baidu.com/f/like/mylike?v=1547424142305'
	s = requests.Session()
    html = s.get(like_url, headers=head)
    html = html.content.decode('gbk')

用正则表达式来从网页源码中找到关注贴吧的名字

	p = re.compile(r'title="(.+?)">\1</a></td>')
    like_result += p.findall(html)

具体正则怎么写，慢慢看网页元素，找对应名字所在字符串的模式


# 签到 #

---

思路简单，代码也简单，直接上代码

	import requests
	import urllib.error
	import re

	head = {
    	'Accept': 'text/html, */*; q=0.01',
    	'Accept-Encoding': 'gzip, deflate',
    	'Accept-Language': 'zh-CN,zh;q=0.9,en;q=0.8',
    	'Connection': 'keep-alive',
    	'Cookie': 'BAIDUID=6DBDEEF7FB3A324BC86AC949E15E20:FG=1; BIDUPSID=6DBDEEF7FB3A324B34C86AC949E15E20; '
              'PSTM=1547101880; BDORZ=B490B5EBF6F3CD402E515D22BCDA1598; TIEBA_USERTYPE=9c139eef6f78a9429a629ca7; '
              'bdshare_firstime=1547105555949; '
              	'BDUSS=kp5ZTRaeE9AAEAAAA6218rx'
              '-DGu7n7zrbEzMzHAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAFwyO1xcMjtcN; '
              'TIEBAUID=04e88654c6a4bf37b1a22cf8; '
              'STOKEN=0d161708b5a11e3029cca3d3b7a9fa5b4f9ccb486475ab12c3a8b; '
              'H_PS_PSSID=1442_21119_28132_28267; Hm_lvt_98b9d8c2fd6608d564bf2ac2ae642948=1547417142,1547419494,'
              '1547419533,1547424956; delPer=0; PSINO=5; '
              'PMS_JT=%28%7B%22s%22%3A1547425493177%2C%22r%22%3A%22http%3A//tieba.baidu.com/home/main%3Fun%3D%25E9'
              '%259D%2592%25E8%258B%25B9%25E6%259E%259C%25E5%2591%25B3%25E5%25A5%25B6%25E7%25B3%2596%26fr%3Dibaidu'
              '%26ie%3Dutf-8%22%7D%29; Hm_lpvt_98b9d8c2fd6608d564bf2ac2ae642948=1547425494',
    	'Host': 'tieba.baidu.com',
    	'Referer': 'http://tieba.baidu.com/i/i/forum',
    	'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) '
                  'Chrome/71.0.3578.98 Safari/537.36',
    	'X-Requested-With': 'XMLHttpRequest'
	}

	like_url = 'http://tieba.baidu.com/f/like/mylike?v=1547424142305'
	sign_url = 'http://tieba.baidu.com/sign/add'
	tbs = '4fb45fea4498360d1547435295'

	like_result = []
	try:
    	s = requests.Session()
    	html = s.get(like_url, headers=head)
    	html = html.content.decode('gbk')
    	p = re.compile(r'title="(.+?)">\1</a></td>')
    	like_result += p.findall(html)
	except urllib.error.HTTPError as e:
    	print(e.reason)

	for name in like_result:
    	data = {
    	    'ie': 'utf-8',
    	    'kw': name,
    	    'tbs': tbs
    	}
    	try:
    	    r = requests.post(sign_url, data=data, headers=head)
    	except urllib.error.HTTPError as e:
    	    print(e.reason)


cookie已悄悄修改，防止泄露 :(

放到自己的服务器上，用crontab来定时任务

`crontab -e`

设置每天6点执行该签到脚本，在里面加入这句话

`0 6 * * * python3 /root/python/tieba/sign.py`

保存退出

搞定，日后在搞输出签到日志