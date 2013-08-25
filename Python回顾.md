Date:2013-08-11
Tags:Python

使用Python写web程序一年零两个月了，基础尤其重要。

最近尤其发现，有很多不太确定的基础知识点不甚明了。今天开始，重新扫一遍过去看过的教程。

先是《简明python教程》：

##使用解释器或源文件执行python程序

脚本文件test.py第一行添加'/usr/bin/python'用来告诉操作系统：当执行该程序时，应该运行哪个解释器。

也可以直接在命令行指定解释器，如：python test.py，此时，文件第一行可以不添加'/usr/bin/python'

当给文件添加执行权限'chmod a+x test.py'，在命令行直接通过指定文件的位置'./test.py'执行脚本时，第一行必须添加'/usr/bin/python'，我们用'./'来指示程序位于当前目录。

##数据类型

在python中有4种类型的数：整数、长整数、浮点数、复数。

字符串：在单引号中的字符串与双引号中的字符串的使用完全相同--它们没有在任何方面有不同；

利用三引号，可以指示一个多行的字符串，可以在三引号中自由的使用单引号或双引号。

转义符：'what\'s your name?'

另一种表示方法："what's your name?"

在一个字符串中，行末的单独一个反斜杠表示字符串在下一行继续，而不是开始一个新的行。如：
"This is the forst sentence. \

This is the second sentence."

等价于"This is the forst sentence. This is the second sentence."

如果想要指示包含特殊字符的自然字符串，通过给字符串加上前缀r或R来指定。例如：r"Newslines are indicated by \n"。

Unicode是书写国际文本的标准方法，python允许你处理unicode文本，只需在字符串前加上前缀u或U.

字符串是不可变的。

一定要用自然字符串处理正则表达式。否则需要使用很多的反斜杠。例如，后向引用符可以写成'\\l'或r'\l'

变量是你的计算机当中存储信息的一部分内存。

##物理行与逻辑行

物理行是你在编写程序时看见的，逻辑行是python看见的单个语句。python假定每个物理行对应一个逻辑行。

如果你想要在一个物理行中使用多于一个逻辑行，需要使用';'来特别标明这种语法，分号表示一个逻辑行语句的结束。如：i=5;print i;不建议使用。

##运算符

** 幂：x**y返回x的y次幂，3**4得到81(3*3*3*3)

4/3：整数的除法得到整数结果,得到1

4.0/3或者4/3.0得到1.33333....

//取整除：返回商的整数部分，如5//3返回1

%取模：返回除法的余数，如5%3返回2

赋值运算符的结合规律为由右向左结合：即a=b=c被处理为a=(b=c)

打印输出变量+字符串的两种方式：
    
    #1
    str = "The num is %s"
    print str%5

    #2
    str = "The num is"
    print str,5

同if...else...elif语句相比，在某些场合，使用字典更加快捷。

可以在while循环中使用一个else从句，虽然比较多余。
    
    while:
        ...
    else:
        ...

python中的for循环：
    
    for i in range(1,5):
        print i
    else:
        ...

for i in range(1,5)等同于for i in [1,2,3,4],类似javascript中的for(int i=1;i<5;i++){}

for i in range(x,y)，默认的，range的步长为1，如果为range提供第三个数，那么它将成为步长。例如：range(1,5,2)给出[1,3]

break:停止执行循环语句，包括对应的循环else块。

continue:跳过当前循环块中的剩余语句，然后继续进行下一轮循环。

函数中的参数名称称为形参，而你提供给函数调用的值称为实参。

##函数内定义全局变量：使用global语句

    def func():
        global x
        print 'x is',x
        x=2
        print 'changed x to',x
    x = 50
    func()
    print 'now x is',x
    
    #输出
    x is 50
    changes x to 2
    now x is 2

##默认参数值

    def say(message,times=1):
        print message*times

    say('Hello')
    say('Word', 5)
    
有默认参数值的参数必须放在末尾。

##关键参数

通过使用关键字而不是位置来给函数指定实参。不必担心参数的顺序；假设其他参数都有默认值，可以只给我们想要的那些参数赋值。

    def func(a,b=5,c=10)    

##return语句

return语句用来从一个函数返回，即跳出函数。没有返回值的return语句等价于return None。每个函数都在结尾暗含有return None语句。

