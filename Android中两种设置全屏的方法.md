Date:2013-08-20
Tags: Android

应用设置为全屏，一中是在代码中设置，另一种方法是在配置文件里面修改！

##一、代码中设置

	public void onCreate(Bundle savedInstanceState) {        
   		super.onCreate(savedInstanceState);        
        //无title            
       requestWindowFeature(Window.FEATURE_NO_TITLE);            
       //全屏            
       getWindow().setFlags(WindowManager.LayoutParams. FLAG_FULLSCREEN, WindowManager.LayoutParams. FLAG_FULLSCREEN);   
       //设置全屏的代码必须在setContentView之前                         
       setContentView(R.layout.main);        
    
	}        
	
##二、布局文件里面修改

	<activity 
		android:name=".OpenGl_Lesson1"     
		android:theme="@android:style/Theme.NoTitleBar.Fullscreen"     
		android:label="@string/app_name">         
	</activity>        