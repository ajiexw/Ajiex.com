Tags: Android

安卓基于linux平台，由操作系统、中间件、用户界面、应用软件组成。

2003年Android公司成立（上高二，网吧qq，游戏）；2005年Google收购（上大学，诺基亚功能手机）；2007年OHA成立（上大二）；2008年G1上市（上大三，用山寨诺基亚）。

Android全景图：

---linux核心（照相机、闪存等驱动程序，电源管理，进程管理，电源等一个操作系统最核心最基础的功能）－－liux内核层。如安全性、内存管理、进程管理、网络协议栈和驱动模型等都依赖于该内核。安卓更多的是一些与移动设备相关的驱动程序，主要的驱动如下所示：
    
    显示驱动、键盘驱动、flash内存驱动、照相机驱动、音频驱动、蓝牙驱动、wifi驱动、电源管理。

---两个：Libraries程序包和Ａndroid运行时环境:（前者基本都是由ｃ及c++语言编写，OpengL,SQLite,WebKit开源浏览器内核等，后者包含java开发中一些常见的类库，如ＩＯ,Utility，同时还包含一个Google自己开发的一个虚拟机，专门针对手机系统优化过的）－－系统运行库（C/C++库以及Android运行库）层。包括：

    bionic系统c库：c语言标准库，系统最底层的库，c库通过linux系统来调用；

    多媒体库：支持常见的音频、视频、图片；

    SGL：2d图形引擎库；

    SSL：位于tcp/ip协议与各种应用层协议之间，为数据通信提供支持；

    OpenGL ES 1.0：3d效果的支持；

    SQLite：关系数据库；

    Webkit：web浏览器引擎；

    FreeType：位图以及矢量。

    每个java程序都运行在Dalvik虚拟机之上，一个应用，一个虚拟机实例，一个进程。

---Applicaton Framework应用程序框架，提供了手机开发中一些最基本的api，手机开发主要基于此开发。如View,Activity.－－应用框架层。包括：
    
    丰富而又可扩展的视图view，包括列表，网格，文本框，按钮以及可嵌入的web浏览器等；
    
    内容提供器（Content Providers），它可以让一个应用访问另一个应用的数据（如联系人数据库），或者共享自己的数据；

    资源管理器（Resource Manager），提供非代码资源的访问，如本地字符串，图形和布局文件；

    通知管理器（Notification Manger)，应用可以在状态栏中显示自定义的提示信息；

    活动管理器（Activity Manager），用来管理应用程序生命周期并提供常用的导航退回功能；

    窗口管理器（Window Manager），管理所有的窗口程序；

    包管理器（Packge Manager），安卓系统内的程序管理。

---Application应用程序层。－－应用层

