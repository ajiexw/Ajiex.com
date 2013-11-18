Tags: Android

##Java4Android


诞生于战争。

大型主机时代（60年代）。

B语言（1970），用B语言编写的第一个Unix操作系统，c语言（1973）。

Sun公司（1983，斯坦福大学校园网，stanford university network）,java语言的缔造者，“没有竞争，就没有创新。没有创新，就没有一切”，最早认识到网络将改变生活，现在已经被oracle收购。
    
“上世纪最伟大的程序员”。

90年代初，java的前身出现，名为Oak（橡树）。

94年，Oak被命名为Java

95年，Sun正式对外界公开Java,并发布JDK1.0

98年，正式被更名为Java2，分化为3个方向:JAVA SE,EE,ME。

学会编程可以让你和计算机随意的沟通；编程是一种创造性的工作；学会编程会让上帝与你同在：“你觉得你不是生活在这个世界上，而是在创造这个世界”。

JDK：一组开发工具的打包,Java Development Kit。

源代码是人类可以理解的代码；

计算机无法理解源代码；

编译就是将源代码转换为计算机可以理解的代码。

运行程序：源文件编译后，会生成一个类文件；使用java命令运行类文件；观察运行结果。

oracle JDK:  /Library/Java/JavaVirtualMachines   #默认为空

Apple JDK:  /System/Library/Java/JavaVirtualMachines   #默认为1.6.0.jdk

JDK的默认安装目录：/System/Library/Frameworks/JavaVM.Framwork/，在这个目录下有个Version目录，里面有不同版本的JDK。(1.4，1.5，1.6，Current，A，CurrentJDK)

安装oracle的7.0版本，安装位置位于/Library/Java/JavaVirtualMachines,其他文件不变。

##JDK的基本组件

vac – 编译器，将源程序转成字节码

jar – 打包工具，将相关的类文件打包成一个文件[1]

javadoc – 文档生成器，从源码注释中提取文档

jdb – debugger，查错工具

java – 运行编译后的java程序（.class后缀的）

appletviewer：小程序浏览器，一种执行HTML文件上的Java小程序的Java浏览器。

Javah：产生可以调用Java过程的C过程，或建立能被Java程序调用的C过程的头文件。

Javap：Java反汇编器，显示编译类文件中的可访问功能和数据，同时显示字节代码含义。

Jconsole: Java进行系统调试和监控的工具


##Java的基本概念

环境变量：操作系统当中，用来指定操作系统运行时需要的一些参数；通常为一系列的键值对。

Path环境变量是操作系统外部命令搜索路径。

classpath环境变量是类文件搜索路径。解决"java Hello"里面Hello的路径问题，“在哪些路径下寻找这个Hello这个文件”。

JDK里面什么？

bin:java的所有命令都在此目录下；绝大多数都是可执行文件，如javac（编译），java；

include:放的是一些c语言的程序；

jre:java运行时环境；

lib:java运行的包文件；

src.zip:java JDK的部分源文件。

JRE:Java Runtime Environment，即Java运行环境，包括：java虚拟机；java平台核心类文件；其他支持文件。

JVM是Java Virtual Machine（Java虚拟机）的缩写，它是一个由软件虚拟出来的计算机。

程序执行步骤：程序员编写源文件 - 编译：检查错误 - 翻译为虚拟机可读取的字节代码 - 用户启动程序 - java虚拟机将class文件翻译成适合当前操作系统的代码 - 操作系统交给硬件去执行

一次编译，处处运行。

##什么是变量？

计算机是一种极度精确的机器；要将信息存储在计算机当中，就必须指明信息存储的位置和所需的内存空间；在java编程语言当中，使用声明语句来完成上述的任务。

变量的声明方法：变量类型 变量名 赋值号 变量值;
    
    int age = 20 ;  #这条语句使计算机分配足够的空间，用于存储一个整数，而这个整数的名字就叫做age。决定存储位置。
    
    int是java定义的一种数据类型，专门用于存储一定大小的整数。决定内存空间；

##变量类型

