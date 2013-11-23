Date: 2013-11-23 11:45
Tags: Linux Mysql

##简介

Sphinx是一个基于SQL的全文检索引擎，可以结合MySQL,PostgreSQL做全文搜索，它可以提供比数据库本身更专业的搜索功能，使得应用程序更容易实现专业化的全文检索。

Sphinx特别为一些脚本语言设计搜索API接口，如PHP,Python,Perl,Ruby等，同时为MySQL也设计了一个存储引擎插件。

Sphinx单一索引最大可包含1亿条记录，在1千万条记录情况下的查询速度为0.x秒（毫秒级）。Sphinx创建索引的速度为：创建100万条记录的索引只需 3～4分钟，创建1000万条记录的索引可以在50分钟内完成，而只包含最新10万条记录的增量索引，重建一次只需几十秒。

##特性

- 高速索引 (在新款CPU上,近10 MB/秒);

- 高速搜索 (2-4G的文本量中平均查询速度不到0.1秒);

- 高可用性 (单CPU上最大可支持100 GB的文本,100M文档);

- 提供良好的相关性排名

- 支持分布式搜索;

- 提供文档摘要生成;

- 提供从MySQL内部的插件式存储引擎上搜索

- 支持布尔,短语, 和近义词查询;

- 支持每个文档多个全文检索域(默认最大32个);

##Ubuntu安装

    sudo apt-get install sphinxsearch

##配置文件

    /etc/sphinxsearch/sphinx.conf 

##开启sphinxsearch功能

编辑/etc/default/sphinxsearch文件，将'START=no'修改为'START=yes'

##建立索引

    sudo indexer –all 

##启动sphinx

    sudo /etc/init.d/sphinxsearch start

检索新创建的索引,可以使用search实用程序可以从命令行对索引进行检索，因为咱们是apt-get安装的，可以直接 /search test

    /usr/local/sphinx/bin/search test

##相关命令

    sudo indexer --all  //生成索引

    sudo searchd    //启动进程
    
    sudo searchd --stop     //停止进程

##所有代码

[https://github.com/zhaoxingjie/Python-library/tree/master/Sphinx](https://github.com/zhaoxingjie/Python-library/tree/master/Sphinx)

##参考文档

[http://www.wapm.cn/uploads/pdf/sphinx_doc_zhcn_0.9.pdf](http://www.wapm.cn/uploads/pdf/sphinx_doc_zhcn_0.9.pdf)

[http://www.sphinxsearch.org/sphinx-tutorial](http://www.sphinxsearch.org/sphinx-tutorial)


















