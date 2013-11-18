Tags: Android

我们可以编写些程序、编译、运行。

优点：友好的语法、面向对象、内存管理、跨平台可移植。/write-once,run-anywhere

Java的工作方式：编写源代码-编译器检查错误并翻译成字节码-java虚拟机读取并执行字节码，转换成平台可以理解的形式

类存于源文件里面，方法存于类中，语句存于方法中。

类是对象的蓝图，java中的绝大多数都是对象。

错误语法：

    int x = 1;
    while(x){}

java中的整型与boolean两种类型并不相容，只能用如下的boolean变量来测试：
    
    boolean isHot= true;
    while(isHot){}

类不是对象，是对象的蓝图。是用来创建对象的模型。会告诉虚拟机如何创建某种类型的对象。
对象是靠类的模型塑造出来的。

main的两种用途：测试真正的类；启动你的java应用程序。

java的程序在执行期是一组会互相交谈的对象。

java程序是由一组类组成，其中一个类会带有启动用的main方法。

对象引用变量保存的是存取对象的方法。

Dog d = new Dog() #代表取得Dog对象的方法以字节形式放进变量中。对象本身并没有放进变量中。

任意一个java虚拟机，所有的引用大小都一样。但不同的虚拟机可能会以不同的方式来表示引用，因此某个java虚拟机的引用可能会大于或小于另外一个虚拟机的引用。

java不是c。不可以对引用变量进行运算。

类所描述的是对象知道什么与执行什么。在编写类时，你是在描述java虚拟机应该如何制作该类型的对象。

封装会对实例变量加上绝对领域。

封装：强迫其他程序一定得经过setter，如此setter就能检查参数并判断是否可以执行。

实例变量与局部变量的区别：

实例变量声明在类内，局部变量声明在方法中，局部变量在使用前必须被初始化。

实例变量永远都会有默认值。int类型的变量默认值为0,float类型为0.0，booleans为false，对象引用默认值为null;

局部变量没有默认值，如果在变量被初始前就要使用的话，编译器会显示错误。

变量的比较：

使用"=="来比较两个主数据类型，或者判断两个引用是否引用同一个对象。

使用equals()来判断两个对象是否在意义上相等。（比如两个string对象是否带有相同的字节组合）。

"=="只用来比对两个变量的字节组合，实质所表示的意义则不重要。字节组合要么相等，要么就是不相等。如：
    
    int a = 3; 
    byte b =3;
    if(a == b){}  //由于字节组合相等，此处为true

实例变量应该要标记为private，并通过getter与setter来存取。如此才能有机会确保实例变量值会落在合法的范围内。

同for循环相比，while循环只有boolean测试，没有内建的初始化和重复表达式。

while适合用在不知道要循环几次的循环上，若知道执行几次，则使用for循环比较容易阅读。

    int i = 0;  //初始化
    while(i<8){
        System.out.println(i); 
        i++;   //计数器递增
    }

加强版的for循环

    for(String name:nameArray){}

将String转换成int
    
    Integer.parseInt(string)

"System.out.println(++y);"此处标准输出内部对y进行的运算也会对当前所在语句块当中的y产生影响，效果等同于"++y;"

数组无法改变大小。对象有状态和行为，无法对数组调用方法，比如不能从数组中删除元素，

ArrayList是个Java API的类，是个高级类，是个对象，可以确实地把引用删除掉，也可以动态的改变大小。在运用基本数据类型的时候，数组会比ArrayList快。

在使用ArrayList时，你只是在运用ArrayList类型的对象，因此跟运用其他对象一样，你会使用"."运算符调用它的方法。

使用数组时，你会用特殊的数组语法来操作，这样的语法只能用在数组上。虽然数组也是对象，但是它有自己的规则，你无法调用它的方法，最多只能存它的Length实例变量。

虽然ArrayList只能携带对象而不是primitive主数据类型，但编译器能够自动地将主数据类型包装成Object以存放在ArrayList中。

##ArrayList与一般数组的比较

创建：
    
    ArrayList<String> mylist = new ArrayList<String>();    //不需指定大小，会在加入或删除元素时自动调整大小
    String[] mylist = new String[3];    //创建时就必须确定大小

添加元素：

    String s1 = new String("string"); mylist.add(s1);   //可以使用add(int,Object)指定索引值，也可以让它自行管理
    String s1 = new String("string"); mylist[0] = s1;   //存放对象给一般数组时必须指定位置

获取个数：

    int theSize = mylist.size();
    int theSize = mylist.length;

