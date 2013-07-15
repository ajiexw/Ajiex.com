Tags: IE6

在IE6下，当父元素的子元素的样式拥有position:relative属性时，父元素的overflow:hidden属性就会失效。

CSS代码

	.box{
 		height:50px;
 		overflow:hidden;
 		border:1px solid red;
	}
	.son{
 		height:100px;
 		border:1px solid green;
 		position:relative;
	}

HTML代码
	
	<div>
		<div></div>
	</div>

在其他浏览器中正常，但在IE6中子元素的高度超出了父元素，也就是说“overflow:hidden”在IE6中并没有起到作用。

解决办法是：在父元素中增加position:relative。

原文件地址：http://www.sjyhome.com/ie6-bug-overflow-hidden.html