##文档字符串DocString

    def printStr():
        '''Print some strings.

        it's done.'''
        print 'yes'

    #输入文档字符串
    print printStr.__doc__
    
书写惯例：首行以大写字母开始，句号结尾。第二行是空行，从第三行开始是详细的描述。可以通过help(printStr)查看文档字符串。

系统的help()所做的就是抓取函数的__doc__属性，然后整洁的展示给你。

模块基本上就是一个包含了所有你定义的函数和变量的文件。

    import sys
    for i in sys.argv:
        print i
    print sys.path


当Python执行import sys语句的时候,它在sys.path变量中所列目录中寻找sys.py模块。

如果你想要直接输入argv变量到你的程序中(避免在每次使用它时打sys.),那么你可以使用 from sys import argv语句。如果你想要输入所有sys模块使用的名字,那么你可以使用from sys import *语句。一般说来,应该避免使用from..import而使用import语 句,因为这样可以使你的程序更加易读,也可以避免名称的冲突。

每个Python模块都有它的__name__,如果它是'__main__',这说明这个模块被用户单独运行而不是被import, 我们可以进行相应的恰当操作。

    if __name__ == '__main__':
        print 'This program is being run by itself'
    else:
        print 'I am being imported from another module'

可以使用内建的dir函数来列出模块定义的标识符。标识符有函数、类和变量。

在Python中有三种内建的数据结构——列表、元组和字典。

##数据结构

list是处理一组有序项目的数据结构,即你可以在一个列表中存储一个序列的项目。列表中的项目应该包括在方括号中,这样Python就知道你是在指明一个列表。一旦你创建了一个列表,你可以添加、删除或是搜索列表中的项目。由于你可以增加或删除项目,我们说列表是可变的数据类型,即这种类型是可以被改变的。

我们使用列表的sort方法来对列表排序。需要理解的是,这个方法影响列表本身, 而不是返回一个修改后的列表——这与字符串工作的方法不同。这就是我们所说的列表是 可 变的 而字符串是 不可变的 。

元组tuple和列表十分类似,只不过元组和字符串一样是 不可变的 即你不能修改元组。元组通过圆 括号中用逗号分割的项目定义。元组通常用在使语句或用户定义的函数能够安全地采用一组值 的时候,即被使用的元组的值不会改变。

含有0个或1个项目的元组。一个空的元组由一对空的圆括号组成,如myempty = ()。然而,含 有单个元素的元组就不那么简单了。你必须在第一个(唯一一个)项目后跟一个逗号,这样 Python才能区分元组和表达式中一个带圆括号的对象。即如果你想要的是一个包含项目2的元 组的时候,你应该指明singleton = (2 , )。

元组最通常的用法是用在打印语句中,print语句可以使用跟着%符号的项目元组的字符串。这些字符串具备定制的功能。

print的这个用法使得编写输出变得极其简单,它避免了许多字符串操作。它也避免了我们一直 以来使用的逗号。

记住字典dict中的键/值对是没有顺序的。如果你想要一个特定的顺序,那么你应该在使用前自己 对它们排序。

我们使用字典的items方法,来使用字典中的每个键/值对。这会返回一个元组的列 表,其中每个元组都包含一对项目——键与对应的值。我们抓取这个对,然后分别赋给for..in 循环中的变量name和address然后在for-块中打印这些值。

    ab = { 'Swaroop' : 'swaroopch@byteofpython.info', 'Larry' : 'larry@wall.org','Matsumoto' : 'matz@ruby-lang.org','Spammer' : 'spammer@hotmail.com' }

    for name, address in ab.items():
        print 'Contact %s at %s' % (name, address)

    #这种写法也可以执行成功，但可读性不好
    for na in ab.items():
        print 'Contact %s at %s' % na

可以使用in操作符来检验一个键/值对是否存在,或者使用dict类的has_key方法。

    if 'Guido' in ab: 
    #or ab.has_key('Guido')

列表、元组和字符串都是序列,序列的两个主要特点是索引操作符和切片操作符。索引操作符让我们可以从序列中抓取一个特定项目。切片操作符让我们能够获取序列的一个切片,即一部分序列。序列的 神奇之处在于你可以用相同的方法访问元组、列表和字符串。

