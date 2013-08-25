Date: 2013-08-25
Tags: SSH

##问题

	ssh ajie@192.168.1.100

	#ssh: connect to host XX.XX.XX.XX port 22: Connection refused
	
##检查

1、目标主机的ssh server端程序是否安装、服务是否启动，是否在侦听22端口；

	ps -ef|grep sshd
	#root      2859     1  020:29 ?        00:00:00 /usr/sbin/sshd -D

/usr/sbin/sshd为ssh clinet/server中server端的守护进程，如果上述结果中没有sshd出现，那么可能就是你的server端程序没有安装，或者sshd服务没有启动。（Ubuntu 11.04 默认没有安装ssh server，只安装了ssh client）。

2、是否允许该用户登录；

3、本机是否设置了iptables规则，禁止了ssh的连入/连出；

	sudo  iptables -L
	
4、查查ssh的配置文件
 	
 	ls -lrt /etc/ssh
	
##解决-Ubuntu如何开启ssh服务

SSH分客户端openssh-client和openssh-server
  
如果只是想登陆别的机器的SSH，只需要安装openssh-client，ubuntu有默认安装，如果没有则
	
	sudo apt-get install openssh-client
  
如果要使本机开放SSH服务，就需要安装openssh-server：

	sudo apt-get install openssh-server

然后确认sshserver是否启动了：
	
	ps -e |grep ssh


如果看到sshd那说明ssh-server已经启动。
  
如果没有则可以这样启动：sudo /etc/init.d/ssh start

ssh-server配置文件位于/etc/ssh/sshd_config，在这里可以定义SSH的服务端口，默认端口是22，你可以自己定义成其他端口号，如222。然后重启SSH服务：

	sudo /etc/init.d/ssh stop
	sudo /etc/init.d/ssh start
	
参考：[http://hi.baidu.com/leejun_2005/item/bfc0ded296cb8ebf32db907e](http://hi.baidu.com/leejun_2005/item/bfc0ded296cb8ebf32db907e)

##解决-mac上开启ssh服务

mac默认已经安装了openssh，所以只需要打开服务即可。

选择System prefrence（系统偏好设置） -> sharing（共享）, 将remote connectionx（远程登录）打开即可。
 


	