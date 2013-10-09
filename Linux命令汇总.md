Tags: Linux

##sudo su :
用sudo命令获得root权限，然后用su命令切换至用户root，不需要输入密码。

##系统变量
直接在命令行输入$HOME时，当前目录变为HOME目录；

查看系统变量：echo $PATH

脚本文件test.py第一行添加'/usr/bin/python'用来告诉操作系统：当执行该程序时，应该运行哪个解释器。

也可以直接在命令行指定解释器，如：python test.py，此时，文件第一行可以不添加'/usr/bin/python'

当给文件添加执行权限'chmod a+x test.py'，在命令行直接通过指定文件的位置'./test.py'执行脚本时，第一行必须添加'/usr/bin/python'，我们用'./'来指示程序位于当前目录。

把文件shiyan.c的所有者改为wang。 
    
    $ chown wang shiyan.c 

把目录/his及其下的所有文件和子目录的属主改成wang，属组改成users。 
    
    $ chown -R wang.users /his 
    
改变/opt/local /book/及其子目录下的所有文件的属组为book。

    $ chgrp -R book /opt/local /book 


修改组用户权限

    chmod -R g+rwx *


