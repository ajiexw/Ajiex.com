Date:2013-08-01 19:45
Tags: Python

webpy中，使用web.input()方法可以返回一个web.storage对象(类似字典)，内容为从url(GET)或http header(POST)获取的变量，web.Storage对象可以直接使用'i.id'方式获取变量，字典类型需要使用i['id']。

如：你访问页面http://example.com/test?id=10，在Python后台想取得'id'的值,通过web.input()可以实现。

	class SomePage:
   		def GET(self):
       	i = web.input()
        	return i.id
        	
给id设定默认值：

    class SomePage:
   		def GET(self):
       	i = web.input(id='1')
        	return i.id
        	
注：web.input()取得的值都会被当作string类型,即使你传递的是一些数字.
        	
web.Stroage()返回的数据格式：
	
	data = {'name':'wuliang', 'age':'20', 'adress':'Beijing'}
	web.Storage(data) #返回的数据格式为：<Storage {'name':'wuliang', 'age':'20', 'adress':'Beijing'}>   