数据类型分为基本（原始）数据类型、引用数据类型。

基本数据类型包括数值型、字符型(char)、布尔型(boolean)。

数值型包括整数类型(byte,short,int,long)、浮点类型(float,double)。

引用数据类型包括类(class)、接口(interface)、数组。


##变量的命名规范

1、应该以字母、下划线、美元符号开头；
2、后面跟字母、下划线、美元符号、数字；
3、java变量名没有长度限制；
4、java变量名对大小写敏感；

boolean类型适用于逻辑运算，一般用于程序流程控制；

在java当中只有两种取值：true,false。注意不能用0和非0,或者空和非空来表示。

字符型char类型数据用来表示通常意义上的字符；

字符是由单引号括起来的单个字符；

java字符使用Unicode字符集；

Unicode为每种语言的每个字符设定了统一并且唯一的二进制码。

Unicode满足了跨语言文本转换和处理的需求；在互联网当中扮演着非常重要的角色；使用数字0-0x10FFFF来表示字符；最多允许有1114112个字符。

Unicode中一个中文字符和一个英文字符占的存储空间是一样的。

整数字面量为整型(int)

小数字面量为双精度浮点型(double)

不能把大的数据类型赋值给小的数据类型。

错误：int j = 0.5 * 10; #此处j为双精度浮点型。

四则运算计算出来的结果取决于表达式当中最大的数据类型。如j = 0.5 * 10 + 4 - 1为双精度浮点型。

强制类型转换：byte k = (byte)(b+0);

数值类型表数范围关系：byte < short < int < long < float < double

##运算符与表达式

包括算数、关系、布尔逻辑、位、赋值、扩展赋值、字符串连接运算符

int i = 3/2 #结果为1

float i = 3/2 #结果为1.0

double i = 3/2 #结果为1.0

double i = 3/2.0 #结果为1.5

double i = 3.0/2 #结果为1.5

逻辑运算符：逻辑非(!)，逻辑与(&)，逻辑或(|)，逻辑异或(^)，短路与(&&)，短路或(||)


逻辑与、短路与的区别：

逻辑与-前后两个表达式都会判断，即使前者为false;

短路与-前者为false时，后面的表达式就不会再运算。


逻辑或、短路或的区别：

逻辑或-前后两个表达式都会判断，即使前者为true;

短路或-前者为true时，后面的表达式就不会再运算。


逻辑异或：前后都为真或假时返回false；前后有一个为真一个为假时，返回true.

表达式是符合一定语法规则的运算符和操作符的序列：

i

10.5 + i

(i+j)-2

表达式的值：对表达式中操作数进行运算得到的结果称为表达式的值。
表达式的类型：表达式的值的数据类型即为表达式的类型。

##程序运行流程

分类：顺序结构、分支结构、循环结构

switch()括号里的数据类型只能是char类型、byte类型、短整型、整型四种。（浮点型不可以？）

##面向对象

面向对象是一种编程方法；是一种思维方式。

如何学习面向对象：掌握一门面向对象语言的语法；掌握面向对象的思维方式；熟悉面向对象设计原则；掌握面向对象设计模式。

面向对象的终极目标：消除应用程序当中的重复代码。


面向对象的思维方式：

首先确定谁来做，其次确定怎么做；

首先考虑整体，其次考虑局部；

首先考虑抽象，其次考虑具体。

类是抽象的，是具有一系列特征的某种事物。

构造函数没有“void”等返回类型。

构造函数也可以被重载。

构造函数是被new调用，不像函数那样被对象调用。

构造函数的主要目的是初始化成员变量。

    public class Person{
        String name;
        int age;

        Person(){
            
        }

        Person(String n, int a){
            name = n;
            age =a;
        }    
    }

    Person p1 = new Person("zhangsan", 20);


this代表调用这个函数的对象。

优化构造函数：

    public class Person{
        String name;
        int age;
        
        Person(){
            
        } 
        
        Person(String name, int age){   //阅读性更好
            this.name = name;
            this.age = age;
        }   
    }

