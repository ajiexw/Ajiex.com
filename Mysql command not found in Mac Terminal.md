Tags: Mac Mysql

##问题：

Mac终端中输入'mysql -uroot -p'，出现'mysql:command not found'的错误提示

需要'./usr/local/mysql/bin/mysql -u root -p'才可以。

##原因：

系统变量PATH中，没有mysql命令的所在路径。

##解决方法：
   
	#方法一,永久生效，针对所有用户
	vim /etc/profile
	#在适当位置添加 
	PATH=$PATH:/usr/local/mysql/bin
	#使PATH立即生效
	source profile

	#方法二，只针对当前用户
	vim ~/.bash_profile
	#在打开的文件中添加：
	export PATH="$PATH:/usr/local/mysql/bin"
	
	#方法三，只对当前会话有效
	PATH=$PATH:/usr/local/mysql/bin
	
	#使用以上方法，发现还是有问题，如下解决
	vim ~/.zshrc
	alias mysql='/usr/local/mysql/bin/mysql'
	
##延伸：

linux系统下，如果下载并安装了应用程序，很有可能在键入它的名称时出现“command not found”的提示内容。如果每次都到安装目标文件夹内，找到可执行文件来进行操作就太繁琐了。这涉及到环境变量PATH的设置问题，而PATH的设置也是在linux下定制环境变量的一个组成部分。

变量简介

Linux是一个多用户的操作系统。每个用户登录系统后，都会有一个专用的运行环境。通常每个用户默认的环境都是相同的，这个默认环境实际上就是一组环境变量的定义。用户可以对自己的运行环境进行定制，其方法就是修改相应的系统环境变量。

定制环境变量

环境变量是和Shell紧密相关的，用户登录系统后就启动了一个Shell。对于Linux来说一般是bash，但也可以重新设定或切换到其它的Shell（使用chsh命令）。
根据发行版本的情况，bash有两个基本的系统级配置文件：/etc/bashrc和/etc/profile。这些配置文件包含两组不同的变量：shell变量和环境变量。前者只是在特定的shell中固定（如bash），后者在不同shell中固定。很明显，shell变量是局部的，而环境变量是全局的。环境变量是通过Shell命令来设置的，设置好的环境变量又可以被所有当前用户所运行的程序所使用。对于bash这个Shell程序来说，可以通过变量名来访问相应的环境变量，通过export来设置环境变量。
注：Linux的环境变量名称一般使用大写字母

环境变量设置实例

1.使用命令echo显示环境变量
本例使用echo显示常见的变量HOME

	$ echo $HOME  
	/home/kevin

2.设置一个新的环境变量

	$ export MYNAME=”my name is kevin”
	$ echo $ MYNAME
	
	my name is Kevin

3.修改已存在的环境变量
接上个示例
	
	$ MYNAME=”change name to jack”
	$ echo $MYNAME
	change name to jack
	
4.使用env命令显示所有的环境变量

	$ env
	HOSTNAME=localhost.localdomain
	SHELL=/bin/bash
	….
		
5.使用set命令显示所有本地定义的Shell变量
	
	$ set
	BASH=/bin/bash
	BASH_ENV=/root/.bashrc
	……
	
6.使用unset命令来清除环境变量

	$ export TEMP_KEVIN=”kevin”     
	#增加一个环境变量TEMP_KEVIN
	$ env | grep TEMP_KEVIN
	#查看环境变量TEMP_KEVIN是否生效（存在即生效）
	TEMP_KEVIN=kevin #证明环境变量TEMP_KEVIN已经存在
	$ unset TEMP_KEVIN 
	#删除环境变量TEMP_KEVIN
	$ env | grep TEMP_KEVIN
	#查看环境变量TEMP_KEVIN是否被删除，没有输出显示，证明TEMP_KEVIN被清除了。

7.使用readonly命令设置只读变量
注：如果使用了readonly命令的话，变量就不可以被修改或清除了。
	
	$ export TEMP_KEVIN ="kevin"      
	#增加一个环境变量TEMP_KEVIN
	$ readonly TEMP_KEVIN 
	#将环境变量TEMP_KEVIN设为只读
	$ env | grep TEMP_KEVIN 
	#查看环境变量TEMP_KEVIN是否生效
	TEMP_KEVIN=kevin
	#证明环境变量TEMP_KEVIN已经存在
	$ unset TEMP_KEVIN  
	#会提示此变量只读不能被删除
	-bash: unset: TEMP_KEVIN: cannot unset: readonly variable
	$ TEMP_KEVIN ="tom"        
	#修改变量值为tom会提示此变量只读不能被修改
	-bash: TEMP_KEVIN: readonly variable

##学习总结

按变量的生存周期来划分，Linux变量可分为两类：
1.     永久的：需要修改配置文件，变量永久生效。
2.     临时的：使用export命令行声明即可，变量在关闭shell时失效。
