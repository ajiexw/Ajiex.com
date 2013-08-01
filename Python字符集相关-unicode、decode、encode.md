Date:2013-07-31 23:04
Tags:Python


##将字符串解码为unicode
一、python里打印中文的时候需要在字符串前面加'u',中文才能显示，这里面的u的作用就是将后面的字符串转换为unicode码

	print u"中文"

二、与之相关的是一个unicode()函数：unicode(str,"原来的编码")的作用是将str解码成unicode字符串。

	str="中文"
	str=unicode(str,"utf-8")
	print str



三、str.decode("原来的编码")：也是将str解码成unicode。这个函数等同于unicode(str,"原来的编码")。 

	print '\u6d4b\u8bd5'.decode('unicode_escape')   #返回'测试'

四、拼接u+字符串：u = eval('u"' + s + '"')

	s = '\u4f60\u597d\uff0c\u4ece\u6ce2\uff01'
	u = eval('u"' + s + '"')
	u    #输出u'\u4f60\u597d\uff0c\u4ece\u6ce2\uff01'
	print u
	

##将unicode转换成指定的字符集

	u.encode("指定的字符集")

##为什么不所有的文件都使用unicode，还要用GBK，utf-8等编码呢？

Unicode只是一个符号集，它只规定了符号的二进制代码，却没有规定这个二进制代码应该如何存储，一般不能直接保存。保存到磁盘上时，需要把它转换为对应的编码，如utf-8和utf-16。

##utf-8和unicode的关系

互联网的普及，强烈要求出现一种统一的编码方式。<strong style="color:green">UTF-8就是在互联网上使用最广的一种Unicode的实现方式。</strong>其他实现方式还包括UTF-16（字符用两个字节或四个字节表示）和UTF-32（字符用四个字节表示），不过在互联网上基本不用。

UTF-8最大的一个特点，就是它是一种变长的编码方式。可以使用1~4个字节表示一个符号，根据不同的符号而变化字节长度。
	
 

##示例：

假如在python源文件中指定使用编码cp936，'#coding=cp936或#-*- coding:cp936 -*-或#coding:cp936'的方式（不写默认是ascii编码）。

这样在源文件中的str对象就是cp936编码的，我们要把这个字符串传给一个需要保存成其他编码的地方（比如xml的utf-8,excel需要的utf-16）通常这么写： 
	
	strobj.decode("cp936").encode("utf-16") 