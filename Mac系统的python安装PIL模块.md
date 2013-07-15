Tags: Mac Python Lib

[来源：python安装Image模块](http://jordy.easymorse.com/?p=760)

python的图片处理库PIL（Python Image Library）可以转换图片的类型，修改图片大小、色值、透明度，将2张图片合成到1张图片。

下载安装PIL文件及依赖文件：
    
    http://www.pythonware.com/products/pil/i  下载最新版的PIL安装程序 ，目前是Imaging-1.1.7.tar.gz
    
    http://www.ijg.org  目前最新版本是jpegsrc.v8b.tar.gz，安装JPEG库
    
    http://www.gzip.org/zlib/ 目前版本是zlib-1.2.5.tar.gz

默认mac平台安装PIL不支持对JPEG格式图片的处理，需要安装jpegsrc.v8b.tar.gz 和zlib-1.2.5.tar.gz。
  
第一步 安装JPEG库，安装命令：
    
    tar -zxf jpegsrc.v8b.tar.gz
    cd jpeg-8b
    ./configure
    make
    make test 
    sudo make install

第二步  安装zlib库，安装命令：

    tar xfz zlib-1.2.5.tar.gz
    cd zlib-1.2.5
    ./configure
    make
    sudo make install
    
第三步  安装PIL库，安装命令：

    tar xfz Imaging-1.1.7.tar.gz
    cd Imaging-1.1.7
    python setup.py build_ext -i
    python setup.py build 
    sudo  python setup.py install
    #说明：python setup.py build_ext –i 命令是用于测试安装PIL库使用支持对哪些格式的图片处理。
    #安装时出现如下：
        JPEG support available
        ZLIB (PNG/ZIP) support available
    #说明前两步安装正确。

最后，测试PIL库是否安装成功

在终端输入指令：

    $python
    >>>import Image

或者写一个python脚本，代码：

    #!/usr/bin/python
    #-*- coding: utf-8 -*-
    import Image

    img = Image.open('page.jpg')
    #50,50是指新生成图片的宽高，Image.ANTIALIAS参数表示新生成的图片消除锯齿
    out = img.resize((50,50),Image.ANTIALIAS)
    #新生成图片名
    out.save('pagenew.jpg')
    没有出现报错，说明PIL库安装成功。
    注意：Image.open()和out.save()方法中的图片的路径正确。

