Date:2013-08-15
Tags: Android Error

##Android SDK更新以及ADT更新出现问题:

Failed to fectch URl https://dl-ssl.google.com/android/repository/addons_list.xml, 

reason: Connection to https://dl-ssl.google.com refused 

##解决方案：

修改hosts文件，在最后一行添加：74.125.237.1 dl-ssl.google.com 
