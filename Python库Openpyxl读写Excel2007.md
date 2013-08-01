Date: 2013-07-16 15:15
Tags: Python Lib

工作中碰到将当前用户参加的活动数据从Mysql中导出到Excel发给客户的需求。编辑手动录入太浪费时间，昨天终于有时间实现这个需求了。

Openpyxl是一个用来处理excel文件的python代码库。能够完成对Excel常见的读写操作，包括合并单元格，调整单元格大小，设置单元格背景颜色，设置文本字体、大小、颜色等。不过只能用来处理Excel2007及以上版本的文件（优点也是缺点），也就是 .xlsx/.xlsm 格式的表格文件。需求python的版本是2.6+。


##安装

可以在这里下载openpyxl的代码包：[https://pypi.python.org/pypi/openpyxl](https://pypi.python.org/pypi/openpyxl)

	tar -xzvf openpyxl.tar.gz
	cd openpyxl
	python setup.py install

##文档

[Openpyxl - A Python library to read/write Excel 2007 xlsx/xlsm files](http://pythonhosted.org/openpyxl/)
	
##写

	#-*- coding:utf-8 -*  	
	import MySQLdb  
	import time  
	import sys  
	from openpyxl.workbook import Workbook  
	from openpyxl.writer.excel import ExcelWriter  
	from openpyxl.cell import get_column_letter  

	def export(title,header,rows):
    	wb = Workbook()  
    	ew = ExcelWriter(workbook = wb)  
    	dest_filename = r'/aoaola/web/html/excel/' + title +'.xlsx'  
    	ws = wb.worksheets[0]  
    
    	#设置表头：header = ['用户Id','昵称','邮箱', '真实姓名','地址', '电话']
    	for x in range(1,len(header)+1):  
        	col = get_column_letter(x)  
        	ws.cell('%s%s'%(col, 1)).value = '%s' % (header[x-1])        
		
		#rows = [[],[],[]…]
    	for i in range(len(rows)):
        	assert len(rows[i]) == 6
        	for j in range(len(rows[i])):
            ws.cell(row=i+1,column=j).value = rows[i][j]

    	ew.save(filename = dest_filename)  


##Python假代码

	import web    	
	from tool import exportToExcel

	class ExportEvent:
    	def GET(self, i=None):        
       	title = '…'
        	header = ['用户Id','昵称','邮箱', '真实姓名','地址', '电话']
        	rows = []
        	for i in winners:
            	rows.append([i.Userid,i.username,i.email,i.realname,
            i.address,i.phone_number])
        exportToExcel.export(title,header,rows) 
        return page_helper.redirectTo('/html/excel/%s.xlsx'%(unicode(title, 'utf-8')))
	

##读

	from openpyxl import load_workbook
 
	wb = load_workbook(filename=r'existing_file.xlsx')
 
	sheets = wb.get_sheet_names()   #获取所有表格(worksheet)的名字
	sheet0 = sheets[0]  #第一个表格的名称
	ws = wb.get_sheet_by_name('sheet_name') #获取特定的 worksheet
 
	#获取表格所有行和列，两者都是可迭代的
	rows = ws.rows
	columns = ws.columns
 
	#行迭代
	content = []
	for row in rows:
    	line = [col.value for col in row]
    	content.append(line)
 
	#通过坐标读取值
	print ws.cell('B12').value    # B 表示列，12 表示行
	print ws.cell(row=12, column=2).value


##其他类似的代码库
Python还有一些内置的类似的代码库，不过都有不少局限性。

比如 [xlrd、xlwt、xlutils－读、写、修改](http://www.python-excel.org/)，pyExcelerator两者只能支持到excel2003，生成的Excel的后缀是xls，遇到导出大量数据（超过65535条）的需求时，Excel2003就不够用了。

win32com(pywin32扩展)是python到VBA的接口，python利用win32com直接调用office API，只能在windows下运行。office下的所有功能，都可以通过这个接口用python实现。致命的缺点是速度慢，它是所有操作excel的库中最慢的一个。


##延伸：xls和xlsx是两种完全不同的格式

xls的列数有最大256的限制，xlsx没有。

2007版本之前的xls的核心结构是复合文档类型，xlsx的核心结构是XML类型，基于XML的压缩方式，使其占用的空间更小。名字中最后一个'x'的意义就在于此。