根据位置获取元素：

    Object o = mylist.get(1);
    String o = mylist[1];   //方括号是只用在数组上的特殊语法

删除元素：

    mylist.remove(1); mylist.remove(s1);
    mylist[1] = null;

判断元素是否在集合中：

    boolean isIn = mylist.contains(s1);
    //数组：
    boolean isIn = false;
    for(String item : mylist){
        if(s1.equals(item)){
            isIn = true;
            break;
        }
    }


短运算符：&&

避免调用内容为null:if(refVar != null && refVar.isValidType()){},前者为假是后面就不执行了。

&与|运算符使用在boolean表达式时，会强制虚拟机一定要计算运算符两边的算式。但这两个运算符通常是用来做位的运算。

在java的api中，类是被包装在包中的。要使用api中的类，你必须知道它被放在哪个包中。

以javax开头的包以前曾经是扩展，后来才被包含进标准库。

类会用包来组织。类有完整的名称，那是由包的名称与类的名称所组成的。ArrayList事实上叫做java.util.ArrayList。

除了java.lang之外，使用到其他包的类都需要指定全名。也可以在原始程序代码的最开始部分下import指令来说明所使用到的包。

java.lang是个预先被引用的基础包。所以不必import进java.lang.System,java.lang.String等class。

import与c语言的include并不相同，运用import只是帮你省下每个类前面的包名称而已。程序不会因为用了import而变大或变慢。

继承：实例变量无法被覆盖掉是因为不需要，它们并没有定义特殊的行为。

多态：

    Animal[] animals = new Animal[3];
    animals[0] = new Dog();
    animals[1] = new Cat();
    animals[2] = new Wolf();
    for(int i=0;i<animals.length;i++){
        animals[i].eat();   //当i为0的时候会调用Dog的eat，当i为1的时候会调用Cat的eat。        
    }

java中除了内部类之外，没有私有类这样的概念。有3种方法可以防止某个类被作出子类。

第一类是存取控制。就算类不能被标记为私有，但它还是可以不标记为公有。非公有的类只能被同一个包的类作出子类。

第二类是使用final这个修饰符，这表示它是继承树的末端，不能被继承。（如果需要安全，确保方法都会是你写出的版本，此时就需要final）

第三类是让类只拥有private的构造程序。

如果想要防止特定的方法被覆盖，可以将该方法标识上final这个修饰符。将整个类标识成final表示没有任何的方法可以被覆盖。

子类的参数同父类的必须一样，返回类型必须要兼容（返回一样的类型或该类型的子集）。

子类的存取权限必须跟父类相同，或者更为开放。比如：不能覆盖一个公有的方法并将它标记为私有。

##方法的重载

重载的意义是两个方法的名称相同，但参数不同。所以，重载与多态、与继承毫无关系。

重载可以有同一方法的多个不同参数版本以方便调用。

##接口

接口是一种100%的抽象类，也就是无法初始化的类。接口是多态和java的重点。

抽象类：不能被"new"出来的类，不能被初始化，不能创建任何类型的实例。

但是还可以用这种抽象的类型作为引用类型，给多态使用。

设计抽象类的方法：在类的声明前加上抽象类的关键词：abstract。

不是抽象的类就被称为具体类。查阅Java API你会发现其中有很多的抽象类，特别是GUI的函数库中更多。GUI的组件类是按钮、滚动条等类的父类，你只会对组件下的具体子类作初始化动作。

除了类之外，也可以将方法标记为abstract。抽象的类代表此类必须要被extend过，抽象的方法代表此方法一定要被覆盖过。

抽象类中的某些行为（方法）无法兼顾到所有的子类，所以会在各子类中单独定义。（此处可以用到抽象方法）

抽象的方法没有实体。（因为你已经知道编写出抽象方法的程序代码没有意义，所以不会含有方法）

public abstract void eat();    //没有方法体，直接以分号结束。

如果你声明出一个抽象的方法，就必须将类也标记为抽象的。你不能在非抽象类中拥有抽象方法。就算只有一个抽象的方法，此类也必须标记为抽象的。

抽象方法无法实现方法的内容，意义在于定义出一组子型共同的协议。

抽象的好处：多态！想达成的目标是要使用父型作为方法的参数、返回类型或数组的类型。通过这个机制你可以加入新的子型到程序中，却又不必重写或修改处理这些类型的程序。因此多态的好处就在于所有子型都会有那些抽象的方法。

