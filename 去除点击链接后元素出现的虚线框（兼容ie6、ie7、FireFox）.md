Tags: IE6

##IE6、IE7

###方法一：

采用htc文件的办法来解决,将以下代码保存为link.htc文件。

	<public:attach event="onfocus" onevent="hscfsy()"/>
	<script language="javascript">
	function hscfsy(){
	this.blur();
	}
	</script>


为链接定义CSS样式：

	a { behavior:url(line.htc); }
	
###方法二：
	
	a { star:expression(this.onFocus=this.blur()); }
	
###方法三：

	<a href="＃" hidefocus="true">...</a>	
##火狐FireFox

FF中新增一条样式定义来解决此问题。

	a:focus { outline:0; } 或者 a:focus { outline:none; }

