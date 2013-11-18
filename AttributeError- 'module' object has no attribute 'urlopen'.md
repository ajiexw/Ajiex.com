Date: 2013-07-25
Tags: Error Python


	import urllib2  
	response = urllib2.urlopen('http://www.baidu.com/')  
	html = response.read()  
	print html 
	
错误提示：AttributeError: 'module' object has no attribute 'urlopen'

原因：代码的文件名名称为urllib2.py，和python库的名称冲突。	 