我们开发应用时都是通过框架与安卓底层进行交互，接触最多的就是应用框架层了。应用程序框架是一个应用程序的核心，是所有参与开发的程序员共同使用和遵守的约定，大家在其约定上进行必要的扩展，让程序始终保持主体结构的一致性。`

Android建国纲领：随时随地为每个人提供信息。

Android开发最重要的4个组件：Activity（美女-界面）、Intent（运输大队长-传输数据）、Service（劳模-服务）、Content Provider（国家档案馆-负责存放数据，数据共享给外部其他程序）。任何应用程序都必须在AndroidManifest.xml文件中声明使用到的这些模块。补充：BroadcastReveiver（监听系统行为）。

开发环境的搭建：

安卓SDK：安卓开发工具包、调试工具、类库。包括在命令行执行的可执行文件以及类库（IO、集合等）两部分。

ADT（Android Develpment Tools）：Google开发的Eclipse插件，提升开发效率。

手机模拟器AVD。


Activity启动流程：

    Android操作系统--AndroidManifest.xml--确定首先启动哪个Activity--生成MainActivity类--onCreate()创建该对象--读取布局文件activity_main.xml

所有控件的父类View

Activity里面所有的控件都是对象，对象都有生成自己的类，所有的控件类都是View类的子类。如文本、按钮、多选、单选、布局...

为控件绑定监听器：获取代表控件的对象--定义一个类，实现监听器接口--生成监听器对象--为控件绑定监听器对象

Eclipse导入快捷键：Ctrl+shift+o

override复写快捷键：Alt+/

代码格式化：Ctrl + Shift + F

所谓的控件布局方法，就是指控制控件在Activity当中的位置、大小、颜色以及其他控件样式属性的方法。

两种方法：使用布局文件完成控件布局；在java代码当中完成控件布局。

布局分类：线性布局（LinearLayout）、相对布局（Relative Layout)、ListView、Grid View。

距离单位：px, dp, sp

dpi(dot per inch)每英寸像素 = （宽的平方+高的平方）的平方根 / 手机对角线的英寸数

dp = dip(Device independent pixels) 设备无关像素

换算公式px = dp * (dpi /160)

在dpi为160的屏幕上：1dp = 1px

sp:scaled pixels 通常用于指定字体的大小，当用户修改手机显示字体时，sp会随之改变

外边距：layout_margin, layout_maringTop, layout_marginBottom, layout_marginLeft, layout_marginRight

内边距：padding, paddingTop, paddingBottom, paddingLeft, paddingRight

多选按钮：CheckBox

单选按钮：RadioButton, RadioGroup

监听器：OnClickListener, OnCheckedChangeListener

ImageView的scaleType:matrix, fitXY, fitStart, fitEnd, fitCenter, center, centerCrop, centerInside

java方法：
    
    img1.setScaleType(ScaleType.CENTER);
    img1.setScaleType(ScaleType.CENTER_CROP);
    img1.setScaleType(ScaleType.CENTER_INSIDE);
    img1.setScaleType(ScaleType.FIT_CENTER);
    img1.setScaleType(ScaleType.FIT_START);
    img1.setScaleType(ScaleType.FIT_END);
    img1.setScaleType(ScaleType.FIT_XY);
    img1.setScaleType(ScaleType.MATRIX);

修改图片链接：img1.setImageResource(R.drawable.hashiqi)

matrix 用矩阵来绘制(从左上角起始的矩阵区域)

fitXY  把图不按比例扩大/缩小到View的大小显示（确保图片会完整显示，并充满View）

fitStart  把图片按比例扩大/缩小到View的宽度，显示在View的上部分位置（图片会完整显示）

fitCenter  把图片按比例扩大/缩小到View的宽度，居中显示（图片会完整显示）

fitEnd   把图片按比例扩大/缩小到View的宽度，显示在View的下部分位置（图片会完整显示）

center  按图片的原来size居中显示，当图片宽超过View的宽，则截取图片的居中部分显示，当图片宽小于View的宽，则图片居中显示

centerCrop  按比例扩大/缩小图片的size居中显示，使得图片的高等于View的高，使得图片宽等于或大于View的宽

centerInside  将图片的内容完整居中显示，使得图片按比例缩小或原来的大小（图片比View小时）使得图片宽等于或小于View的宽 （图片会完整显示）

##相对布局

两个元素的相对位置：layout_above, layout_below, layout_toRightOf, layout_toLeftOf

两个元素覆盖对齐：layout_alignLeft, layout_alignRight, layout_alignTop, layout_alignBottom

对齐至基准线：layout_alignBaseline

与父控件边缘对齐：layout_alignParentLeft, layout_alignParentRight, layout_alignParentTop, layout_alignParentBottom

对齐至父控件的中央：layout_centerInParent, layout_centerHorizontal, layout_centerVertical


4.2版本RelativeLayout的新属性

layout_alignStart, layout_alignEnd, layout_alignParentStart, layout_alignParentEnd.

应用程序目录

src：应用程序的源文件存放地

gen：ADT插件帮我们自动生成的目录。--R.java

res：资源目录，在R.java中都会自动生成相应的id。

assets：资源目录，不会在R.java中生成id。

AndroidManifest.xml：整个应用程序的配置文件。


创建Activity的要点

1、一个Activity就是一个类，并且这个类要继承Activity;（继承的Activity来源于android.jar这个包）

2、需要复写onCreate方法；

3、每一个Activity都需要在AndroidManifest.xml文件当中进行配置；

4、为Activity添加必要的控件。


一个Intent对象包含了一组信息：（相当于web的一个请求）

1、Component name（决定开始启动哪一个组件（Activity、Service等等））

2、Action（启动后的组件要做什么）

3、Data（传输的数据）

4、Category

5、Extras（键值对，可以传输给新启动的组件）

6、Flags

Activity生命周期（7种状态） 

FirstApp:onCreate -- onStart -- onResume-- 启动SecondApp之后 -- FirstApp:onPause -- SecondApp:onCreate -- onStart -- onResume -- FirstApp:onStop -- 点击返回按钮 -- SecondApp:onPause -- FirstApp:onRestart（不需要重新绘制） -- onStart -- onResume -- SecondApp:onStop -- SecondApp:onDestory

SecondApp为弹出框时：
FirstApp:onCreate -- onStart -- onResume-- 启动SecondApp之后 -- FirstApp:onPause -- SecondApp:onCreate -- onStart -- onResume -- 点击返回按钮 -- SecondApp:onPause -- FirstApp:onResume -- SecondApp:onStop -- SecondApp:onDestory

当一个应用执行finish()退出时：onPause -- onStop -- onDestory


Activity被destory的两种情况：执行Activity的finish()方法；系统资源不够自动销毁Activity。

Task: a task is a stack of activities. 

调试：

    static final String TAG = "Helloqq";
    Log.v(TAG, "VERBOSE");
    Log.d(TAG, "DEBUG");
    Log.i(TAG, "INFO");
    Log.w(TAG, "WARN");
    Log.e(TAG, "ERROR");

根据规范，log.v, log.d信息应当只存在于开发过程当中，最终版本只可以包含其余3种信息。

R.java是定义项目所有资源的索引文件。

intent-filters描述了Activity启动的位置和时间。每当一个activity（或者操作系统）要执行一个操作时，它将创建出一个intent对象，这个intent对象能承载的信息可描述你想做什么，想处理什么数据，数据是什么类型，以及其他一些信息。而安卓则会和每一个application所暴露的intent-filter数据进行比较，找到最合适的activity来处理调用者所指定的数据和操作。

manifest：根节点，描述了package中所有的内容；

xmlns:android：包含命名空间的声明。使得安卓中各种标准属性能在文件中使用。

package：声明应用程序包。

application :程序包中application级别组件声明的根节点，包含application的一些全局和默认的属性，如标签、icon、主题、必要的权限等。一个manifest中能包含零个或一个此元素（不能大于一个）

android:icon：应用程序图标

uses-sdk：该应用程序所使用的sdk版本

获取string资源：

    Resources r = this.getContext().getResources();
    String appname = ((String) r.getString(R.string.appname))
    String hello = ((String) r.getString(R.string.hello))

项目中所有使用的常量都可以通过xml的方式来定义：
    
    <xml version="1.0" encoding="utf-8"?>
    <resources>
        <color name="red">#ff0000</color>
    </resources>

安卓用intent这个特殊类实现在activity与activity之间的切换。intent类用于描述应用的功能。其结构中有两个最重要的数据：动作和动作对应的数据。

在安卓中，每个应用都运行在各自的进程中，当一个应用需要访问其他应用的数据时，也就是数据需要在不同的虚拟机之间传递，这样的情况操作起来可能有些困难，Content Provider用来解决在不同的应用包之间共享数据。

Content Provider是一个特殊的存储数据的类型，它提供了一套标准的接口用来获取和操作数据，应用可以通过唯一的ContentResolver界面来使用具体的某个Content Provider,通过ContentResover提供的方法来操作Content Provider。

Service是一个生命周期长且没有用户界面的程序。

安卓系统的特点：开放、应用程序无界限、应用程序是在平等的条件下创建的（即便是拨号程序或者主屏幕这样的核心组件）、可以轻松的嵌入网络（可以轻松的嵌入HTML\JAVASCRIPT\CSS，还可以通过webview显示网络内容）、完整的多任务环境，应用程序可以并行运行。

##用户界面

安卓生成屏幕有3种方式：xml配置生成；通过用户界面接口生成；直接用代码生成。



















