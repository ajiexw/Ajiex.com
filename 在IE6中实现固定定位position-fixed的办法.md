Tags: IE6

##使元素固定在浏览器的顶部

	#top{
   		_position:absolute;
    	_bottom:auto;
    	_margin-top:10px; #控制元素的精确位置
    	_top:expression(eval(document.documentElement.scrollTop));
	}
	
##使元素固定在浏览器的底部

	#top{
   		_position:absolute;
    	_bottom:auto;
    	_margin-bottom:10px; #控制元素的精确位置
    	_top:expression(eval(document.documentElement.scrollTop+document.documentElement.clientHeight-this.offsetHeight-(parseInt(this.currentStyle.marginTop,10)||0)-(parseInt(this.currentStyle.marginBottom,10)||0)));
	}


##position:fixed闪动问题

用了上面的办法后，被固定定位的元素在滚动滚动条时会闪动。解决方案：

	*html{
   		background-image:url(about:blank);
   	 	background-attachment:fixed;
	}