可以用负数做切片。负数用在从序列尾开始计算的位置。例如,shoplist[:-1]会返回除了最后 一个项目外包含所有项目的序列切片。

如果你想要复制一个列表或者类似的序列或者其他复杂的对象(不是如整数那样的简单对象),那么你必须使用切片操作符来取得拷贝。
    
    mylist = shoplist[:]

字符串方法

    name = 'Swaroop' 
    
    if name.startswith('Swa'):
        print 'Yes, the string starts with "Swa"'
    
    if 'a' in name:
        print 'Yes, it contains the string "a"'
    
    #find方法用来找出给定字符串在另一个字符串中的位置,或者返回-1以表示找不到子字符串。
    if name.find('war') != -1:
        print 'Yes, it contains the string "war"'
    
    delimiter = '_*_'
    mylist = ['Brazil', 'Russia', 'India', 'China'] 
    print delimiter.join(mylist)

##备份脚本示例：

    #!/usr/bin/python
    # Filename: backup_ver4.py
    
    import os import time

    # 1. The files and directories to be backed up are specified in a list.
    source = ['/home/swaroop/byte', '/home/swaroop/bin']
    
    # If you are using Windows, use source = [r'C:\Documents', r'D:\Work'] or something like that
    # 2. The backup must be stored in a main backup directory
    target_dir = '/mnt/e/backup/' # Remember to change this to what you will be using
    
    # 3. The files are backed up into a zip file.
    # 4. The current day is the name of the subdirectory in the main directory today = target_dir + time.strftime('%Y%m%d')
    # The current time is the name of the zip archive
    now = time.strftime('%H%M%S')
    
    # Take a comment from the user to create the name of the zip file comment = raw_input('Enter a comment --> ')
    if len(comment) == 0: # check if a comment was entered
        target = today + os.sep + now + '.zip' else:
        target = today + os.sep + now + '_' + \ comment.replace(' ', '_') + '.zip'
    
    # Notice the backslash!
    # Create the subdirectory if it isn't already there if not os.path.exists(today):
        os.mkdir(today) # make directory
        print 'Successfully created directory', today
    
    # 5. We use the zip command (in Unix/Linux) to put the files in a zip archive zip_command = "zip -qr '%s' %s" % (target, ' '.join(source))
    # Run the backup
        if os.system(zip_command) == 0:
            print 'Successful backup to', target else:
            print 'Backup FAILED'


os.sep变量的用法——这会根据你的操作系统给出目录分隔符,即在Linux、Unix下它 是'/',在Windows下它是'\\',而在Mac OS下它是':'。使用os.sep而非直接使用字符,会使我们的 程序具有移植性,可以在上述这些系统下工作。

属于一个对象或类的变量被称为域。

域有两种类型——属于每个实例/类的对象或属于类本身。它们分别被称为实例变量和类变 量。

域有两种类型——属于每个实例/类的对象或属于类本身。它们分别被称为实例变量和类变 量。

假如你有一个类称为MyClass和这个类的一个实例MyObject。当你调用这个对象的方法 MyObject.method(arg1, arg2)的时候,这会由Python自动转为MyClass.method(MyObject, arg1, arg2)——这就是self的原理了。这也意味着如果你有一个不需要参数的方法,你还是得给这个方法定义一个self参数。

##类：

    class Person:
        def sayHi(self):
            print 'Hello, how are you?'
    p = Person() 
    p.sayHi()

__init__方法在类的一个对象被建立时,马上运行。


类的变量由一个类的所有对象(实例)共享使用。只有一个类变量的拷贝,所以当某个对象 对类的变量做了改动的时候,这个改动会反映到所有其他的实例上。

