Date: 2013-07-26 11:55 
Tags: Python

web.input()与web.data()是web.py模块中的两个函数，都用来获取http请求包中的信息，但是获取的信息不同。

web.input()获取的是通过http请求包第一行的url传入的参数，返回值是类似于字典的key-value对。由于GET和POST请求包都可以通过url传入参数，所以两种请求包均可使用web.input()来获取url传入的参数，当然，通常情况下使用web.input()来获取GET请求包的url参数。

示例：假如http请求包中的url处的内容是/index.html?name=kun&age=23，通过以下代码获取参数中的值：

	user_input = web.input()
	user_input.name   #'kun'
	user_input.age    #'23'
	
可以看出获取到的值都是字符串类型的。

web.data()获取的是http请求包中的实体正文，函数返回值类型是字符串。由于GET请求包中没有实体正文，所以GET请求包是不能使用web.data()函数的，只有POST请求包才可以使用web.data()函数。

示例：假如http的POST请求包的实体正文是：username=jay&password=123456，通过以下代码获取实体正文：

	web.data()
	＃'username=jay&password=123456'
可以看出，获取的实体正文的数据类型是一整个大的字符串。

##综合来说：

web.input()获取url参数，可以用于GET和POST请求。

web.data()获取实体正文，只能用用POST请求。
