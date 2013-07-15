Tags: Linux SSH

新的机子使用git，提示需要配置ssh密钥。

另一台机子也还在使用，做下笔记：

    $ ssh-keygen -t rsa   #命令行运行该命令，连续3次回车即可，在~/.ssh中生成id_rsa,id_rsa.pub
    #备份id_rsa,id_rsa.pub
    scp username@old_computer_ip:/home/username/.ssh/id_rsa ~/.ssh/     #复制旧机子上的密钥
    scp username@old_computer_ip:/home/username/.ssh/id_rsa.pub ~/.ssh/


SSH是一种网络协议，用于计算机之间的加密通信。

##公钥Public Key与私钥Private Key

SSH需要生成公钥Public Key和私钥Private Key, 常用的是使用RSA算法生成id_rsa.pub和id_rsa。

公钥Public Key(id_rsa.pub)是可以暴露在网络传输上的，是不安全的。而私钥Private Key(id_rsa)是不可暴露的，只能存在客户端本机上。

所以公钥Public Key(id_rsa.pub)的权限是644，而私钥Private Key(id_rsa)的权限只能是600。如果权限不对，SSH会认为公钥Public Key(id_rsa.pub)和私钥Private Key(id_rsa)是不可靠的，就无法正常使用SSH登陆了。

同时在服务端会有一个~/.ssh/authorized_keys文件，里面存放了多个客户端的公钥Public Key(id_rsa.pub)，就表示拥有这些Public Key的客户端就可以通过SSH登陆服务端。

##SSH公钥登陆过程

1、客户端发出公钥登陆的请求(ssh user@host)；

2、服务端返回一段随机字符串；

3、客户端用私钥Private Key(id_rsa)加密这个字符串，再发送回服务端；

4、服务端用~/.ssh/authorized_keys里面存储的公钥Public Key去解密收到的字符串。如果成功，就表明这个客户端是可信的，客户端就可以成功登陆。


由此可见，只要多台电脑上的的公钥Public Key(id_rsa.pub)和私钥Private Key(id_rsa)是一样的，对于服务端来说着其实就是同一个客户端。所以可以通过复制公钥Public Key(id_rsa.pub)和私钥Private Key(id_rsa)到多台电脑来实现共享登陆。