构造函数2：

    public class Person{
        String name;
        int age;
        String adress;

        Person(){
            
        } 
        
        Person(String name, int age){
            this.name = name;
            this.age = age;
        }   

        Person(String name, int age, String adress){
            this.name = name;   //出现重复代码
            this.age = age;
            this.address = address;
        }   

    }

优化后：

    public class Person{
        String name;
        int age;
        String adress;

        Person(){
            
        } 
        
        Person(String name, int age){
            this.name = name;
            this.age = age;
        }   

        Person(String name, int age, String adress){
            this(name, age);   //调用上一个构造函数
            this.address = address;
        }   

    }

对this的调用必须是构造函数的第一个语句。

static静态成员变量

静态成员变量可以直接通过类名.成员变量名称来访问。

静态成员变量对该类的所有对象是共享的。任何一个对象修改了静态成员变量，其他对象的成员变量都会跟着改变。

静态函数也可以通过类名直接访问。但是无法使用非静态变量。

静态代码块：静态代码块自己会运行，在装载类的时候执行，主要为静态变量赋一些初始值。
    
    static{}

在面向对象的世界中，继承就是一个类得到了另外一个类当中的成员变量和成员方法。

java中只继承单继承，不允许多继承。

使用继承是为了减少重复代码。

对象的转型

向上转型——将子类的对象赋值给父类的引用：
    
    Sudent s = new Student();  //必须是父类和子类的关系
    Person p = s;

一个引用能够调用哪些成员变量和方法，取决于引用的类型。

一个引用调用的是哪一个方法，取决与这个引用所指向的对象。

    class Person{
        String name;
        int age;
        void introduce(){
            System.out.println("我的姓名是" + name + ",我的年龄是" + age);    
        }  
          
        void study(){
            
        }
    }

    class Student extends Person{
        String adress;
        void introduce(){
            System.out.println("我的姓名是" + name + ",我的年龄是" + age + ",我的地址是" + adress );    
        }    
    }

    class Test{
        Student s = new Student();
        Person p = s;
        System.out.println("我的姓名是"+p.name+",我的年龄是" +p.age+",我的地址是"+ p.adress); //此处编译错误，无法调用adress
        p.introduce();  //此处会调用Student的方法。   
        p.study();  //无法调用
    }

向下转型——将父类的对象赋值给子类的引用;

    Student s1 = new Student(); //向下转型的前提是向上转型
    Person p = s1;
    Student s2 = (Student)p;

    Person p = new Person();    //错误的向下转型，一个学生肯定是一个人，但一个人并不一定就是学生
    Student s = (Student)p;

抽象函数：只有函数的定义，没有函数体的函数被称为抽象函数。abstract void fun();只要有{}就是有函数体。

如果一个类里面有一个抽象函数，这个类也必须定义为抽象的。

抽象类不能够生成对象；

如果一个类当中没有抽象函数，这个类也可以被声明抽象类。

抽象类天生就是来当爹的，需要被继承。

继承抽象类的子类需要复写抽象函数为具体函数。然后Person.eat()就可以调用子类的eat方法了（多态？）

抽象类不能生成类；构造函数用于生成类的对象，那么抽象类可以有构造函数吗？

抽象类可以有构造函数：生成子类的时候，子类一定会先调用父类的构造函数:super().

    abstract class Person{
        String name;
        int age;
        Person(){}
        Person(String name, int age){
            this.name = name;
            this.age = age;    
        }
    }

    class Student extends Person{
        String adress;
        Student(){
            super();    
        }
        Student(String name, int age, String adress){
            super(name, age);
            this.adress = adress;    
        }
    }

##为什么要用抽象类？

声明成抽象函数会使编译器强制子类复写抽象函数。否则，声明成无内容但有函数体的函数（fun(){}），子类就算不复写，也不会报错。这样，在执行的时候就容易出错。

如果一段代码在语意上是有错误的，那么在语法上也是应该有错误的。语法上有错误更容易找出来。

如果一个类的函数，无发兼顾到所有的子类，那就把它定义为抽象函数，类定义位抽象函数。