抽象的方法没有内容，它只是为了标记出多态而存在。这表示在继承树结构下的第一个具体类必须要实现出所有的抽象方法。java很注重你的具体子类有没有实现这些方法。

必须实现所有的抽象的方法。

java中的所有类都是从Object(java.lang.Object)这个类继承出来的。

任何从ArrayList<Object>取出的东西都会被当作Object类型的引用而不管它原来是什么。

编译器是根据引用类型来判断有哪些Method可以调用，而不是根据Object确实的类型。

不管实际上所引用的对象是什么类型，只有在引用变量的类型就是带有某方法的类型时才能调用该方法。

Object引用变量在没有类型转换的情况下不能赋值给其他的类型，若堆上的对象类型与所要转换的类型不兼容，则此转换会在执行期间产生异常。

类型转换的例子：Dog d = (Dog) x.getObject(aDog);

java不允许多重继承，因为那样会有致命方块的问题。



接口的定义：
    
    public interface Pet{
        public abstract void beFriendly();  //接口的方法带有public和abstract的意义，这两个修饰符属于选择性的。
        public abstract void play();    //接口的方法没有内容
    }

接口的实现：

    public class Dog extends Canine implements Pet{
        public void beFriendly(){...}   //必须在这里实现出Pet所有的方法
        public void play(){...}
    }

不同继承树的类也可以实现相同的接口。

类可以实现多个接口（extend只能有一个，implement可以有好多个；类来自单亲家庭，但可以扮演多重角色）

    public class Dog extends Animal implements Pet, Saveable, Paintable{...}

当需要定义一群子类的模板，又不想让程序员初始化此模板时，设计出抽象的类给他们用。

如果想要定义出类可以扮演的角色，使用接口。

在子类中指定下面的命令，父类的方法就会执行：

    super.xxxxx();

##构造器与垃圾收集器

栈与堆：生存空间

在java中，程序员会在乎内存中的两种区域：对象的生存空间堆，和方法调用及局部变量的生存空间栈。

当java虚拟机启动时，它会从底层的操作系统取得一块内存，并以此区段来执行java程序。

所有的对象都存活于可垃圾回收的堆上。

实例变量与局部变量

实例变量被声明在类而不是方法里面。它们代表每个独立对象的“字段”（每个实例都有不同的值）。实例变量存在于所属的对象中。

局部变量和参数都是被声明在方法中。它们是暂时的，且生命周期只限于方法被放在栈上的这段期间（也就是方法调用至执行完毕为止）。

不论对象是否声明或创建，如果局部变量是个对该对象的引用，只有变量本身会放在栈上，对象本身只会存在于堆上。

实例变量存在于对象所属的堆空间上。java创建对象所需的空间是足以存放该对象所有实例变量的空间。

Duck myDuck = new Duck()

new Duck()看来像是在调用Duck()方法，并不是，我们是在调用Duck的构造函数。

构造函数带有你在初始化对象时执行的程序代码。也就是新建一个对象时就会被执行。就算你没有自己写构造函数，编译器也会帮你写一个。

唯一能够调用构造函数的办法就是新建一个类。

编译器自动写出来的构造函数：Public Duck(){}，名称一定要与类的名称相同。

同方法的区别：方法有返回类型，构造函数没有。

构造函数的一项关键特征是它会在对象能够被赋值给引用之前就执行。这代表你有机会在对象被使用之前就介入new的过程。

大部分的人都是使用构造函数来初始化对象的状态，也就是设置和给对象的实例变量赋值。

例如你需要通过用户的输入来完成对象的创建。

java有可以与类同名的方法而不会变成构造函数，其中的差别在于是否有返回类型。构造函数不会有返回类型。

构造函数不会被继承。

一定要有不需要参数的构造函数。

如果一个类有一个以上的构造函数，这代表它们也是重载的。

如果你已经写了一个有参数的构造函数，并且你需要一个没有参数的构造函数，你必须自己手动写。

如果类有一个以上的构造函数，则参数一定要不一样。

编译器看的是参数的类型和顺序，而不是参数的名字。你可以做出相同类型但是顺序不同的参数。

构造函数可以是公有、私有或不指定的。

在创建新对象时，所有继承下来的构造函数都会执行。

每个类至少都会有一个构造函数，包括抽象的类。

虽然不能对抽象的类执行new操作，但抽象的类还是父类，因此它的构造函数会在具体子类创建出实例时执行。

构造函数在执行的时候，第一件事是去执行它的父类的构造函数，这会连锁反应到Object这个类为止。

