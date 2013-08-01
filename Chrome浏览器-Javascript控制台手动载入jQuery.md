Date:2013-07-31 22:42
Tags: Js

在控制台输入：

	var fileref=document.createElement('script')
	fileref.setAttribute("type","text/javascript")
	fileref.setAttribute("src", 'http://ajax.microsoft.com/ajax/jquery/jquery-1.4.min.js')
	document.getElementsByTagName("head")[0].appendChild(fileref)