设计模式中的模板模式就是以抽象类为基础的。

##包和访问权限

java当中的软件包：就是把类放在不同的文件夹下。避免不同的开发组出现同样名称的类冲突。

    package ajie;   //将类放置到一个包当中，需要使用package 包名;
    class Test{
        
    }
    
    打包命令：javac -d . Test.java  //编译时使用-d参数，该参数的作用是依照包名生成相应的文件夹

一个类被打包后，全名应该是"包名" + "." + "类名"。如ajie.Test

包名的命名规范：

1、要求包名所有的字母都要小写；

2、报名一般情况下，是你的域名倒过来写，如com.aoaola（会生成com文件夹及子文件夹aoaola）；

软件包为java类提供了命名空间。

java的访问权限：public, private, default, protected.

如果一个类被声明为public权限，类名和文件名必须相同。

调用不在一个包中的其他类，必须写包名。

在一个包的外部（其他包）访问该包的成员变量或成员函数，变量或函数必须声明为public的。

跨包访问需要public权限。

变量或函数声明为private时，只能在该类当中使用，同一个包的其他类也不能使用。

default：默认权限。在同一个包当中，可以任意使用其他类的变量或函数。不同包的话不可以使用。

public可以修饰类、成员变量、成员函数。没有任何限制，同一个包当中，或者不同包当中的类都可以自由访问。

private可以修饰成员变量和成员函数。只能在本类当中使用。继承的子类也无法使用。

default可以修饰类、成员变量和成员函数。不写权限修饰符，就是default权限。在同一个包当中，可以自由访问。

如果子类和父类不在同一个包当中，则子类无法继承到父类中default权限的变量和函数。（有问题）

如果子类和父类不在同一个包当中，子类可以继承到父类中default权限的变量和函数,但是由于权限不够，无法使用。

如果你希望一个类能够被跨包的继承，则应该将该类及该类的变量、函数声明为public的。

protected权限首先拥有和default一样的功能，但是该权限只能修饰成员变量和成员函数。同时，protetced允许跨包的继承。

同public的区别：public是跨包的所有类都可以访问，protected是只有跨包的子类可以访问。

public > prodected > default > private

一个类的权限应该尽可能的小。封装性。

定义了接口，就是定义了对象的标准。

接口使用interface定义；接口当中的方法都是抽象方法；接口当中的方法都是public权限。

    interface USB{
        void read();
        void write();    
    }

implements是特殊的继承extends.也就可以向上转型。

一个类可以实现多个接口；一个接口可以继承多个接口。

实现多个接口后，向上转型也就有多种选择。

    interface USB{
        void read();
        void write();    
    }

    interface WiFi{
        void open();
        void close();    
    }

    class Phone implements USB, WiFi{
        void read(){}
        void write(){}
        void open(){}
        void close(){}
    }

    class Test{
        Phone phone = new Phone;
        
        USB usb = phone;
        usb.read();
        usb.write();
        
        WiFi wifi = phone;
        wifi.open();
        wifi.close();       
    }

接口继承

    interface A{
        void funcA();    
    }

    interface B{
        void funcB();    
    }

    interface C extends A,B{
        void funcC();   //此时C接口里有3个方法
    }


##异常

异常：中断了正常指令流的事件。

异常是在程序运行的过程中出现的，不是编译期间出现的语法问题。

异常的分类：父类Throwable:包括Exception和Error两种。Error是java虚拟机自身出现问题，程序员无能为力。

Exception包括check exception及uncheck exception。RuntimeException属于uncheck exception。

java会强制要求check exception进行异常处理。
    
    //uncheck型异常
    class User{  //编译时可以通过
        
        private age;
        public void setAge(int age){
            if(age < 0){
                RuntimeException e = new RuntimeException("年龄不能为负数");
                throw e;    
            }    
            this.age = age;
        }  
    }

    //check型异常
    class User{    //编译时通不过，必须对异常进行捕获或声明
        
        private age;
        public void setAge(int age) throws Exception{//一旦用throws声明了异常，这个函数就可以不处理异常，而转交调用它的代码来处理
            if(age < 0){
                Exception e = new Exception("年龄不能为负数");
                throw e;    //抛出异常
            }    
            this.age = age;
        }  
    }

    class Test{
        public static void main(String [] args){
            User user = new User();
            try{
                user.setAge(-20);   
            }catch(Exception e){
                System.out.println(e);    
            }
        }    
    }

