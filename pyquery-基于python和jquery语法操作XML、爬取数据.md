Date:2013-07-31 22:03
Tags: Python

PyQuery是一个类似于jQuery的Python库，也可以说是jQuery在Python上的实现。使用lxml来实现快速的xml和html的操作解析。

具体的PyQuery文档见：[https://pypi.python.org/pypi/pyquery](https://pypi.python.org/pypi/pyquery)，[pyquery: 基于python和jquery语法操作XML](http://geoinformatics.cn/lab/pyquery/) 

对十分熟悉jQuery的人来说，用这个来做蜘蛛爬数据-分析html并从中提取数据比用正则表达式容易的多。

Ubuntu下安装：

	sudo apt-get install python-pyquery

示例：

	from pyquery import PyQuery as pq
	
	html = pq(url='http://ajiex.com/') #html等同于jquery的$对象
	
	print html('title') #取得title元素
	print html("#logo").html() #根据ID获取元素
	print html('#topNav li:eq(5)').find('a').attr('href', 'http://ajiex.com').attr('href') #修改属性值
	
	
