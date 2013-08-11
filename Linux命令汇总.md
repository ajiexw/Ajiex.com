Tags: Linux

##sudo su :
用sudo命令获得root权限，然后用su命令切换至用户root，不需要输入密码。

##系统变量
直接在命令行输入$HOME时，当前目录变为HOME目录；

查看系统变量：echo $PATH

脚本文件test.py第一行添加'/usr/bin/python'用来告诉操作系统：当执行该程序时，应该运行哪个解释器。

也可以直接在命令行指定解释器，如：python test.py，此时，文件第一行可以不添加'/usr/bin/python'

当给文件添加执行权限'chmod a+x test.py'，在命令行直接通过指定文件的位置'./test.py'执行脚本时，第一行必须添加'/usr/bin/python'，我们用'./'来指示程序位于当前目录。