##java当中的输入输出系统io:input-output

i/o操作目标：从数据源当中读取数据，以及将数据写入到数据目的地当中。

（文件/键盘/网络） -> 输入 -> java程序 -> 输出 ->（文件/键盘/网络）

IO的分类：

输入流/输出流；字节流/字符流；节点流/处理流。

I/O当中的核心类（字节流）：InputStraem, OutputStream。这两个是所有的字节流的父类。且都是抽象类。

抽象类是不能生成对象的，要想生成对象需要使用子类。InputStream最常用的子类是FileInputStream，OutputStream最常用的子类是FileOutputStream。一个用来读，一个用来写。

字节流核心类的核心方法：

InputStream: int read(byte[] b, int off, int len)

OutputStream: void write(byte[] b, int off, int len)

    public class Test{
        public static void main(String [] args){
            try{
                FileInputStream fi = new FileInputStream("from.txt");
                FileOutputStream fo = new FileOutputStream("to.txt");

                byte[] buffer = new byte[100];
                int temp = fi.read(buffer, 0, buffer.length);
                fo.write(buffer, 0, temp);

            }catch(Exception e){
                System.out.println(e);
            }
        }
   }

读取大文件：

    import java.io.*;

    public class Test3{
        public static void main(String [] args){
            FileInputStream fi = null;
            FileOutputStream fo = null;

            try{
                fi = new FileInputStream("from.txt");
                fo = new FileOutputStream("to.txt");
                byte[] buffer = new byte[1024];

                while(true){
                    int temp = fi.read(buffer, 0, buffer.length);
                    if(temp == -1){
                        break;
                    }
                    fo.write(buffer, 0, temp);
                }

            }catch(Exception e){
                System.out.println(e);
            }finally{
                try{
                    fi.close();
                    fo.close();
                }catch(Exception e){
                    System.out.println(e);
                }
            }
        }
    }

字符流：读写文件时，以字符为基础
字符输入流：Reader <- FileReader， 方法：read(char[] c, int off, int len)
字符输出流：Writer <- FileWriter， 方法：write(char[] c, int off, int len)

    import java.io.*;

    public class TestChar{
        public static void main(String [] args){
            FileReader fr = null;
            FileWriter fw = null;

            try{
                fr = new FileReader("from2.txt");       
                fw = new FileWriter("too.txt");

                char [] ch = new char[100];
                int temp = fr.read(ch, 0, ch.length);
                fw.write(ch,0,temp);
            }catch(Exception e){
                System.out.println(e);
            }finally{
                try{
                    fr.close();
                    fw.close();
                }catch(Exception e){
                    System.out.println(e);
                }
            }
        }
    }

节点流/处理流

    import java.io.*;

    public class TestBuffer{
        public static void main(String [] args){
            FileReader fileReader = null;
            BufferedReader bufferedReader = null;

            try{
                fileReader = new FileReader("users.txt");
                bufferedReader = new BufferedReader(fileReader);
                while(true){
                    String line = bufferedReader.readLine();
                    if(line == null){
                        break;
                    }
                    System.out.println(line);
                }
            }catch(Exception e){
                System.out.println(e);
            }finally{
                try{
                    bufferedReader.close();
                    fileReader.close();
                }catch(Exception e){
                    System.out.println(e);
                }
            }
        }
    }

装饰者模式：在不必改变原类文件和使用继承的情况下，动态的扩展一个对象的功能。

特点：

装饰对象和真实对象有相同的接口，这样客户端对象就可以以和真实对象相同的方式和装饰对象交互；

装饰对象包含一个真实对象的引用；

装饰对象接受所有来自客户端的请求，它把这些请求转发给真实对象;