如果你没有编写构造函数，编译器会帮我们加上，并且加上super()对父类构造函数的调用。

    public ClassName(){super();}

如果你有构造函数但没有调用super();编译器会帮你对每个重载版本的构造函数加上调用super()；编译器加上的一定会是没有参数的版本，假使父类有多个重载版本，也只有无参数的这个版本会被调用到。

对super()的调用必须是构造函数的第一个调用。

从某个构造函数调用重载版的另一个构造函数：使用this()，this就是个对对象本身的引用。

this()只能用在构造函数中，且必须是第一行语句。

super()与this()不能同时调用。

局部变量只会存活在声明该变量的方法中。

只要变量的堆栈还存在与堆栈上，局部变量就算活着。方法执行完毕，局部变量即消失。

局部变量的寿命与对象相同。

除非有对对象的引用，否则该对象一点意义也没有。当最后一个引用消失时，对象就变成可回收的。

3种方法释放对象的引用：

    void go(){  //z会在方法结束时消失
        Life z = new Life();    
    }

    Life z = new Life();    //第一个对象会在z被赋值到别处时挂掉
    z = new Life();

    Life z = new Life();    //第一个对象会在z被赋值为null是击毙
    z = null;

对象的状态：保持在实例变量中的值。

##数字与静态

在Math这个类中的所有方法都不需要实例变量值，因为这些方法都是静态的。所以你无需Math的实例。你会用到的只有它的类本身。因此也不会有空间被它的实例所占用。你不需创建math的实例，实际上也无法创建。

java是面向对象的，但若处于某种特殊的情况下，通常是实用方法，则不需要类的实例。

static这个关键词可以标记出不需类实例的方法。一个静态的方法代表说“一种不依靠实例变量也就不需要对象的行为。”

静态与非静态区别：

    Math.min(88,86)     //以类的名称调用静态的方法

    Song t2 = new Song();   //以引用变量的名称调用非静态的方法
    t2.paly();


带有静态方法的类通常不打算要被初始化（虽然不一定是这样）

Math让构造函数标记为私有，防止被初始化，所以你无法创建Math的实例。

任何非静态的方法都代表必须以某种实例来操作。

静态的方法（比如main）不能调用非静态的变量。

    public class Duck{
        private int size;
        public static void main(String[] args){
            System.out.println(size);   //此处会报错   
        }
    }

静态方法无法看到实例变量的状态。

静态的方法也不能调用非静态的方法。

用引用变量代替类名称-调用静态方法的程序代码：合法但不是好事，会产生容易误解的程序代码：

    Duck d = new Duck();   //虽然合法，但编译器还是会解析出原来的类
    String[] s = { };      //使用d来调用main()并不代表main会知道是哪个对象引用所做的调用，如此调用的方法也还是静态的
    d.main(s);

静态变量：它的值对多有的实例来说都相同（被同类的所有实例共享的变量）

假设要在执行过程中计算有多少Duck的实例已经被创建，要怎么做？

    //错误方案
    class Duck{
        int duckCount = 0;  //此处的duckCount是个实例变量，每个Duck在初始化的时候该值都为0
        public Duck(){
            duckCount++;
        }
    }


    //正确方案，所有的Duck实例只有一份duckCount变量
    class Duck{
        private static int duckCount = 0;   //此处的duckCount变量只会在类第一次载入的时候被初始化
        public Duck(){
            duckCount++;
        }    
    }

静态变量是共享的。同一类所有的实例共享一份静态变量。

实例变量：每个实例一个;

静态变量：每个类一个。

静态变量会在该类的任何静态方法执行之前就初始化。

静态变量会在该类的任何对象创建之前就完成初始化。

一个被标记为final的变量代表它一旦被初始化之后就不会改动，一直会维持原值。

    public static final double PI = 3.1415926
    //变量被标记为public，因此可供各方读取；
    //变量被标记为static，所以不需要Math的实例;
    //变量被标记为final，因为圆周率是不变的

静态final变量的初始化，两种方法：（如果没有以这两种方式之一来给值的话，编译器会报错）

    public class Foo{
        public static final int FOO_X = 25;    
    }

    //或者在静态化初始程序中：
    public class Bar{
        public static final double BAR_SIGN;
        static{   //这段程序会在类被加载时执行
            BAR_SIGN = (double) Math.random();
        }
    }

静态化初始程序(static initializer)是一段在加载类时会执行的程序代码，它会在其他程序可以使用该类之前就执行，所以很适合放静态final变量的起始程序。

