Date: 2013-07-16 16:32
Tags: Python Error

页面执行以下代码，出现UnicodeDecodeError: 'ascii' codec can't decode byte 0xe5 in position 108: ordinal not in range(128)的错误。

      
 	return page_helper.redirectTo('/html//%s.xlsx' % (title))
 	
##解决方案一：

reload(sys)是动态重新加载sys模块，在正式运行时不建议使用这个函数，有可能使网站挂掉。
	
	import sys 
	reload(sys) 
	sys.setdefaultencoding('utf8')  #将str编码由ascii改为utf8  	
setdefaultencoding在系统启动的时候设置，就是你在控制台下输入python出现>>>的时候系统会自动读取的一个文件，所以在使用时就必须重新reload。

如果不想用reload，可以在/usr/lib/python2.4/ 下建立一个名为sitecustomize.py的文件，内容为： 
    
    import sys
    sys.setdefaultencoding('utf-8')
    
这样的话就可以不用每次去reload了，系统会在每次启动python的时候自动读取这个文件。

＃另：Python2.5初始化后会删除sys.setdefaultencoding方法，需要重新载入。	
##解决方案二： 	
 	
 	
 	return page_helper.redirectTo('/html/excel/%s.xlsx' % (unicode(title, 'utf-8')))

##延伸：

Python 里面的编码和解码也就是 unicode 和 str 这两种形式的相互转化。编码是 unicode -> str，相反的，解码就是 str -> unicode。

下面剩下的问题就是确定何时需要进行编码或者解码了，像一些库是 unicode 版的，这样我们在将这些库函数的返回值进行传输或者写入文件的时候就要考虑将它编码成合适的类型。

关于文件开头的“编码指示”，也就是 # -*- coding: -*- 这个语句。Python 默认脚本文件都是 ANSCII 编码的，当文件中有非 ANSCII 编码范围内的字符的时候就要使用“编码指示”来修正。

关于 sys.defaultencoding，这个在解码没有明确指明解码方式的时候使用。比如我有如下代码：

#! /usr/bin/env python   
# -*- coding: utf-8 -*-   
 
s = '中文' # 注意这里的 str 是 str 类型的，而不是 unicode   
s.encode('gb18030')  
这句代码将 s 重新编码为 gb18030 的格式，即进行 unicode -> str 的转换。因为 s 本身就是 str 类型的，因此Python 会自动的先将 s 解码为 unicode ，然后再编码成 gb18030。因为解码是python自动进行的，我们没有指明解码方式，python 就会使用 sys.defaultencoding 指明的方式来解码。很多情况下 sys.defaultencoding 是ANSCII，如果 s 不是这个类型就会出错。

拿上面的情况来说，我的 sys.defaultencoding 是 anscii，而 s 的编码方式和文件的编码方式一致，是 utf8 的，所以出错了:

UnicodeDecodeError: 'ascii' codec can't decode byte 0xe4 in position

0: ordinal not in range(128)

对于这种情况，我们有两种方法来改正错误：

一是明确的指示出 s 的编码方式

#! /usr/bin/env python   
# -*- coding: utf-8 -*-   
 
s = '中文'   
s.decode('utf-8').encode('gb18030')  
二是更改 sys.defaultencoding 为文件的编码方式

#! /usr/bin/env python   
# -*- coding: utf-8 -*-   
 
import sys   
reload(sys) # Python2.5 初始化后会删除 sys.setdefaultencoding 这个方法，我们需要重新载入   
sys.setdefaultencoding('utf-8')   
 
str = '中文'   
str.encode('gb18030') 
这样应该可以解决Jython中文乱码的问题了。