装饰对象可以在转发这些请求以前或以后增加一些附加功能。这样就确保了在运行时，不用修改给定对象的结构就可以在外部增加附加的功能。在面向对象的设计中，通常是通过继承类实现对给定类的功能扩展。

    interface Worker{
        public void doSomeWork();    
    }

    class Plumber implements Worker{
        public void doSomeWork(){
            System.out.println("修水管");        
        }
    }
    
    class Carpenter implements Worker{
        public void doSomeWork(){
            System.out.println("修家具");    
        }
    }

    class AWorker implements Worker{
        private Worker worker;
        
        public AWorker(Worker worker){
            this.worker = worker;
        }

        public void doSomeWork(){
            System.out.println("你好");  
            worker.doSomeWork();
        }
    }

    class Test{
        public static void main(String [] args){
            Plumber plumber = new Plumber();
            AWorker aWorker1 = new AWorker(plumber);
            aWorker1.doSomwWork();

            CarPenter carpenter = new Carpenter();
            AWorker aWorker2 = new AWorker(carpenter);
            aWorker2.doSomeWork();
        }    
    }

装饰这模式：动态地将责任附加到对象上，若要扩展功能，装饰者提供了比继承更有弹性的替代方案。

例子解析：

工人下面有木匠与水工，然后又有A公司的木匠，B公司的木匠，A公司的水工，B公司的水工。

如果完全用继承实现的话，需要3层继承：interface Worker, class Plumber implements Worker, class A-Plumber extends Plumber。如果再出现新的工种，出现n个公司，会让程序代码变得复杂。

为了避免过多的继承，改用装饰者模式实现，用Aworker包装Plumber，给Plumber增加额外的功能：interface Worker, class Plumber implements Worker, class Aworker implements Plummber.

##内部类

    class A{   //编译后生成A.class及A$B.class两个文件
        class B{
            
        }
    }

    class Test{
        public static void main(String [] args){
            A a = new A();
            A.B b = new A().new() B;        
        }    
    }

