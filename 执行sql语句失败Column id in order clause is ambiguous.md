Date: 2013-11-20
Tags: Mysql Error


执行SQL语句：

    select * from Goods g join GoodsHasTag t on g.Goodsid=t.Goodsid where t.Tagid = '1' order by Goodsid desc;

报错：
    
    Column 'Goodsid' in order clause is ambiguous
 
 
正确写法（order排序应指明根据哪个表的Goodsid排名）：
   
    select * from Goods g join GoodsHasTag t on g.Goodsid=t.Goodsid where t.Tagid = '1' order by <span style="color:red">g.Goodsid</span> desc;
