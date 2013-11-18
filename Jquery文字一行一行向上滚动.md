Date:2013-09-13
Tags:Fe

From: [http://tugenhua.blog.51cto.com/3912301/719078](http://tugenhua.blog.51cto.com/3912301/719078)

Jquery文字一行一行向上滚动 效果是用了Jquery中animate这个方法实现的 相对来说是蛮简单的 虽然是简单点 还是要分享下 基本原理就是：运用了一个小技巧 滚动的高度和每个li的高度一样的，先找到外层ul的容器 然后相对于外层的容器进行向上滚动 ul在css里面没有设置他的高度 默认情况下是n个li×li的高度 向上是marginTop： -li.height(每个li的高度);当滚动到最后一个li时候 那么最外层的容器应该滚动到0了 那么我就把第一个li加到ul里面去 这样就实现了循环滚动
而不会滚动到最后一个就停止下来了基本的HTML/CSS如下:


    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd"> 
    <html> 
        <head> 
            <meta http-equiv="Content-Type" content="text/html; charset=utf-8"> 
            <title>Untitled Document</title> 
        <style type="text/css"> 
            ul,li{list-style:none; margin:0; padding:0;} 
            .scroll{ width:500px; height:175px; overflow:hidden; border:1px solid #333; margin:50px auto 0;} 
            .scroll li{ width:500px; height:25px; line-height:25px; overflow:hidden;} 
            .scroll li a{ font-size:14px; font-family:"宋体";color:#333; text-decoration:none;} 
            .scroll li a:hover{ text-decoration:underline;} 
        </style> 
        <script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/1.5.1/jquery.min.js"></script> 
        </head> 
        <body> 
            <div class="scroll"> 
                <ul class="list"> 
                    <li><a href="#" target="_blank">我来博主的空间了</a></li> 
                    <li><a href="#" target="_blank">我来博主的空间了</a></li> 
                    <li><a href="#" target="_blank">我来博主的空间了</a></li> 
                    <li><a href="#" target="_blank">我来博主的空间了</a></li> 
                    <li><a href="#" target="_blank">我来博主的空间了</a></li> 
                    <li><a href="#" target="_blank">我来博主的空间了</a></li> 
                    <li><a href="#" target="_blank">我来博主的空间了</a></li> 
                </ul> 
            </div> 
            <script type="text/javascript" src="index.js"></script> 
        </body> 
    </html> 

JS代码如下：

    /** 
    * @author tugenhua 
    * @email tugenhua@126.com 
    * 一行一行文字向上滚动js 
    * 运用了Jquery中的animate动画方法 
    * 运用了一个小技巧 滚动的高度和每个li的高度一样 
    * 先找到外层ul的容器 然后相对于外层的容器进行向上滚动 ul没有设置他的高度 默认情况下是n个li×li的高度 向上是marginTop： -li.height(每个li的高度); 
    * 当滚动到最后一个li时候 那么最外层的容器应该滚动到0了 那么我就把第一个li加到ul里面去 这样就实现了循环滚动 
    * 而不会滚动到最后一个就停止下来了 
    */ 
     function autoScroll(obj){ 
             $(obj).find(".list").animate({ 
                         marginTop : "-25px" 
                             },500,function(){ 
                                         $(this).css({marginTop : "0px"}).find("li:first").appendTo(this); 
                                             }) 
     } 
     $(function(){ 
             setInterval('autoScroll(".scroll")',3000) 
     }) 

