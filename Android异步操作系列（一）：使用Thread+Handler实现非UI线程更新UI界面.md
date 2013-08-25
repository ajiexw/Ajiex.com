Date: 2013-08-16
Tags: Android

转载来源：[http://blog.csdn.net/mylzc/article/details/6736988](http://blog.csdn.net/mylzc/article/details/6736988)

##新开线程处理繁重任务

为了给用户带来良好的交互体验，对于运算量较大的操作和IO操作，需要新开线程来处理这些繁重的工作，以免阻塞UI线程。但是新开线程不能操作UI元素。

##非UI线程不能操作UI元素

每个Android应用程序都运行在一个dalvik虚拟机进程当中，进程开始的时候会启动一个主线程（Main Thread），主线程负责处理和UI相关的事件，因此主线程通常又叫做UI线程。而由于Android采用UI单线程模型，所以只能在主线程中对UI元素进行操作。如果在非UI线程对UI进行了操作，则会报错：“CalledFromWrongThreadException:only the original thread that created a view hierarchy can touch its views”。

##一、利用消息循环机制实现线程间通信，完成UI操作

Android提供了消息循环的机制，我们可以利用这个机制来实现线程间的通信。通过在非UI线程发送消息到UI线程，完成对UI元素的操作。

##示例：

使用Thread+Handler方式，实现在非UI线程发送消息——>通知UI线程更新界面。

不要忘记在AndroidMainfest.xml中设置网络权限：

	<uses-permission android:name="android.permission.INTERNET" />


