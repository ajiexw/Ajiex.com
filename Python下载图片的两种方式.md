Date:2013-07-31 
Tags: Python

在python中可以用urllib方便的实现图片和flash下载：

##方法一：非线程安全

	import urllib
	
	url = "http://ajix.com/img/pic.jpg"
	path = "/home/ajie/img"
	
	data = urllib.urlopen(url).read()
	f = file(path, 'wb')
	f.write(data)
	f.close()
	
	
##方法二：线程安全

	import urllib
	
	url = "http://ajix.com/img/pic.jpg"
	path = "/home/ajie/img"
	data = urllib.urlretrieve(url, path)
