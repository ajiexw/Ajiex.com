Date: 2013-08-17
Tags: Android

##一、Handler的定义

Handler主要接受子线程发送的数据, 并用此数据配合主线程更新UI。可以说它是内部启动的线程Runnable和Activity交互的桥梁，或者说是内部线程的回调函数。

当应用程序启动时，Android首先会开启一个主线程,我们创建的Service、Activity以及Broadcast均是一个主线程，这里可以理解为UI线程。主线程为管理界面中的UI控件进行事件分发, 比如：点击一个Button,Android会分发事件到Button上，来响应用户操作。  

如果此时在主线程执行耗时操作，例如:I/O读写、数据库操作以及网络下载，界面会出现假死现象,如果5秒钟还没有完成的话，会收到系统的错误提示"强制关闭"。需要把这些耗时操作放在一个子线程中执行。但是Android主线程是线程不安全的，更新UI只能在主线程中更新. 

Android用Handler来解决这个问题，在Activity中创建handler并将其引用传递给worker thread，worker thread执行完任务后使用handler发送消息通知Activity更新UI。

##二、Handler一些特点

handler可以分发Message对象和Runnable对象到主线程中, 当创建一个新的Handler实例时, 它会绑定到当前线程和消息的队列中,开始分发数据。默认的handler操作的就是主线程。
        
它包括两种队列：线程队列和消息队列，线程对象和消息对象分别对应线程队列和消息队列，分别通过post和sendmessage来实现。
   
    post(Runnable)
    postAtTime(Runnable,long)
    postDelayed(Runnable long)
    sendEmptyMessage(int)
    sendMessage(Message)
    sendMessageAtTime(Message,long)
    sendMessageDelayed(Message,long)
               
 
##三、Handler涉及的几个概念：
    
- Message：包含了消息id、数据等信息，由MessageQueue队列控制。
    
- MessageQueue：消息队列，用链表的方式存储Message，按照FIFO（队列先进先出规则）让Looper来抽取Message进行处理。
     
- Looper：一个线程只有一个Looper对象，负责不断从MessageQueue抽取Message进行处理。

- Bundle：Android中Handler可以传递一些内容，通过Bundle对象可以封装String、Integer以及Blob二进制对象，并在线程中使用Handler对象的sendEmptyMessage或sendMessage方法来传递一个Bundle对象到Handler处理器。Handler处理器通过重写方法handleMessage(Message msg)将Bundle解包来实现Handler类更新UI线程中的内容，实现控件的刷新操作。

##四、使用Handler的流程：

1）创建Handler对象，可以直接使用Handler无参构造函数创建，也可以继承handler类后重写HandlerMessage函数再创建。

	Handler handler = new Handler(){
   		public void handleMessage(Message msg) {

        }
	};

2）建立Runnable对象，复写run方法，run方法中是将要执行的操作。

    Runnable runnable = new Runnable() {
        public void run() {
        	handler.obtainMessage().sendToTarget();
        }
    };

3）启动线程

	public void onCreate(Bundle savedInstanceState) {
    	super.onCreate(savedInstanceState);
    	this.setContentView(R.layout.share_mblog_view);
    	new Thread(runnable).start();
    	//开启一个线程唯一的方式就是执行start()方法。而只执行run()方法的，并没有产生新线程，还是运行在主线程中
    	//所以Handler是依附UI线程，实现Runnable接口的run()方法的时候，不能进行耗时操作，不然会出现UI卡死。

	}	

##五、sendMessage与obtainmessage区别

向Handler发送消息的两种方法：

	Message msg = handler.obtainMessage();  
	msg.arg1 = i;  
	msg.sendToTarget();   
  
    #第二种
	Message msg=new Message();  
    msg.arg1=i;  
    handler.sendMessage(msg);  


>##public final boolean sendMessage (Message msg)

>Pushes a message onto the end of the message queue after all pending messages before the current time. It will be received in handleMessage(Message), in the thread attached to this handler.

>Returns

>Returns true if the message was successfully placed in to the message queue. Returns false on failure, usually because the looper processing the message queue is exiting.

>##public final Message obtainMessage ()

>Returns a new Message from the global message pool. More efficient than creating and allocating new instances. The retrieved message has its handler set to this instance (Message.target == this). If you don't want that facility, just call Message.obtain() instead.
 
第一种写法是使用Handler的obtainmessage()方法从消息池中拿来一个msg。sendtoTarget()是message的方法,这个要事先知道目标是谁，才能调.

第二种写法是创建一个Message实例，然后调用handler的发送消息方法发送消息。sendMessage()是Handler的方法，这个是目标直接自己调。

建议使用Handler.obtainMessage()替代msg = new Message()，因为使用前者不需要重新创建对象以及分配内存，同时还可以循环利用。 
