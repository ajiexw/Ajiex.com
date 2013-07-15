Tags: Nginx

##常用web服务器

web服务器也称为www服务器、HTTP服务器，其主要功能是提供网上信息浏览服务。

常用web服务器：Apache,Nginx,Lighttpd,Tomcat,IBM WebSphere,IIS(Internet Infornation Server)。

- Apache：重量级、消耗内存高、性能低。

- Tomcat：基于java的web应用软件容器，对静态文件、高并发的处理较弱。

- IBM-WebSphere：基于java的应用环境，范围从简单到高级，直到企业级，相对其他应用数量很少。

- IIS：提供图像界面的管理工具。

- Lighttpd：轻量级、内存开销低，cpu占用率低，效能好，模块丰富，德国人编写，是Nginx的竞争对手之一。

- Nginx：高性能的HTTP和反向代理服务器。能够支持高达50000个并发连接数的响应，内存、CPU消耗非常低，运行非常稳定。

##选择Nginx的理由：

1.可以高并发连接,官方测试Nginx能支撑5万并发连接，实际生产环境中可支撑2－4万并发连接数。得益于Nginx使用的最新的epoll(linux2.6内核)和kqueue(freebsd)网络I／O模型，而Apache使用的则是传统的select模型（多进程模式，需要经常派生子进程，消耗的cpu等服务器资源要高的多）。

2.内存消耗少

3.成本低廉

4.配置文件简单

5.支持Rewrite重写规则,能够根据域名、URL的不同，将HTTP请求分到不同的后端服务器群组。

6.内置的健康检查功能,如果Nginx Proxy后端的某台web服务器宕机了，不会影响前端访问。

7.节省带宽
支持GZIP压缩，可以添加浏览器本地缓存的Header头。

8.稳定性高
用于反向代理，宕机的概率微乎其微。

9.支持热部署,启动特别容易，几乎可以7天x24小时不间断运行，即使运行数个月也不需要重新启动。还能够在不间断服务的情况下，对软件版本进行升级。

##缺点

技术资料很少，安全性一般。


##nginx默认路径

安装路径：/usr/local/nginx

可执行文件安装路径：/usr/local/nginx/sbin/nginx

配置文件路径：/usr/local/nginx/conf/nginx.conf

Nginx.pid路径：/usr/local/nginx/logs/nginx.pid

错误日志路径:/usr/local/nginx/logs/error.log

访问日志路径：/usr/local/nginx/logs/access.log

##自定义编译选项示例：
    ./configure
        --prefix= /usr \
        --sbin-path= /usr/sbin/nginx \
        --conf-path= /etc/nginx/nginx.conf


###一、Nginx的启动
假设Nginx安装在/usr/local/nginx目录中，启动命令就是：

/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf

参数-c指定了配置文件的路径，如果不加-c参数，nginx会默认加载其安装目录的conf子目录中的nginx.conf文件。

###二、Nginx的停止

停止方法有很多种，一般通过发送系统信号给Nginx主进程的方式来停止Nginx。

可以通过ps命令来查找nginx的主进程号：

ps -ef | grep nginx

master process表示主进程，另外4个worker process表示子进程。

也可以通过nginx.pid文件来获取nginx主进程号，该文件存放的就是Nginx当前的主进程号。

1、从容停止Nginx:

kill - QUIT Nginx 主进程号

或

kill - QUIT `/usr/local/nginx/logs/nginx.pid`

2、快速停止Nginx:

kill - TERM Nginx 主进程号

或

kill - TERM `/usr/local/nginx/logs/nginx.pid`

或

kill - INT Nginx 主进程号

或

kill - INT `/usr/local/nginx/logs/nginx.pid`

3、强制停止所有nginx进程

pkill -9 nginx


###三、Nginx的平滑重启

通过发送系统信号给Nginx主进程的方式来进行。

首先确认配置文件的语法是否正确：

/usr/local/nginx/sbin/nginx -t -c /usr/local/nginx/conf/nginx.conf

重启命令：

kill -HUP Nginx主进程号

kill -HUP `/usr/local/nginx/logs/nginx.pid`


Nginx的信号控制：

TERM,INT 快速关闭

QUIT 从容关闭

HUP 平滑重启，重新加载配置文件

USR1 重新打开日志文件，在切割日志时用途较大

USR2 平滑升级可执行程序

WINCH 从容关闭工作进程

###四、Nginx的平滑升级

当需要将正在运行中的nginx升级、添加／删除服务器模块时，可以在不中断服务的情况下，使用新版本、重编译的nginx可执行程序替换旧版本的可执行程序。步骤如下：

