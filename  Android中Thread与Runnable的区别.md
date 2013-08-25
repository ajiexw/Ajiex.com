Date:2013-08-18
Tags: Android

From:[http://blog.chinaunix.net/uid-27024249-id-3313735.html](http://blog.chinaunix.net/uid-27024249-id-3313735.html)

##Thread类

Thread类是在java.lang包中定义的。一个类只要继承了Thread类同时覆写了本类中的run()方法就可以实现多线程操作了，但是一个类只能继承一个父类，这是此方法的局限，下面看例子：
package org.thread.demo;

    class MyThread extends Thread{
        private String name;
        public MyThread(String name) {
            super();
            this.name = name;
        }
        public void run(){
            for(int i=0;i<10;i++){
                System.out.println("线程开始："+this.name+",i="+i);
            }
        }
    }

    package org.thread.demo;
    public class ThreadDemo01 {
        public static void main(String[] args) {
            MyThread mt1=new MyThread("线程a");
            MyThread mt2=new MyThread("线程b");
            mt1.run();
            mt2.run();
        }
    }

但是，此时结果很有规律，先第一个对象执行，然后第二个对象执行，并没有相互运行。在JDK的文档中可以发现，一旦调用start()方法，则会通过JVM找到run()方法。下面使用start()方法启动线程：

    package org.thread.demo;
    public class ThreadDemo01 {
    public static void main(String[] args) {
    MyThread mt1=new MyThread("线程a");
    MyThread mt2=new MyThread("线程b");
    mt1.start();
    mt2.start();
    }
    };

这样程序可以正常完成交互式运行。那么为啥非要使用start();方法启动多线程呢？
在JDK的安装路径下，src.zip是全部的java源程序，通过此代码找到Thread中的start()方法的定义，可以发现此方法中使用了private native void start0();其中native关键字表示可以调用操作系统的底层函数，那么这样的技术成为JNI技术（java Native Interface）

##Runnable接口

在实际开发中一个多线程的操作很少使用Thread类，而是通过Runnable接口完成。

    public interface Runnable{
        public void run();
    }

例子：

    package org.runnable.demo;

    class MyThread implements Runnable{
        private String name;
        public MyThread(String name) {
            this.name = name;
        }
        public void run(){
            for(int i=0;i<100;i++){
                System.out.println("线程开始："+this.name+",i="+i);
            }
        }
    };

但是在使用Runnable定义的子类中没有start()方法，只有Thread类中才有。此时观察Thread类，有一个构造方法：public Thread(Runnable targer)此构造方法接受Runnable的子类实例，也就是说可以通过Thread类来启动Runnable实现的多线程。（start()可以协调系统的资源）:

    package org.runnable.demo;
    import org.runnable.demo.MyThread;
    public class ThreadDemo01 {
        public static void main(String[] args) {
            MyThread mt1=new MyThread("线程a");
            MyThread mt2=new MyThread("线程b");
            new Thread(mt1).start();
            new Thread(mt2).start();
        }
    }

两种实现方式的区别和联系：
在程序开发中只要是多线程肯定永远以实现Runnable接口为主，因为实现Runnable接口相比继承Thread类有如下好处：
->避免点继承的局限，一个类可以继承多个接口。
->适合于资源的共享
以卖票程序为例，通过Thread类完成：

    package org.demo.dff;
    class MyThread extends Thread{
        private int ticket=10;
        public void run(){
            for(int i=0;i<20;i++){
                if(this.ticket>0){
                    System.out.println("卖票：ticket"+this.ticket--);
                }
            }
        }
    };

下面通过三个线程对象，同时卖票：

    package org.demo.dff;
    public class ThreadTicket {
    public static void main(String[] args) {
        MyThread mt1=new MyThread();
        MyThread mt2=new MyThread();
        MyThread mt3=new MyThread();
        mt1.start();//每个线程都各卖了10张，共卖了30张票
        mt2.start();//但实际只有10张票，每个线程都卖自己的票
        mt3.start();//没有达到资源共享
        }
    }

如果用Runnable就可以实现资源共享，下面看例子：

    package org.demo.runnable;
    class MyThread implements Runnable{
        private int ticket=10;
        public void run(){
            for(int i=0;i<20;i++){
                if(this.ticket>0){
                    System.out.println("卖票：ticket"+this.ticket--);
                }
            }
        }
    }
    package org.demo.runnable;
    public class RunnableTicket {
    public static void main(String[] args) {
        MyThread mt=new MyThread();
        new Thread(mt).start();//同一个mt，但是在Thread中就不可以，如果用同一
        new Thread(mt).start();//个实例化对象mt，就会出现异常
        new Thread(mt).start();
        }
    };

虽然现在程序中有三个线程，但是一共卖了10张票，也就是说使用Runnable实现多线程可以达到资源共享目的。
Runnable接口和Thread之间的联系：
public class Thread extends Object implements Runnable
发现Thread类也是Runnable接口的子类。

-————————————————————————————————————————————————————

From:[http://www.360doc.com/content/10/1219/22/573136_79607619.shtml](http://www.360doc.com/content/10/1219/22/573136_79607619.shtml)

JDK文档：Runnable接口应该由那些打算通过<span style="color:green;font-weight:bold">“某一线程”</span>执行其实例的类来实现。

“当不需要改变一个线程中除了run()方法以外的其他方法时，让线程实现Runnable接口。”

如果让一个线程实现Runnable接口，那么当调用这个线程的对象开辟多个线程时，可以让这些线程调用同一个变量；若这个线程是由继承Thread类而来，则要通过内部类来实现上述功能，利用的就是内部类可任意访问外部变量这一特性。

例子程序：实现Runnable接口

    public class ThreadTest
    {
    public static void main(String[] args)
    {
       MyThread mt=new MyThread();
       new Thread(mt).start();     //通过实现Runnable的类的对象来开辟第一个线程
       new Thread(mt).start();     //通过实现Runnable的类的对象来开辟第二个线程
       new Thread(mt).start();     //通过实现Runnable的类的对象来开辟第三个线程
       //由于这三个线程是通过同一个对象mt开辟的，所以run()里方法访问的是同一个index
    }
    }

    class MyThread implements Runnable    //实现Runnable接口
    {
    int index=0;
    public void run()
    {
       for(;index<=200;)
        System.out.println(Thread.currentThread().getName()+":"+index++);
    }
    }

例子程序：使用Thread

    public class ThreadTest
    {
    public static void main(String[] args)
    {
       MyThread mt=new MyThread();
       mt.getThread().start();       //通过返回内部类的对象来开辟第一个线程
       mt.getThread().start();      //通过返回内部类的对象来开辟第二个线程
       mt.getThread().start();      //通过返回内部类的对象来开辟第三个线程
       //由于这三个线程是通过同一个匿名对象来开辟的，所以run()里方法访问的是同一个index
    }
    }

    class MyThread
    {
    int index=0;
    private class InnerClass extends Thread    //定义一个内部类，继承Thread
    {
       public void run()
       {
        for(;index<=200;)
         System.out.println(getName()+":"+index++);
       }
    }
    Thread getThread()     //这个函数的作用是返回InnerClass的一个匿名对象
    {
       return new InnerClass();
    }
    }

Runnable是接口，建议用接口的方式生成线程，因为接口可以实现多继承，况且Runnable只有一个run方法，很适合继承。在使用Thread的时候只需继承Thread，并且new一个实例出来，调用 start()方法即可以启动一个线程。

	Thread Test = new Thread();
	Test.start();

在使用Runnable的时候需要先new一个实现Runnable的实例，之后启动Thread即可。

	Test impelements Runnable;
	Test t = new Test();
	Thread test = new Thread(t);
	test.start();

总结：Thread和Runnable是实现java多线程的2种方式，runable是接口，thread是类，建议使用runable实现 java多线程，不管如何，最终都需要通过thread.start()来使线程处于可运行状态。


在java中可有两种方式实现多线程，一种是继承Thread类，一种是实现Runnable接口；

##认识Thread的start和run

1） start：

用start方法来启动线程，真正实现了多线程运行，这时无需等待run方法体代码执行完毕而直接继续执行下面的代码。通过调用Thread类的 start()方法来启动一个线程，这时此线程处于就绪（可运行）状态，并没有运行，一旦得到cpu时间片，就开始执行run()方法，这里方法 run()称为线程体，它包含了要执行的这个线程的内容，Run方法运行结束，此线程随即终止。

2） run：

run()方法只是类的一个普通方法而已，如果直接调用Run方法，程序中依然只有主线程这一个线程，其程序执行路径还是只有一条，还是要顺序执行，还是要等待run方法体执行完毕后才可继续执行下面的代码，这样就没有达到写线程的目的。

总结：调用start方法方可启动线程，而run方法只是thread的一个普通方法调用，还是在主线程里执行。