final不只用在静态变量上....

也可以用final关键字来修饰非静态的变量，包括实例变量、局部变量、方法的参数————都代表它的值不能被变动。

也可以用final来防止方法的覆盖或创建子类。

    class Foo{
        final int size = 3;     //size将无法改变

        void doSth(final int x){    //x不能被改变
            
        }

        void doMore(){
            final int z = 7;    //不能改变z    
        }

        final void Poof(){  //此方法不能被覆盖
            
        }
    }


    final class ClassName{  //该类不能被继承（也就是创建它的子类）
        
    }

将类标记为final，主要目的是为了安全。例如String这个类，假使有人继承过，弄了一个行为很不一致的版本，预期操作String的程序产生很多问题。

如果类已经是final的，再标记final的方法就很多余了。

静态的方法应该用类的名称来调用，而不是用对象引用变量。

静态的方法可以直接调用，而不需堆上的实例。

静态的方法是一个非常实用的方法，它不需要特别的实例变量值。

静态的方法不能存取非静态的方法。

如果类只有静态的方法，你可以将构造函数标记为private的，以避免被初始化。

在java中的常量是把变量同时标记为static和final的。

final的静态变量值必须在声明或静态初始化程序中赋值。
e
##primitive主数据类型的包装

当你需要以对象方式来处理primitive主数据类型时，就把它包装起来。java5.0之前的版本必须要这么做。

有时你会想把primitive主数据类型当作对象来处理。在5.0之前的java版本上，你无法直接把主数据类型放进ArrayList或Hasg-Map中：

    int x = 32;
    ArrayList list = ArrayList();
    list.add(x);    //除非是5.0以上的版本，否则这个命令不会成功

每一个primitive主数据类型都有个包装用的类，且因为这些类都在java.lang这个包中，所以无需去import它们。

Boolean, Character, Byte, Short, Integer, Long, Float, Double.

    int i = 32;
    Integer iWrap = new Integer(i);     //包装值，传入primitive主数据类型给包装类的构造函数

    int unWrapped = iWrap.intValue();   //解开包装，所有的包装类都有类似这样的方法

Integer与int两者间毫无关系，只是Integer带有一个int类型的实例变量（以保存Integer所包装的primitiv)

int的ArrayList
    
    public void doSth(){
        ArrayList list = new ArrayList();
        list.add(new Integer(3));   //不能直接加入primitive的3，得先转换成Integer
        Integer one = (Integer) list.get(0);  返回Object类型，将Object类型转换成Integer
        int intOne = one.intValue();  
    }

从5.0版开始加入的autoboxing功能能够自主地将primitive主数据类型转换成包装过的对象。

int的ArrayList（有autoboxing)

    public void doSth(){
        Arraylist<Integer> list = new ArrayList<Integer>();    //不能直接声明ArrayList<int>，只能指定类或接口类
        list.add(3);    //虽然ArrayList没有add(int)这样的方法，但编译器会自动帮你包装
        int num = list.get(0);   //编译器也会自动地解开Integer对象的包装，因此可以直接赋值给int
    }

autoboxing不只是包装与解开primitive主数据类型给collection用而已，它还可以让你在各种地方交换地运用primitive主数据类型与它的包装类型。

方法的参数：如果参数是某种包装类型，你可以传入相对应的primitive主数据类型，反之亦然。void takeNum(Integer i){}

返回值：如果method声明为返回某种primitive主数据类型，你可以返回兼容的primitive主数据类型，也可以是该主数据类型的包装类型。

boolean表达式：任何预期返回boolean值的位置都可以用求出boolean的表达式来代替，例如说4>2或是Boolean包装类型的引用。

数值运算：可以对包装类型做运算。Integer i = new Integer(3); i++; i=i+3;

赋值：可以将包装类型或primitive主数据类型赋值给声明成相对应的包装或primitive主数据类型。

包装也有静态的实用性方法：

    int x = Integer.parseInt("string2");
    double d = Double.parseDouble("420.45");
    boolean b = new Boolean("true").booleanValue();    //没有Boolean.parseBoolean()

    //将数值转换为String
    double d = 42.5;
    String doubleString = "" + d;   //"+"这个操作符是java中唯一有重载过的运算符

    double d = 42.5;
    String doubleString = Double.toString(d);   //静态方法

    
