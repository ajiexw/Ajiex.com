Tags: Linux Nginx

执行sudo nginx -s reload后出现：    
    
    nginx: [emerg] open() "/usr/local/Cellar/nginx/1.4.0/nginx.conf" failed (2: No such file or directory)

原因：把nginx进程杀死后，pid丢失了。

解决方案：

"nginx -s reload is only used to tell a running nginx process to reload its config. After a stop, you don't have a running nginx process to send a signal to. Just run nginx (possibly with a -c /path/to/config/file)"

    sudo nginx -c /usr/local/etc/nginx/nginx.conf