对象的变量由类的每个对象/实例拥有。因此每个对象有自己对这个域的一份拷贝,即它们不是共享的,在同一个类的不同实例中,虽然对象的变量有相同的名称,但是是互不相关的。

    class Person:
        '''Represents a person.''' 
        population = 0
    
    def __init__(self, name):
        '''Initializes the person's data.''' self.name = name
        print '(Initializing %s)' % self.name
        # When this person is created, he/she 
        # adds to the population 
        Person.population += 1

    def __del__(self):
        '''I am dying.'''
        print '%s says bye.' % self.name
        Person.population -= 1
        if Person.population == 0: 
            print 'I am the last one.'
        else:
            print 'There are still %d people left.' % Person.population
        
    def sayHi(self):
        '''Greeting by the person.
        Really, that's all it does.'''
        print 'Hi, my name is %s.' % self.name
    
    def howMany(self):
        '''Prints the current population.''' 
        if Person.population == 1:
            print 'I am the only person here.' 
        else:
            print 'We have %d persons here.' % Person.population
        
    swaroop = Person('Swaroop') 
    swaroop.sayHi() 
    swaroop.howMany()
    
    kalam = Person('Abdul Kalam') 
    kalam.sayHi() 
    kalam.howMany()
    
    swaroop.sayHi() 
    swaroop.howMany()

    #输出
    $ python objvar.py (Initializing Swaroop)
    Hi, my name is Swaroop.
    I am the only person here. (Initializing Abdul Kalam) Hi, my name is Abdul Kalam. We have 2 persons here.
    Hi, my name is Swaroop.
    We have 2 persons here. Abdul Kalam says bye.
    There are still 1 people left. Swaroop says bye.
    I am the last one.

##私有变量

Python中默认的成员函数，成员变量都是公开的(public),而且python中没有类似public,private等关键词来修饰成员函数，成员变量。

在python中定义私有变量只需要在变量名或函数名前加上 ”__“两个下划线，那么这个函数或变量就会为私有的了。

在内部，python使用一种 name mangling 技术，将 __membername替换成 _classname__membername，所以你在外部使用原来的私有成员的名字时，会提示找不到。

命名混淆意在给出一个在类中定义“私有”实例变量和方法的简单途径， 避免派生类的实例变量定义产生问题，或者与外界代码中的变量搞混。 要注意的是混淆规则主要目的在于避免意外错误， 被认作为私有的变量仍然有可能被访问或修改。 在特定的场合它也是有用的，比如调试的时候， 这也是一直没有堵上这个漏洞的原因之一 （小漏洞：派生类和基类取相同的名字就可以使用基类的私有变量。）

"单下划线" 开始的成员变量叫做保护变量，意思是只有类对象和子类对象自己能访问到这些变量；记住这只是一个惯例,并不是Python所要求 的(与双下划线前缀不同)。
"双下划线" 开始的是私有成员，意思是只有类对象自己能访问，连子类对象也不能访问到这个数据。

_xxx    不能用'from module import *'导入
__xxx__ 系统定义名字

一个子类型在任何需要父类型的场合可以被替换成父类型,即对象可 以被视作是父类的实例,这种现象被称为多态现象。

##使用继承

    class SchoolMember:
        def __init__(self, name, age):
            self.name = name
            self.age = age
            print '(Initialized SchoolMember: %s)' % self.name
        def tell(self):
            print 'Name:"%s" Age:"%s"' % (self.name, self.age),
    
    class Teacher(SchoolMember):
        def __init__(self, name, age, salary):
            SchoolMember.__init__(self, name, age) 
            self.salary = salary
        print '(Initialized Teacher: %s)' % self.name
    
    def tell(self): 
        SchoolMember.tell(self)
        print 'Salary: "%d"' % self.salary
    
    class Student(SchoolMember):
        def __init__(self, name, age, marks):
            SchoolMember.__init__(self, name, age) 
            self.marks = marks
            print '(Initialized Student: %s)' % self.name
    
    def tell(self): 
        SchoolMember.tell(self)
        print 'Marks: "%d"' % self.marks
        
    t = Teacher('Mrs. Shrividya', 40, 30000) 
    s = Student('Swaroop', 22, 75)
    
    print  # prints a blank line
    
    members = [t, s]
    for member in members:
        member.tell() 

为了使用继承,我们把基本类的名称作为一个元组跟在定义类时的类名称之后。然后,我们注 意到基本类的__init__方法专门使用self变量调用,这样我们就可以初始化对象的基本类部分。 这一点十分重要——Python不会自动调用基本类的constructor,你得亲自专门调用它。

注意,在我们使用SchoolMember类的tell方法的时候,我们把Teacher和Student的实例仅仅作为 SchoolMember的实例。另外,在这个例子中,我们调用了子类型的tell方法,而不是SchoolMember类的tell方法。