java不像c语言中，有printf的这种格式化功能。数字与日期的格式化功能并没有结合在输出/输入功能上。通常对用户显示数字是由GUI来进行的。如果格式化功能绑定在文字模式的输入/输出上，那就没有办法把字符串以比较漂亮的格式输出到GUI上。

    //将数字以带逗号的形式格式化
    String s = String.format("%,d", 1000000000);
    System.out.prinln(s);   //输出：1,000,000,000

静态变量例子：Color类已经定义好了红橙黄绿等颜色；System.out是System这个类的一个静态变量，你不必自己创建出System的实例，只要从该class调用out变量即可。

##异常

编译器要确定你了解所调用的方法是有风险的。

如果你把有风险的代码包含在try/catch块中，那么编译器会放心许多。它只会注意你有没有表示你会注意到异常。

异常是一种Exception类型的对象

会抛出异常的方法必须要声明它有可能会这么做。
    //有风险、会抛出异常的程序代码
    public void takeRisk() throws BadException{   //必须要声明它会抛出异常
        if(abandonAllHope){
            throw new BadException();   //创建BadException对象并抛出；
        }
    }

    //调用该方法的程序代码
    public void crossFingers(){
        try{
            abObject.takeRisk();    
        }catch(BadException ex){
            System.out.println("wow");    
            ex.printStackTrace();   //如果无法从异常中恢复，至少要使用printStackTrace()来列出有用的信息
        }
    }

编译器会核对每件事，除了RuntimeExceptions之外。编译器保证：

如果你有抛出异常，则你一定要使用throw来声明这件事;

如果你调用会抛出异常的方法，你必须得确认你知道异常的可能性。将调用包含在try/catch块中是一种满足编译器的方法。

try/catch是用来处理真正的异常，而不是你程序的逻辑错误（RuntimeExceptions），该块要做的是恢复的尝试，或者至少会优雅的列出错误信息。

finally块是用来存放不管有没有异常都得执行的程序。

如果try或catch块有return指令，finally还是会执行！流程会跳到finally然后再回到return指令。

如果有必要的话，方法可以抛出多个异常，但方法的声明必须要含有全部可能的检查异常（若两个或两个以上的异常有共同的父类时，可以只声明该父类就行）。

异常也是多态的，这样的好处在于方法可以不必明确地声明每个可能抛出的异常，可以只声明父类就行。

有多个catch块时，要从小排到大。小的要先上，不然就根本没有机会使用。

另外的方法：如果不想处理异常，你可以把它duck掉来避开：让调用你的方法的程序来catch该异常。

方法抛出异常时，栈上的方法会立即被取出，而异常会再度丢给该方法的调用方。如果调用方也是个ducker，则此ducker也会从栈中被取出，异常再度抛给此时栈上的上一层调用方。

    public class Washer{
        Laundry lanudry = new Laundry();
        
        public void foo() throws ClothingException{
            lanudry.doLaundry();
        }    

        public static void main(String [] args) throws ClothingException{ 
            Washer a = new Washer();
            a.foo();    //两者都躲避异常，因此没人来处理，但有duck掉，所以可以通过编译

        }
    }

只有finally没有catch的try必须要声明异常

    void go() throws FooException{
        try{
            x.duStuff();    
        }finally{
            
        }   
    }

监听按钮事件：

1、实现ActionListener这个接口;

2、向按钮注册（告诉它你要监听事件）；

3、定义事件处理的方法（实现接口上的方法）；

如果类想要知道按钮的ActionEvent，你就得实现ActionListener这个接口。（$增加类的技能：监听按钮事件）

按钮是ActionEvent的来源，因此它必须要知道有哪些对象是需要事件通知的。此按钮有个addActionListener()方法可以提供对事件有兴趣的对象一种表达此兴趣的方法。

当用户按下按钮时，按钮的addActionListener()方法被调用，（$开始通知各对象），按钮会通过调用清单上每个监听的actionPerformed()来启动事件。

作为一个ActionListener()，（$addActionListener里面的对象）编译器会确保你实现此接口的actionPerformed()。

步骤为：增加监听接口--确定点击后，有哪几个对象会做出反应--各对象的反应（方法）

在Gui上面加东西的3种方法：在frame上放置widget；在widget上绘制2d图形；在widget上绘制JPEG图。

内部类可以使用外部类所有的方法与变量，就算是私用的也一样。内部累把存取外部类的方法和变量当作是开自家冰箱。

一个地址可以有65536个不同的端口号可用(0-65535)。

从0-1023的TCP端口号是保留给已知的特定服务使用，不应该使用这些端口。







