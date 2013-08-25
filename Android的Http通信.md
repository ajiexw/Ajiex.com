Date: 2013-08-15
Tags: Android

##Android的3种网络连接方式：

标准java接口：java.net.* - HttpURLConnection;

Apache接口：android.net.http.* - HttpClient；

Android接口：android.net.* - InetAddress

##AndroidSDK中一些与网络有关的package：

java.net/java.io/java.nio/org.apache.*/android.net/android.net.http/android.net.wifi/android.telephony.gsm。

安卓中http服务应该放到线程中执行，以此来达到异步处理。

##HttpURLConnection

HttpURLConnection是java的标准类，HttpURLConnection继承自URLConnection，可用于向指定网站发送GET请求、POST请求。

URLConnection与HttpURLConnection都是抽象类，无法直接实例化对象。其对象主要通过URL的openConnection方法获得，创建一个HttpURLConnection连接的代码如下：

	URL url= new URL("http://www.google.com/");
	HttpURLConnection urlConn = (HttpURLConnection) url.openConnection();

在一般情况下，如果只是需要Web站点的某个简单页面提交请求并获取服务器响应，HttpURLConnection完全可以胜任。但在绝大部分情况下，Web站点的网页可能没这么简单，这些页面并不是通过一个简单的URL就可访问的，可能需要用户登录而且具有相应的权限才可访问该页面。在这种情况下，就需要涉及Session、Cookie的处理了，如果打算使用HttpURLConnection来处理这些细节，当然也是可能实现的，只是处理起来难度就大了。

为了更好地处理向Web站点请求，包括处理Session、Cookie等细节问题，Apache开源组织提供了一个HttpClient项目，看它的名称就知道，它是一个简单的HTTP客户端（并不是浏览器），可以用于发送HTTP请求，接收HTTP响应。但不会缓存服务器的响应，不能执行HTML页面中嵌入的Javascript代码；也不会对页面内容进行任何解析、处理。
 
简单来说，HttpClient就是一个增强版的HttpURLConnection，HttpURLConnection可以做的事情HttpClient全部可以做；HttpURLConnection没有提供的有些功能，HttpClient也提供了，但它只是关注于如何发送请求、接收响应，以及管理HTTP连接。


使用HttpClient发送请求、接收响应很简单，只要如下几步即可。

1、创建HttpClient对象。
2、如果需要发送GET请求，创建HttpGet对象；如果需要发送POST请求，创建HttpPost对象。
3、如果需要发送请求参数，可调用HttpGet、HttpPost共同的setParams(HetpParams params)方法来添加请求参数；对于HttpPost对象而言，也可调用setEntity(HttpEntity entity)方法来设置请求参数。
4、调用HttpClient对象的execute(HttpUriRequest request)发送请求，执行该方法返回一个HttpResponse。
5、调用HttpResponse的getAllHeaders()、getHeaders(String name)等方法可获取服务器的响应头；调用HttpResponse的getEntity()方法可获取HttpEntity对象，该对象包装了服务器的响应内容。程序可通过该对象获取服务器的响应内容。