一个术语的注释——如果在继承元组中列了一个以上的类,那么它就被称作 多重继承 。

使用文件
    
    poem = '''\
    Programming is fun
    When the work is done
    if you wanna make your work also fun:
    use Python! '''
    
    f = file('poem.txt', 'w')
    f.write(poem)
    f.close()

    f = file('poem.txt')
    while True:
        line = f.readline()
        if len(line) == 0:
            break
        print line,    #因为从文件读到的内容已经以换行符结尾,所以我们在print语句上使用逗号来消除自动 换行。
    f.close()


##os模块
这个模块包含普遍的操作系统功能。如果你希望你的程序能够与平台无关的话,这个模块是尤 为重要的。即它允许一个程序在编写后不需要任何改动,也不会发生任何问题,就可以在 Linux和Windows下运行。一个例子就是使用os.sep可以取代操作系统特定的路径分割符。

下面列出了一些在os模块中比较有用的部分。它们中的大多数都简单明了。

os.name字符串指示你正在使用的平台。比如对于Windows,它是'nt',而对于Linux/Unix 用户,它是'posix'。

os.getcwd()函数得到当前工作目录,即当前Python脚本工作的目录路径。

os.getenv()和os.putenv()函数分别用来读取和设置环境变量。

os.listdir()返回指定目录下的所有文件和目录名。

os.remove()函数用来删除一个文件。

os.system()函数用来运行shell命令。

os.linesep字符串给出当前平台使用的行终止符。例如,Windows使用'\r\n',Linux使
用'\n'而Mac使用'\r'。

os.path.split()函数返回一个路径的目录名和文件名。

    os.path.split('/home/swaroop/byte/code/poem.txt')
    ('/home/swaroop/byte/code', 'poem.txt')

os.path.isfile()和os.path.isdir()函数分别检验给出的路径是一个文件还是目录。

类似地,os.path.exists()函数用来检验给出的路径是否真地存在。

##特殊的方法：

![image](http://ww3.sinaimg.cn/large/68796df6jw1e7m5e68vjbj20pg0b3n06.jpg)

##列表综合

    listOne = [1,2,3,4,5,6,7,8,9,10]
    listTwo = [i*2 for i in listOne if i%2==0]
    #listTwo
    #返回[4, 8, 12, 16, 20]


##在函数中接收元组和列表

当要使函数接收元组或字典形式的参数的时候,有一种特殊的方法,它分别使用*和**前缀。 这种方法在函数需要获取可变数量的参数的时候特别有用。

由于在args变量前有*前缀,所有多余的函数参数都会作为一个元组存储在args中。如果使用的 是**前缀,多余的参数则会被认为是一个字典的键/值对。

    >>> def powersum(power, *args):
    ... '''Return the sum of each argument raised to specified power.'''
    ... total = 0
    ... for i in args:
    ... total += pow(i, power)
    ... return total
    ...
    >>> powersum(2, 3, 4) 
    25
    >>> powersum(2, 10) 
    100

##lambda

lambda作为一个表达式，定义了一个匿名函数。

    g = lambda x:x+1   #x为入口参数，x+1为函数体
    g(1)   #返回2
    g(2)   #返回3

    也可以这样用lambda x:x+1(1)

    #等同于
    def g(x):
        return x+1

lambda并不会带来程序效率的提高，只会使代码更简洁。它是为了减少单行函数的定义而存在的。 

##exec和eval语句

exec语句用来执行储存在字符串或文件中的Python语句。例如,我们可以在运行时生成一个包 含Python代码的字符串,然后使用exec语句执行这些语句。
    exec 'print "Hello World"' 
    #Hello World

eval语句用来计算存储在字符串中的有效Python表达式。
    eval('2*3') 
    #6

##repr函数

repr函数用来取得对象的规范字符串表示。反引号(也称转换符)可以完成相同的功能。注 意,在大多数时候有eval(repr(object)) == object。

    i = []
    i.append('item') >>> `i`
    #返回 "['item']"
    repr(i) 
    #返回 "['item']"

基本上,repr函数和反引号用来获取对象的可打印的表示形式。你可以通过定义类的__repr__ 方法来控制你的对象在被repr函数调用的时候返回的内容。
