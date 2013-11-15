Date:2013-11-15 23:47
Tags: ssh error

用ssh登录服务器时，长时间不操作，没有数据传输，会出现连接断开被服务器踢出的情况。常见的提示如下：

    Write failed: Broken pipe

解决此问题有三种方法:


##方案一：客户端编辑'/etc/ssh/ssh_config'文件，取消以下两行的注释

    ServerAliveInterval 60

    ServerAliveCountMax 3

此后该系统里的用户连接ssh时，每60秒就会发一个KeepAlive请求，以上两行代表允许超时60*3=180秒，避免被踢。


##方案二：在服务器端设置（会影响安全性），编辑'/etc/ssh/sshd_config'文件：

    ClientAliveInterval 60

    ClientAliveCountMax 5

ClientAliveInterval 60表示每分钟发送一次, 然后客户端响应, 这样就保持长连接了.

ClientAliveCountMax, 使用默认值3即可.ClientAliveCountMax表示服务器发出请求后客户端没有响应的次数达到一定值, 就自动断开.

然后重启OPENSSH服务器后该项设置就会生效。每一个连接到此服务器上的客户端都会受其影响。所以建议用客户端设置。


##方案三：如果只想让当前的ssh保持连接，可以使用以下的命令：

    ssh -o ServerAliveInterval=60 user@sshserver