1、使用新的可执行程序替换旧的可执行程序，对于编译安装的nginx,可以将新版本编译安装到旧版本的nginx安装路径中（替换之前，最好备份）。

2、发送以下指令：
    kill -USR2 旧版本的Nginx主进程号

3、旧版本nginx的主进程将重命名它的.pid文件为.oldbin(例如：/usr/local/nginx/logs/nginx.pid.oldbin），然后执行新版本的nginx可执行程序，依次启动新的主进程和新的工作进程。

4、此时，新旧版本的nginx实例会同时运行，共同处理输入的请求。要逐步停止旧版本的nginx实例，你必须发送WINCH信号给旧的主进程，然后，它的工作进程就将开始从容关闭：

    kill -WINCH 旧版本nginx的主进程

5、一段时间后，旧的工作进程处理了所有已连接的请求后退出，仅由新的工作进程来处理输入的请求。

6、这时候，可以决定是使用新版本还是使用旧版本：

    kill -HUP 旧的主进程号：nginx将在不重载配置文件的情况下启动它的工作进程。

    kill -QUIT 新的主进程号：从容关闭其工作进程

    kill -TERM 新的主进程号：强制退出

    kill 新的主进程号或旧的主进程号：如果因为某些原因新的工作进程不能退出，则向其发送kill信号。

新的主进程退出后，旧的主进程会移除.oldbin前缀，恢复它的.pid文件，这样，一切就恢复到升级之前了。如果尝试升级成功，而你也希望保留新的服务器时，可发送QUIT信号给旧的主进程，使其退出而只留下新的服务器运行。

    


##Nginx配置文件：

——————————————————————————————————————————————————————————————————————————————————————————————————

user www www;   #使用的用户和组
worker_processes 8;    #指定工作衍生进程数（一般等于cpu的总核数或总核数的两倍）
error_log /usr/local/nginx/logs/error.log crit;   #指定错误日志存放的路径，错误日志记录级别可选项为：debug,info,notice,warn,error,crit。
pid /usr/local/nginx/logs/nginx.pid    #指定pid存放的路径，存储nginx的主进程号
worker_rlimit_nofile 51200;   #指定文件描述符数量

events
{
use epoll;    #使用的网络I/O模型，linux系统推荐采用epoll模型，FreeBSD系统推荐采用kqueue模型
worker_connections 51200;    #允许的连接数
}

http
{
include mime.types;
default_type application/octet-stream;
charset gb2312;     #设置使用的字符集，如果一个网站有多种字符集，不要在这里随便设置，应让程序员在html代码中通过meta标签设置
server_names_hash_bucket_size 128;
client_header_buffer_size 32k;
large_client_header_buffers 4 32k;

client_max_body_size 8m;    #设置客户端能够上传文件的大小
sendfile on;
tcp_nopush on;

keepalive_timeout 60;
tcp_nodelay on;

fastcgi_connect_timeout 300;
fastcgi_send_timeout 300;
fastcgi_read_timeout 300;
fastcgi_buffer_size 64k;
fastcgi buffers 4 64k;
fastcgi_busy_buffers_size 128k;
fastcgi_temp_file_write_size 128k;

gzip on;    #开启gzip压缩
gzip_min_length 1k;
gzip_buffers 4 16k;
gzip_http_version 1.1;
gzip_comp_level 2;
gzip_types text/plain application/x-javscript text/css application/xml;
gzip_vary on;

limit_zone crawler $binary_remote_addr 10m;

server
    {
        listen 80;
        server_name www.yourdomain.com yourdomain.com;
        index index.html index.htm index.php;
        root /data0/htdocs;
        limit_conn crawlwr 20;

        location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
        {
            expires 30d;
        }

        location ~ .*\.(js|css)?$
        {
            expores 1h;
        }

        log_format access '$remote_addr - $remote_user [$time_local] "$request" '
                          '$status $body_bytes_sent "$http_referer" '
                          '"$http_user_agent" $http_x_forwarded_for';
        access_log /data1/logs/access.log access;

    ｝
}

———————————————————————————————————————————————————————————————————————————————————————————————————

虚拟主机

虚拟主机是使用特殊的软硬件技术，把一台运行在因特网上的服务器主机分成一台台“虚拟”的主机，每台虚拟主机都可以是一个独立的网站，可以具有独立的域名，具有完整的internet服务器功能（www,FTP,Email等），同一台主机上的虚拟主机之间是完全独立的。
虚拟主机提供了在同一台服务器、同一组nginx进程上运行多个网站的功能。

一个最简化的虚拟主机配置：
http
{
    server
    {
        listen 80;
        server_name _*;
        access_log logs/default.access.log combined;
        location / {
            index index.html;
            root /data0/htdocs;
        }
    }
}

跟Apache一样，Nginx也可以配置多种类型的虚拟主机：基于IP、基于域名、基于端口。
基于域名的虚拟主机是最常见的一种虚拟主机。
1、配置你的DNS服务器，将每个主机名映射到正确的IP地址；
2、配置Nginx服务器，令其识别不同的主机名。
这种虚拟主机技术，使很多虚拟主机可以共享一个ip地址，有效解决了ip地址不足的问题。
http
{
    #第一个虚拟主机,所有对域名aaa.domain.com的访问都由它来处理
    server
    {
        listen 80;  #监听的端口
        server_name aaa.domain.com;    #主机名称
        access_log logs/aaa.domain.com.access.log combined;    #访问日志文件存放的路径
        location /
        {
            index index.html index.htm;    #默认首页文件
            root /data0/htdocs/aaa.domain.com;   #html网页文件存放的目录
        ｝
    ｝

    #第二个虚拟主机，所有对域名bbb.otherdomain.com的访问都由它来处理
    server
    {
        listen 80;  #监听的端口
        server_name bbb.otherdomain.com;    #主机名称
        access_log logs/bbb.otherdomain.com.access.log combined;    #访问日志文件存放的路径
        location /
        {
            index index.html index.htm;    #默认首页文件
            root /data0/htdocs/bbb.otherdomain.com;   #html网页文件存放的目录
        ｝
    ｝

    #第三个虚拟主机，对域名www.domain.com,domain.com,以及除了aaa.domain.com之外的所有*.domain.com二级域名的访问都由它来处理
    server
    {
        listen 80;  #监听的端口
        server_name www.domain.com domain.com *.domain.com;    #主机名称
        access_log logs/bbb.domain.com.access.log combined;    #访问日志文件存放的路径
        location /
        {
            index index.html index.htm;    #默认首页文件
            root /data0/htdocs/domain.com;   #html网页文件存放的目录
        ｝
    ｝




Nginx日志文件配置与切割
与Nginx日志相关的指令有两条，一条是log_format,用来设置日志的格式；另外一条是access_log,用来指定日志文件的存放路径、格式和缓存大小。两条指令在配置文件中的位置可以在http{...}之间，也可以在虚拟主机之间，即server{...}两个大括号之间。

log_format指令用来设置日志的记录格式，语法：log_format name format [format ...]
其中name用来表示定义的格式名称，在配置文件中不可重复，format表示定义的格式样式。log_format有一个默认的、无须设置的combined日志格式设置，相当于Apache的combined日志格式，具体参数如下：
log_format combined '$remote_addr - $remote_user [$time_local]'
                    '"$request" $status $body_bytes_sent'
                    '"$http_referer" "$http_user_agent" $http_x_forwarded_for';




假设将Nginx服务器作为web服务器，位于负载均衡设备、Nginx反向代理之后，就不能获取到客户端的真实ip地址了。原因是经过反向代理之后，由于在客户端和web服务器之间增加了中间层，因此web服务器无法直接拿到客户端的ip，通过$remote_addr变量拿到的将是反向代理服务器的ip地址。但是，反向代理服务器在转发请求的http头信息中，可以增加X-Forwarded-For信息，可以记录原有的客户端ip地址和原来客户端请求的服务器地址。这时候就要用日志记录X-Forwarded-For信息中的ip地址，即客户的真实ip：用$http_x_forwarded_for变量记录用户的X-Forwarded-For IP地址。
log_format mylogformat '$http_x_forwarded_for - $remote_user [$time_local]'
                    '"$request" $status $body_bytes_sent'
                    '"$http_referer" "$http_user_agent"';

$remote_addr,$http_x_forwarded_for用于记录ip地址；
$remote_user用于记录远程客户端用户名称；
$time_local用于记录访问时间与时区；
$request用于记录请求url与http协议；
$status用于记录请求状态，例如成功时状态为200，页面找不到时状态为404;
$body_bytes_sent用于记录发送给客户端的文件主体内容大小；
$http_referer用于记录是从哪个页面链接访问过来的；
$http_user_agent用于记录客户端浏览器的信息。


用access_log指令指定日志文件存放路径
语法：access_log path [format [buffer=size | off]]
path表示日志文件的存放路径，format表示使用log_format指令设置的日志格式的名称，buffer=size表示设置内存缓冲区的大小，例如可以设置buffer=32k。
1、如果不想记录日志，可以关闭日志记录：
access_log off;
2、如果想使用默认的combined格式的日志记录：
access_log data1/logs/filename.log
或者
access_log data1/logs/filename.log combined;
3、自定义格式的日志记录，其中mylogformat是日志格式名称：
log_format mylogformat '$http_x_forwarded_for - $remote_user [$time_local]'
                    '"$request" $status $body_bytes_sent'
                    '"$http_referer" "$http_user_agent"';

access_log data1/logs/filename.log mylogformat buffer=32k;
4、Nginx0.7.4之后的版本中，access_log指令中的日志文件路径可以包含变量：
accesss_log data1/logs/$server_name.log combined;

如果日志文件路径中含有变量，将存在以下一些限制：
1）、Nginx进程设置的用户和组必须有对该路径创建文件的权限。假设nginx的user指令设置的用户名和用户组都是www,而/data1/logs/目录的用户名和用户组都为root,日志文件将无法被创建。
2）、缓冲将不会被使用；
3）、对于每一条日志记录，日志文件都将先打开文件，再写入日志记录，然后马上关闭。为了提高包含变量的日志文件存放路径的性能，需要使用open_log_file_cache指令设置经常被使用的日志文件描述符缓存。
open_log_file_cache指令主要用来设置含有变量的日志路径的文件描述符缓存，语法如下：
open_log_file_cache max=N [inactive=time] [min_uses=N] [vaild=time] | off
该指令默认是静止的，等同于：
    open_log_file_cache off;

各项参数说明如下：
max:设置缓存中的最大文件描述符数量。如果超过，则清除较不常用的。
inactive:设置一个时间，如果在设置的时间内没有使用此文件描述符，则自动删除此描述符。此参数为可选参数，默认的时间为10秒钟。
min_uses:在参数inactive指定的时间范围内，如果日志文件超过被使用的次数，则将该日志文件的描述符记入缓存。默认次数为1。
valid:设置多长时间检查一次，看一看变量指定的日志文件路径与文件名是否仍然存在。默认时间为60秒。
off:禁止使用缓存。
示例：open_log_file_cache max=1000 inactive=20s min_uses=2 valid=1m;

Nginx日志文件的切割
生产环境中的服务器，由于访问日志文件增长速度非常快，日志太大会严重影响服务器效率，同时为了方便对日志文件进行分析计算，需要对日志文件进行定时切割。有按月、按天、按小时切割等，最常用的是按天切割。
可以采用以下方式来实现日志文件的切割：

    mv /data1/logs/access.log /data1/logs/20121008.log
    kill -USR1 Nginx主进程号    #让Nginx重新生成一个新的日志文件/data1/log/access.log

 如果配置文件中使用了pid指令，指定了pid文件的存放路径，我们可以通过cat这个pid文件获得主进程号：
    kill -USR1 `cat /usr/local/nginx/logs/nginx.pid`

如果想每天定时切割日志，还需要借助crontab。我们可以写一个按天切割的日志，按年、月份目录存放日志的shell脚本：
    vim /usr/local/nginx/sbin/cut_nginx_log.sh

脚本内容：
    #!/bin/bash
    #这个脚本需在每天的00:00运行

    #Nginx日志文件的存放路径
    logs_path = '/data1/logs/'

    mkdir -p ${logs_path}$(date -d "yesterday" + "%Y")/$(date -d  "yesterday" + "%m")/
    mv ${logs_path}access.log ${logs_path}$(date -d "yesterday" + "%Y")/$(date -d  "yesterday" + "%m")/access_$(date -d "yesterday" + "%Y%m%d").log
    kill -USR1 `cat /usr/local/nginx/logs/nginx.pid`

另外，配置crontab每天凌晨00:00定时执行这个脚本：
    crontab -e
输入以下内容并保存：
    00 00 ＊ ＊ ＊ /bin/bash /usr/local/nginx/sbin/cut_nginx_log.sh




Nginx自动列目录设置
如果需在某一虚拟主机的location / ｛...}目录控制中配置自动列目录，只须加上如下代码：
    location / {
        autoindex on;
    }

还有两项跟自动列目录相关的指令：
autoindex_exact_size [ on | off ]  #设定索引时文件大小的单位
autoindex_localtime [ on | off ]   #开启以本地时间来显示文件时间的功能。默认为关（GMT时间）


Nginx的浏览器本地缓存设置
location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
{
    expires 30d;    #在浏览器本地缓存30天
}

location ~ .*\.(js|css)$
{
    expires 1h;    #在浏览器本地缓存1小时
}
