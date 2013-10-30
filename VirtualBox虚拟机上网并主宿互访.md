Date:2013-08-25
Tags:Linux SSH

VirtualBox是一款x86虚拟机软件。原由德国innotek公司开发，2008年Sun收购了Innotek，而Sun于2010年被Oracle收购，2010年1月21日改 名成Oracle VM VirtualBox。VirtualBox 可在 Linux、Windows、Mac主机中运行，并支持在其中安装 Windows (NT 4.0、2000、XP、Server 2003、Vista)、DOS/Windows 3.x、Linux (2.4 和 2.6)、OpenBSD 等系列的客户操作系统。

##VirtualBox的5种网络模式

一、NAT〈网络地址转换模式〉

VirtualBox 中虚拟的主机（以下简称：虚拟主机）并不真实存在于网络中，宿主机和宿主机网络中的任何主机都不能直接访问虚拟主机，各虚拟主机也互不相通。虚拟主机能访 问宿主机，以及宿主机能访问的任何主机。虚拟主机访问网络是先通过 VirtualBox 转换后再发送出去的，数据接收也是先由 VirtualBox 接收后再转换转发到虚拟主机。

虚拟主机网络参数默认从 VirtualBox 的虚拟 DHCP Ser 获取：

![虚拟主机参数](http://ww2.sinaimg.cn/large/68796df6jw1e7yvmjuil3j209h02d3yl.jpg)
 
VirtualBox 端口转发：宿主机和宿主机网络中的任何主机都不能直接访问虚拟主机，但是VirtualBox 提供了端口转发，使得我们可以设置特定的端口供实体网络访问。

二、Bridged Adapter〈桥接模式〉

虚拟主机通过 VirtualBox 桥接到宿主机的一个网卡中，就像真实存在于宿主机网络中的一台主机一样。虚拟主机能与宿主机和宿主机网络中的主机完美互通。被桥接的网卡会开启混杂模式。

虚拟主机网络参数 ： 手动配置成桥接的宿主机网卡的同一网段，或通过宿主机网络中的 DHCP Ser 获取。

如果宿主机在一个受限制的网络虚拟主机是不能访问互联网的，如：宿主机使用ADSL、使用使用代理或VPN、宿主机网关做了MAC地址限制……

三、Internal〈内部模式〉

虚拟机与外界完全分开，虚拟机与宿主机网络不能互通，只有 同一名称（如：intnet）的内部网络模式的虚拟机之间才能互相访问 ，还有一个条件是在同一网段哦。

虚拟主机网络参数 ： 默认不分配IP，需要自已手动设置。

四、Host-only Adapter〈宿主机模式〉

VirtualBox在宿主机中虚拟一个 host-only 网卡，然后把虚拟主机桥接到 host-only 网卡上，我们可以通过设置host-only网卡（共享、桥接）来实现网络连接。

虚拟主机网络参数 ：默认IP段为192.168.56.X/24

五、未指定

如果你选择“未指定”，你将不能和任何主机通信，只能够自已 ping 自已啦！

From:[http://reverland.bitbucket.org/VirtualBox_net.html](http://reverland.bitbucket.org/VirtualBox_net.html)

##实现主宿访问和虚拟机上网

实现环境：宿主机（mac）+虚拟机（Ubuntu）

设置DHCP 这个一般默认都会设置好,可以不用管.

VirtualBox-偏好设置-网络

![网络](http://ww2.sinaimg.cn/large/68796df6jw1e7yw0ijr04j20m80ifach.jpg)

![服务器](http://ww3.sinaimg.cn/large/68796df6jw1e7yw2ev37yj20lu0i4tbk.jpg)

虚拟机Ubuntu-设置-网络

![网卡1](http://ww3.sinaimg.cn/large/68796df6jw1e7yw45wtapj20m00i8dil.jpg)

![网卡2](http://ww1.sinaimg.cn/large/68796df6jw1e7yw5bwb0zj20lx0i4whb.jpg)

![网卡3](http://ww3.sinaimg.cn/large/68796df6jw1e7yw5szxa8j20lq0hygof.jpg)


##其他问题

Ubuntu开机后仍无法上网，ifconfig发现第二块网卡默认没有启动。可以通过ifconfig命令让Ubuntu开启第二块网卡，方式如下：

	sudo ifconfig eth1 up
	sudo dhclient eth1

再次ifconfig查看发现第二块网卡已经启动并获得IP地址。但是当重新启动虚拟机后，Ubuntu不会自己驱动第二块网卡，还要手动的运行ifconfig来驱动。为了彻底解决这个问题，需要修改/etc/network/interfaces文件。具体方式如下：

	sudo vim /etc/network/interfaces
	
	#修改后的文件：

    # This file describes the network interfaces available on your system
    # and how to activate them. For more information, see interfaces(5).

    # The loopback network interface
    auto lo
    iface lo inet loopback

    # The primary network interface (Host-only)
    auto eth0
    iface eth0 inet dhcp

    # The second network interface (NAT)
    auto eth1
    iface eth1 inet dhcp

这样，我们既能访问虚拟机，又可以访问互联网。

SSH登录提示“port 22 connection refused”的原因为ssh服务端没有开启。

##苹果升级Mavericks后'丢失的vbox-vboxnet0'bug

    sudo /Library/StartupItems/VirtualBox/VirtualBox restart

    打开虚拟机偏好设置，点击'网络'：

    ![网络](http://ww3.sinaimg.cn/large/68796df6jw1ea310t91q8j20dw09wwfh.jpg)

参考：

[https://github.com/mitchellh/vagrant/issues/1671](https://github.com/mitchellh/vagrant/issues/1671)

[http://blog.csdn.net/dongjideyu/article/details/8822263](http://blog.csdn.net/dongjideyu/article/details/8822263)


##参考

[http://vb2005xu.iteye.com/blog/1733809](http://vb2005xu.iteye.com/blog/1733809)

[http://ww3.sinaimg.cn/large/68796df6jw1e7yw5szxa8j20lq0hygof.jpg](http://ww3.sinaimg.cn/large/68796df6jw1e7yw5szxa8j20lq0hygof.jpg)