##匿名内部类

    interface A{
        public void doSth();
    }

    class B{
        public void fun(A a){
            System.out.println("B类的func函数");
            a.doSth();    
        }
    }

    class Aimpl implements A{
        public void doSth(){
            System.out.println("A类的func函数");
        }
    }

    class Test{
        public static void main(String [] args){
            //Aimpl al = new Aimpl();
            //A a = al;
            
            B b = new B();
            //b.fun(a);    
            b.fun(new A(){  //A为接口，不能实例化；此处的"new 接口(){}"是实现A接口，生成一个匿名内部类。同Aimpl
                public void doSth(){
                    System.out.println("匿名内部类");
                }
            }
        }    
    }


##线程

进程和线程：

多进程：在操作系统当中能（同时）运行多个任务（程序）

多线程：在同一应用程序中有多个顺序流（同时）执行

线程就是进程当中一个程序的执行流程。

安卓就是多进程，一个程序一个进程。

创建线程的方法：

方式1：

定义一个线程类，它继承类Thread并重写其中的方法run()，方法run()成为线程体；

    class FirstThread extends Thread{
        public void run(){
            for(int i=0;i<100;i++){
                System.out.println("FirstThread-->" + i);
            }        
        }
    }

    class TestThread{
        public static void main(String [] args){
            
            //生成线程类的对象
            FirstThread ft = new FirstThread();
            
            //启动线程,通过ft.run()不行,不是线程。此时这里有3个线程：主函数线程、当前ft线程、垃圾回收的线程，线程交替运行
            ft.start();    //线程不是马上执行，而是在这里进入就绪状态

            for(int i=0;i<100;i++){
                System.out.println("main主函数-->" + i);
            }        

            
        }
    }


方式2：

提供一个实现接口Runnable的类作为线程的目标对象，在初始化一个Thread类或者Thread子类的线程对象时，把目标对象传递给这个线程实例，由该目标对象提供线程体。

    class RunnableImpl implements Runnable{
        public void run(){
            for(int i=0;i<100;i++){
                System.out.println("Runnable-->" + i);
                if(i==50){
                    try{
                        Thread.sleep(2000);
                    }catch(Exception e){
                        System.out.println(e);
                    }   
                }
            }        
        }
    }

    class TestRunnable{
        public static void main(String [] args){
            
            //生成一个Runnable接口实现类的对象
            RunnableImpl ri = new RunnableImpl();

            //生成一个Thread对象，并将Runnable接口实现类的对象作为参数传递给Thread对象
            Thread t = new Thread(ri);
            
            //线程的优先级最大是10，最小是1,可以使用Thread所提供的静态常量来设置线程的优先级
            //优先级越大，先执行的“概率”越大
            //t.setPriority(Thread.MAX_PRIORITY);
            //t.setPriority(Thread.MIN_PRIORITY);
            //System.out.println(t.getPriority());

            //通知Thread对象，执行start方法
            t.start(); 

            for(int i=0;i<100;i++){
                System.out.println("main主函数-->" + i);
            }        

            
        }
    }

线程的简单控制方法

中断线程：Thread.sleep(), Thread.yield()

设置线程的优先级：getPriority(), setPriority()

线程休眠结束后，不是马上进入运行状态，而是进入就绪状态。还会继续和其他线程抢占cpu进程资源。

Thread.yield():抢占cpu时，遇到此代码，自动让出cpu。但是不代表不抢了，还是会抢。

多线程共用一个线程时，会出现同步线程的错误。

解决方案：同步锁

    class MyThread implements Runnable{
        int i = 100;
        public void run(){
            while(true){
                
                //同步锁
                synchronized(this){

                    //静态方法Thread.currentThread()
                    System.out.println(Thread.currentThread().getName() + i);
                    i--;

                    //静态方法
                    Thread.yield();
                    if(i<0){
                        break;
                    }
                }
            }
        }
    }

    class TestMyThread{
        public static void main(String [] args){
            MyThread mt = new MyThread();
            
            //生成两个Thread对象，但是这两个Thread对象共用一个线程体
            Thread t1 = new Thread(mt);
            Thread t2 = new Thread(mt);

            //每一个线程都有名字，可以通过Thread对象的setName()方法设置线程名字;
            //也可以使用getName()方法获取线程的名字；
            t1.setName("线程a");
            t2.setName("线程b");

            //分别启动两个线程，两个线程交替运行，同时执行run代码
            t1.start();
            t2.start();

        }
    }

##数组

    //数组的静态定义
    int[] arr = {5,2,7,9,0};
    
    //数组的动态定义
    int[] arr = new int[10];

    //二维数组的静态定义方法
    int [][] arr = {{1,2,3},{4,5,6},{7,8}};

    //二维数组的动态定义方法
    int [][] arr = new int[3][5];

数组一旦被声明，数组的长度就确定下来，不能再追加。

##类集框架

类集是一组类和接口；位于java.util包当中；主要用于存储和管理对象；主要分为3大类：集合、列表和映射。

Set-集合：集合中的对象不按特定的方式排序，并且没有重复对象；

List-列表：集合中对象按照索引位置排序，可以有重复的对象；

Map-映射：集合中的每一个元素包含一个键对象和一个值对象，键不可以重复，值可以重复。

    import java.util.List;
    import java.util.ArrayList;

    public class TestList{
        public static void main(String [] args){
            ArrayList<String> arrayList = new ArrayList<String>();
            
            arrayList.add("a");
            arrayList.add("b");

            String s = arrayList.get(1);
            System.out.println(s);
        }
    }


Collection接口基本方法：

boolean add(Object o)：向集合当中加入一个对象;

void clear()：删除集合当中的所有对象；

boolean isEmpty()：判断集合是否为空；

remove(Object o):从集合当中删除一个对象的引用；

int size()：返回集合中元素的数目

set接口和List的接口都是Collection类型的接口。

Iterator（迭代器） <-- Collection <-- Set <--HashSet

Iterator（迭代器） <-- Collection <-- List <--ArrayList

Iterator的方法：hasNext(); next();

迭代器第一个元素前面有一个游标。

    import java.util.Set;
    import java.util.HashSet;
    import java.util.Iterator;

    public class TestSet{
        public static void main(String [] args){
            HashSet<String> hashSet = new HashSet<String>();
            
            Set<String> set = hashSet;  //向上转型为SET类型，很多常用数据为Set类型，要是用Set的方法
            set.add("a");
            set.add("b");
            set.add("c");
            set.add("b");  //集合不允许重复，获取长度仍然会返回3

            int i = set.size();
            System.out.println(i);
            
            //调用Set对象的iterator方法，会生成一个迭代器对象，该对象用于遍历整个set
            Iterator<String> it = set.iterator();
            while(it.hasNext()){
                String s = it.next();
                System.out.println(s);
            }

            //set.clear();   //清空集合
            //set.remove("a");  //删除元素
            //boolean b1= set.isEmpty();

            //for(String item:set){
              //  System.out.println(item);   //返回b,c,a
            //}
        }
    }

Map是一个独立的接口。

    import java.util.Map;
    import java.util.HashMap;

    public class TestMap{
        public static void main(String [] args){
            HashMap<String,String> hashMap = new HashMap<String,String>();
            Map<String,String> map = hashMap;

            map.put("1","a");
            map.put("2","b");
            map.put("3","c");
            map.put("4","d");
            map.put("3","e");  同上面重复，会覆盖掉上面的值，长度仍然为4.

            int i = map.size();
            System.out.println(i);

            String s = map.get("3");
            System.out.println(s);
        }
    }

equals函数属于Object这个类，所有的类都是Object的子类，所以都会有这个函数。

"=="判断两个引用是否指向同一个引用。equals()用来确定两个对象的内容是否相同。

对象的内容相等需要符合两个条件：

对象的类型相同（可以使用instanceof操作符进行比较）；

两个对象的成员变量的值完全相同。
    
    boolean b = u1.equals(u2);

    //自己写的类需要复写equals函数
    class User{
        String name;
        int age;

        public boolean equals(Object obj){
            if(this == obj){
                return true;    
            }        

            boolean b = obj instanceof User;
            if(b){
                User u = (User)obj;
                if(this.age == obj.age && this.name.equals(u.name)){   //这里的equals是String类型的equals，不是当前类的equals。
                    return true; 
                }
            }else{
                return false;    
            }
        }
    }


hashCode(), toString()这俩函数也属于Object这个类。

什么是hash算法：任意长度数据，经过hash算法，变成一个唯一的散列值。

如果两个对象经过equals对比是相等的，则它们的hash码也应该是相等的。

    User u = new User("zhangsan", 20);
    HashMap<User, String> map = new HashMap<User, String>();
    map.put(u, "abc");

    String s = map.get(new User("zhangsan", 20));
    System.out.println(s); 

    //默认时，并不会输出"abc"。
    //因为map.get()里的参数对比的是hash值，默认时，不是相同引用的hash值不相同。
    //但是逻辑上应该，所以需要复写hashcode()函数。

    class User{ //主要是集合用到hashCode()方法
        String name;
        int age;

        public int hashCode(){
            int result = 17;
            result = 31 * result + age;
            result = 31 * result + name.hashCode();
            return result;
        }
    }

toString()

    User u = new User();
    System.put.println(u);  //这里会打印出什么？实际会调用u.toString方法

    class User{
        String name;
        int age;

        public String toString(){
            String result = "age:" + age + ",name:" + name;
            return result;
        }
    }

Eclipse工具

快捷键：

Alt+/：代码助手，自动补全

Ctrl+d：删除整行代码

Ctrl+x：剪切

Ctrl+z：还原

Ctrl+/：生成注释

Ctrl+shift+/: 生成*注释

Ctrl+shift+\:取消*注释

Ctrl+alt+上下光标：向上或向下复制一行

重构可以改善软件的设计；重构可以让软件更加容易理解；重构可以协助寻找bug；重构可以提升开发速度。


