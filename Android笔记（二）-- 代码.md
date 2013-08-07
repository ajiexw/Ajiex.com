Date:2013-07-30 23:19
Tags: Android

##TextView
	textview = (TextView)this.findViewById(R.id.textview);
	String string = "Toast";
	textview.setTextSize(30);
	textview.setText(string);


##Toast
	public void displayToast(String str){
		Toast.makeText(this, str, Toast.LENGTH_SHORT).show();
	}
	
##触屏点击事件TouchEvent
	
	public boolean onTouchEvent(MotionEvent event){
		int iAction = event.getAction();
		if(iAction == MotionEvent.ACTION_CANCEL || iAction == MotionEvent.ACTION_DOWN || iAction == MotionEvent.ACTION_MOVE){
			return false;
		}
		int x = (int) event.getX();
		int y = (int) event.getY();
		displayToast("触笔点击坐标：（" + Integer.toString(x) + "," + Integer.toString(y) + ")");
		return super.onTouchEvent(event);
	}

##ListView

	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		
		//创建布局对象
		m_LinearLayout = new LinearLayout(this);
		
		//设置布局属性
		m_LinearLayout.setOrientation(LinearLayout.VERTICAL);
		m_LinearLayout.setBackgroundColor(android.graphics.Color.BLACK);
		
		//创建ListView对象
		m_ListView = new ListView(this);
		
		LinearLayout.LayoutParams param = new LinearLayout.LayoutParams(LinearLayout.LayoutParams.MATCH_PARENT, LinearLayout.LayoutParams.WRAP_CONTENT);
		m_ListView.setBackgroundColor(Color.BLACK);
		
		//添加m_ListView到m_LinearLauout布局
		m_LinearLayout.addView(m_ListView, param);
		
		//设置显示m_LinearLayout布局
		setContentView(m_LinearLayout);
		
		//获取数据库Phones的Cursor
        Cursor cur = getContentResolver().query(ContactsContract.Contacts.CONTENT_URI, null, null, null, null);
		startManagingCursor(cur);
		
		//ListAdapter是ListView和后台数据的桥梁
		ListAdapter adapter = new SimpleCursorAdapter(this, 
				android.R.layout.simple_list_item_2, 
				cur, 
				new String[] {PhoneLookup.DISPLAY_NAME, PhoneLookup.NORMALIZED_NUMBER},
				new int[] {android.R.id.text1, android.R.id.text2});
		
		m_ListView.setAdapter(adapter);
		
	}
	
工程中使用了电话本数据，需要在AndroidManifest.xml中添加<uses-permission … />	
	<application
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" >
        <activity
            android:name="com.ajiex.hand_listview.MainActivity"
            android:label="@string/app_name" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>
    
    <uses-permission android:name="android.permission.READ_CONTACTS" />
    
##收到短信后弹出Toast提示

    public class SMSReceiver extends BroadcastReceiver{
		public void onReceive(Context context, Intent intent){
			Bundle bundle = intent.getExtras();
			Object messages[] = (Object[]) bundle.get("pdus");
			SmsMessage smsMessage[] = new SmsMessage[messages.length];
			for (int n = 0; n < messages.length; n++){
				smsMessage[n] = SmsMessage.createFromPdu((byte[]) messages[n]);
			}
			
			Toast toast = Toast.makeText(context, "短信内容: " + smsMessage[0].getMessageBody(), Toast.LENGTH_LONG);
			toast.setGravity(Gravity.TOP|Gravity.LEFT, 0, 200);
			toast.show();
		}
	}
	
项目中使用了短信接口，需要在AndroidManifest.xml中声明其权限：

	<uses-permission android:name="android.permission.RECEIVE_SMS"></uses-permission>
    <application android:icon="@drawable/icon" android:label="@string/app_name">
        <activity android:name=".Activity01"
                  android:label="@string/app_name">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
		 <receiver android:name=".SMSReceiver" android:enabled="true">  
		     <intent-filter>  
			      <action android:name="android.provider.Telephony.SMS_RECEIVED"/>  
			  </intent-filter>  
		 </receiver> 
    </application>

    
##EditText

m_EditText.setHint("请输入账号");
//在XML中同样可以实现：android:hint = "请输入账号";    

//获取编辑框内容    
m_EditText.getText().toString(); 


下面两行代码只有用在AbsoluteLayout中才会看到效果，而一般默认的是LinearLayout，或者其他布局，加入这两行代码属于废话，没有用。    
    
	android:layout_x="30px"
	android:layout_y="30px"   
    
##RadioGroup、RadioButton

	public void onCreate(Bundle savedInstanceState){
		super.onCreate(savedInstanceState);
		setContentView(R.layout.main);

		m_TextView = (TextView) findViewById(R.id.TextView01);
		m_RadioGroup = (RadioGroup) findViewById(R.id.RadioGroup01);
		m_Radio1 = (RadioButton) findViewById(R.id.RadioButton1);
		m_Radio2 = (RadioButton) findViewById(R.id.RadioButton2);
		m_Radio3 = (RadioButton) findViewById(R.id.RadioButton3);
		m_Radio4 = (RadioButton) findViewById(R.id.RadioButton4);

		m_RadioGroup.setOnCheckedChangeListener(new RadioGroup.OnCheckedChangeListener(){
			public void onCheckedChanged(RadioGroup group, int checkedId){
				if (checkedId == m_Radio2.getId()){
					DisplayToast("正确答案：" + m_Radio2.getText() + "，恭喜你，回答正确！");
				}else{
					DisplayToast("请注意！回答错误");
				}
			}
		});
	}
	
		public void DisplayToast(String str){
			Toast toast = Toast.makeText(this, str, Toast.LENGTH_LONG);
			toast.setGravity(Gravity.TOP, 0, 220);
			toast.show();
		}
	}    
    
    
##CheckBox

	public void onCreate(Bundle savedInstanceState){
		super.onCreate(savedInstanceState);
		setContentView(R.layout.main);

		m_TextView1 = (TextView) findViewById(R.id.TextView1);
		m_Button1 = (Button) findViewById(R.id.button1);

		m_CheckBox1 = (CheckBox) findViewById(R.id.CheckBox1);
		m_CheckBox2 = (CheckBox) findViewById(R.id.CheckBox2);
		m_CheckBox3 = (CheckBox) findViewById(R.id.CheckBox3);
		m_CheckBox4 = (CheckBox) findViewById(R.id.CheckBox4);

		m_CheckBox1.setOnCheckedChangeListener(new CheckBox.OnCheckedChangeListener() {
			public void onCheckedChanged(CompoundButton buttonView, boolean isChecked)
			{
				if(m_CheckBox1.isChecked())
				{
					DisplayToast("你选择了"+m_CheckBox1.getText());
				}
			}
		});

		m_CheckBox2.setOnCheckedChangeListener(new CheckBox.OnCheckedChangeListener() {
			public void onCheckedChanged(CompoundButton buttonView, boolean isChecked)
			{
				if(m_CheckBox2.isChecked())
				{
					DisplayToast("你选择了"+m_CheckBox2.getText());
				}
			}
		});

		m_CheckBox3.setOnCheckedChangeListener(new CheckBox.OnCheckedChangeListener() {
			public void onCheckedChanged(CompoundButton buttonView, boolean isChecked)
			{
				if(m_CheckBox3.isChecked())
				{
					DisplayToast("你选择了"+m_CheckBox3.getText());
				}
			}
		});

		m_CheckBox4.setOnCheckedChangeListener(new CheckBox.OnCheckedChangeListener() {
			public void onCheckedChanged(CompoundButton buttonView, boolean isChecked)
			{
				if(m_CheckBox4.isChecked())
				{
					DisplayToast("你选择了"+m_CheckBox4.getText());
				}
			}
		});

		m_Button1.setOnClickListener(new Button.OnClickListener(){
			public void onClick(View v){
				int num = 0;
				if(m_CheckBox1.isChecked())
				{
					num++;
				}
				if(m_CheckBox2.isChecked())
				{
					num++;
				}
				if(m_CheckBox3.isChecked())
				{
					num++;
				}
				if(m_CheckBox4.isChecked())
				{
					num++;
				}
				DisplayToast("你一共选择了"+num+"项");
			}
		});
	} 
   
##Spinner

	public class Activity01 extends Activity{
		private static final String[]	m_Countries= { "O型", "A型", "B型", "AB型", "其他" };

		private TextView				m_TextView;
		private Spinner					m_Spinner;
		private ArrayAdapter<String>	adapter;

		public void onCreate(Bundle savedInstanceState){
			super.onCreate(savedInstanceState);
			setContentView(R.layout.main);

			m_TextView = (TextView) findViewById(R.id.TextView1);
			m_Spinner = (Spinner) findViewById(R.id.Spinner1);

			adapter = new ArrayAdapter<String>(this, android.R.layout.simple_spinner_item, m_Countries);

			adapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);
		
			m_Spinner.setAdapter(adapter);

			m_Spinner.setOnItemSelectedListener(new Spinner.OnItemSelectedListener() {
				public void onItemSelected(AdapterView<?> arg0, View arg1, int arg2, long arg3){
					m_TextView.setText("你的血型是：" + m_Countries[arg2]);
					arg0.setVisibility(View.VISIBLE);
				}

				public void onNothingSelected(AdapterView<?> arg0){
					// TODO Auto-generated method stub
				}

			});
		}
	}    
    
##AutoCompleteTextView  
    
	import android.app.Activity;
	import android.os.Bundle;
	import android.widget.ArrayAdapter;
	import android.widget.AutoCompleteTextView;
	import android.widget.MultiAutoCompleteTextView;

	public class Activity01 extends Activity
	{
		private static final String[]	autoString		= new String[] { "a2", "abf", "abe", "abcde", "abc2", "abcd3", "abcde2", "abc2", "abcd2", "abcde2" };
		
    	public void onCreate(Bundle savedInstanceState)
		{
			super.onCreate(savedInstanceState);
			setContentView(R.layout.main);
		
	    	ArrayAdapter<String> adapter = new ArrayAdapter<String>(this,
	            android.R.layout.simple_dropdown_item_1line, autoString);
	    
	    	AutoCompleteTextView m_AutoCompleteTextView = (AutoCompleteTextView) 
	       findViewById(R.id.AutoCompleteTextView01);
	    
	    	m_AutoCompleteTextView.setAdapter(adapter);
	    
	    	MultiAutoCompleteTextView mm_AutoCompleteTextView = (MultiAutoCompleteTextView)      
	    	findViewById(R.id.MultiAutoCompleteTextView01);
	    
	    	mm_AutoCompleteTextView.setAdapter(adapter);
	    	mm_AutoCompleteTextView.setTokenizer(new MultiAutoCompleteTextView.CommaTokenizer());
		}	
	}
    
    
##DatePicker、TimePicker

	import java.util.Calendar;
	import android.app.Activity;
	import android.app.DatePickerDialog;
	import android.app.TimePickerDialog;
	import android.os.Bundle;
	import android.view.View;
	import android.widget.Button;
	import android.widget.DatePicker;
	import android.widget.TextView;
	import android.widget.TimePicker;

	public class Activity01 extends Activity
	{
		TextView	m_TextView;

		DatePicker	m_DatePicker;
		//ÉùÃ÷TimePicker¶ÔÏó
		TimePicker	m_TimePicker;
		Button 		m_dpButton;
		Button 		m_tpButton;

		Calendar 	c;
		
		public void onCreate(Bundle savedInstanceState)
		{
			super.onCreate(savedInstanceState);
			setContentView(R.layout.main);
		
			c=Calendar.getInstance();
		
			m_TextView= (TextView) findViewById(R.id.TextView01);
			m_dpButton = (Button)findViewById(R.id.button1);
			m_tpButton = (Button)findViewById(R.id.button2);
		
		
			m_DatePicker = (DatePicker) findViewById(R.id.DatePicker01);
		
			m_DatePicker.init(c.get(Calendar.YEAR), c.get(Calendar.MONTH), c.get(Calendar.DAY_OF_MONTH), new DatePicker.OnDateChangedListener() {
			@Override
			public void onDateChanged(DatePicker view, int year, int monthOfYear, int dayOfMonth)
			{
				c.set(year, monthOfYear, dayOfMonth);
			}
			});

		
			m_TimePicker = (TimePicker) findViewById(R.id.TimePicker01);
		
			m_TimePicker.setIs24HourView(true);

		
			m_TimePicker.setOnTimeChangedListener(new TimePicker.OnTimeChangedListener() {
			@Override
			public void onTimeChanged(TimePicker view, int hourOfDay, int minute)
			{
				
				c.set(year, month, day, hourOfDay, minute, second);
			}
			});
		
		
			m_dpButton.setOnClickListener(new Button.OnClickListener(){
				public void onClick(View v)
				{
					new DatePickerDialog(Activity01.this,
					new DatePickerDialog.OnDateSetListener()
					{
						public void onDateSet(DatePicker view, int year, int monthOfYear, int dayOfMonth)
						{
					
						}
					},c.get(Calendar.YEAR), c.get(Calendar.MONTH), c.get(Calendar.DAY_OF_MONTH)).show();
				}
			});
		
			m_tpButton.setOnClickListener(new Button.OnClickListener() {
				public void onClick(View v)
				{
					new TimePickerDialog(Activity01.this,
						new TimePickerDialog.OnTimeSetListener()
						{
							public void onTimeSet(TimePicker view, int hourOfDay,int minute)
							{
							
							}
						},c.get(Calendar.HOUR_OF_DAY), c.get(Calendar.MINUTE), true).show();
				}
			});
		}
	}         
	
##Button

	import android.app.Activity;
	import android.graphics.Color;
	import android.os.Bundle;
	import android.view.Gravity;
	import android.view.View;
	import android.widget.Button;
	import android.widget.Toast;

	public class Activity01 extends Activity
	{
		Button m_Button1,m_Button2;

		public void onCreate(Bundle savedInstanceState)
		{
			super.onCreate(savedInstanceState);
			setContentView(R.layout.main);
		
			m_Button1=(Button)findViewById(R.id.Button1);
			m_Button2=(Button)findViewById(R.id.Button2);
		
			m_Button1.setText("开始");
			m_Button2.setText("结束");
		
			m_Button1.setWidth(150);
			m_Button2.setWidth(100);
		
			m_Button1.setTextColor(Color.GREEN);
			m_Button2.setTextColor(Color.RED);
		
			m_Button1.setTextSize(30);
			m_Button2.setTextSize(20);
		
			m_Button1.setBackgroundColor(Color.BLUE);
		
			m_Button1.setOnClickListener(new Button.OnClickListener(){
			public void onClick(View v)
			{
				Toast toast = Toast.makeText(Activity01.this, "你点击了"+m_Button1.getText()+"按钮", Toast.LENGTH_LONG);
				toast.setGravity(Gravity.TOP, 0, 150);
				toast.show();
			}
			});
		
			m_Button2.setOnClickListener(new Button.OnClickListener(){
			public void onClick(View v)
			{
				Activity01.this.finish();
			}
			});
		}	
	}	  

##Menu
要实现菜单功能，首先要通过方法onCreateOptionsMenu方法来创建菜单，然后需要对其能够触发的事件进行监听，这样才能够在事件监听onOptionsItemSelected中根据不同菜单选项来执行不同的任务。可以通过XML布局来实现，也可以通过menu.add方法来实现。

方法一：通过XML布局来实现，在res文件夹中建立menu文件夹，创建如下文件：menu.xml，然后在onCreateOptionsMenu方法中装载这个菜单布局文件，再在onOptionsItemSelected监听方法中通过getItemId方法获得当前选中菜单的ID.

	#menu文件
	<menu xmlns:android="http://schemas.android.com/apk/res/android">
   		<item android:id="@+id/about"
          android:title="关于" />
    	<item android:id="@+id/exit"
          android:title="退出" />
	</menu>	
	
	#Activity文件
	package com.ajiex.android.Examples_04_13;

	import android.app.Activity;
	import android.content.Intent;
	import android.os.Bundle;
	import android.view.Menu;
	import android.view.MenuInflater;
	import android.view.MenuItem;

	public class Activity01 extends Activity
	{
		public void onCreate(Bundle savedInstanceState)
		{
			super.onCreate(savedInstanceState);
			setContentView(R.layout.main);
		}
		public boolean onCreateOptionsMenu(Menu menu)
		{
			MenuInflater inflater = getMenuInflater();
			inflater.inflate(R.menu.menu, menu);
			return true;
		}

		public boolean onOptionsItemSelected(MenuItem item)
		{
			int item_id = item.getItemId();
			switch (item_id)
			{
				case R.id.about:
					Intent intent = new Intent();
					intent.setClass(Activity01.this, Activity02.class);
					startActivity(intent);
					Activity01.this.finish();
					break;
				case R.id.exit:
					Activity01.this.finish();
					break;
			}
			return true;
		}
	}
	 
方法二：通过menu.add方法实现

package com.ajiex.android.Examples_04_13;

	import android.app.Activity;
	import android.content.Intent;
	import android.os.Bundle;
	import android.view.Menu;
	import android.view.MenuItem;

	public class Activity02 extends Activity
	{
		public void onCreate(Bundle savedInstanceState)
		{
			super.onCreate(savedInstanceState);
			setContentView(R.layout.main2);
		}

		public boolean onCreateOptionsMenu(Menu menu)
		{
			menu.add(0, 0, 0, R.string.ok);
			menu.add(0, 1, 1, R.string.back);
			return true;
		}

		public boolean onOptionsItemSelected(MenuItem item)
		{
			int item_id = item.getItemId();

			switch (item_id)
			{
				case 0:
				case 1:
					Intent intent = new Intent();
					intent.setClass(Activity02.this, Activity01.class);
					startActivity(intent);
					Activity02.this.finish();
					break;
			}
			return true;
		}
	}
	
项目中使用了两个Activity，需要在AndroidManifest.xml中声明：
	
	<?xml version="1.0" encoding="utf-8"?>
	<manifest
		xmlns:android="http://schemas.android.com/apk/res/android"
		package="com.yarin.android.Examples_04_13"
		android:versionCode="1"
		android:versionName="1.0"
		>
	<application
		android:icon="@drawable/icon"
		android:label="@string/app_name"
		>
	<activity
		android:name=".Activity01"
		android:label="@string/app_name"
		>
	<intent-filter>
	<action android:name="android.intent.action.MAIN" />
	<category android:name="android.intent.category.LAUNCHER" />
	</intent-filter>
	</activity>
	<activity android:name="Activity02" ></activity>
	</application>
	<uses-sdk android:minSdkVersion="5" />
	</manifest>	
	
##Dialog

	#/layout/dialog.xml
	<?xml version="1.0" encoding="utf-8"?>
	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical">

    <TextView 
        android:id="@+id/username"
        android:layout_height="wrap_content"
        android:layout_width="wrap_content"
        android:layout_marginLeft="20dip"
        android:layout_marginRight="20dip"
        android:text="账号"
        android:gravity="left"
        android:textAppearance="?android:attr/textAppearanceMedium" />
            
    <EditText
        android:id="@+id/username"
        android:layout_height="wrap_content"
        android:layout_width="fill_parent"
        android:layout_marginLeft="20dip"
        android:layout_marginRight="20dip"
        android:scrollHorizontally="true"
        android:autoText="false"
        android:capitalize="none"
        android:gravity="fill_horizontal"
        android:textAppearance="?android:attr/textAppearanceMedium" />

    <TextView
        android:id="@+id/password"
        android:layout_height="wrap_content"
        android:layout_width="wrap_content"
        android:layout_marginLeft="20dip"
        android:layout_marginRight="20dip"
        android:text="密码"
        android:gravity="left"
        android:textAppearance="?android:attr/textAppearanceMedium" />
            
    <EditText
        android:id="@+id/password"
        android:layout_height="wrap_content"
        android:layout_width="fill_parent"
        android:layout_marginLeft="20dip"
        android:layout_marginRight="20dip"
        android:scrollHorizontally="true"
        android:autoText="false"
        android:capitalize="none"
        android:gravity="fill_horizontal"
        android:password="true"
        android:textAppearance="?android:attr/textAppearanceMedium" /> 
	</LinearLayout>
	
	
	#Activity.java
	
	package com.yarin.android.Examples_04_14;

	import android.app.Activity;
	import android.app.AlertDialog;
	import android.app.Dialog;
	import android.app.ProgressDialog;
	import android.content.DialogInterface;
	import android.os.Bundle;
	import android.view.LayoutInflater;
	import android.view.View;

	public class Activity01 extends Activity 
	{
	ProgressDialog m_Dialog;
	
	@Override
	public void onCreate(Bundle savedInstanceState)
	{
		super.onCreate(savedInstanceState);
		setContentView(R.layout.main);

		Dialog dialog = new AlertDialog.Builder(Activity01.this)
			.setTitle("登录提示")
			.setMessage("这里需要登录！")
			.setPositiveButton("确定",
			new DialogInterface.OnClickListener() 
			{
				public void onClick(DialogInterface dialog, int whichButton)
				{	
					LayoutInflater factory = LayoutInflater.from(Activity01.this);
					
	                final View DialogView = factory.inflate(R.layout.dialog, null);
	                
	                AlertDialog dlg = new AlertDialog.Builder(Activity01.this)
	                .setTitle("登陆框")
	                .setView(DialogView)
	                .setPositiveButton("确定",
	                new DialogInterface.OnClickListener()
	                {
	                    public void onClick(DialogInterface dialog, int whichButton) 
	                    {
	               
	                    	m_Dialog = ProgressDialog.show
	                                   (
	                                	 Activity01.this,
	                                     "请等待...",
	                                     "正在登录...", 
	                                     true
	                                   );
	                        
	                        new Thread()
	                        { 
	                          public void run()
	                          { 
	                            try
	                            { 
	                              sleep(3000);
	                            }
	                            catch (Exception e)
	                            {
	                              e.printStackTrace();
	                            }
	                            finally
	                            {
	                            	m_Dialog.dismiss();
	                            }
	                          }
	                        }.start(); 
	                    }
	                })
	                .setNegativeButton("取消", 
	                new DialogInterface.OnClickListener() 
	                {
	                    public void onClick(DialogInterface dialog, int whichButton)
	                    {
	                    	Activity01.this.finish();
	                    }
	                })
	                .create();
	                dlg.show();
				}
			}).setNeutralButton("退出", 
			new DialogInterface.OnClickListener() 
			{
			public void onClick(DialogInterface dialog, int whichButton)
			{
				Activity01.this.finish();
			}
		}).create();
		dialog.show();
	}
	}
	 
	 
##图片视图ImageView

##带图标的按钮ImageButton

##拖动效果Gallery

##切换图片ImageSwitcher

##网格视图GridView

##卷轴视图ScrollView

##进度条ProgressBar

##拖动条SeekBar

##状态栏提示（Notification、NotificationManager）


	#AndroidManifest.xml

    <?xml version="1.0" encoding="utf-8"?>
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
          package="com.yarin.android.Examples_04_23"
          android:versionCode="1"
          android:versionName="1.0">
        <application android:icon="@drawable/icon" android:label="@string/app_name">
            <activity android:name=".Activity01"
                      android:label="@string/app_name">
                <intent-filter>
                    <action android:name="android.intent.action.MAIN" />
                    <category android:name="android.intent.category.LAUNCHER" />
                </intent-filter>
            </activity>
        <activity android:name=".Activity02" android:label="@string/app_name">
        </activity>
        </application>
        <uses-sdk android:minSdkVersion="5" />
    </manifest> 

	#Activity01.java
    package com.yarin.android.Examples_04_23;

    import android.app.Activity;
    import android.app.Notification;
    import android.app.NotificationManager;
    import android.app.PendingIntent;
    import android.content.Intent;
    import android.os.Bundle;
    import android.view.View;
    import android.widget.Button;

    public class Activity01 extends Activity
    {
        Button				m_Button1, m_Button2, m_Button3, m_Button4;

        //声明通知（消息）管理器
        NotificationManager	m_NotificationManager;
        Intent				m_Intent;
        PendingIntent		m_PendingIntent;
        //声明Notification对象
        Notification		m_Notification;


        /** Called when the activity is first created. */
        @Override
        public void onCreate(Bundle savedInstanceState)
        {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.main);
            //初始化NotificationManager对象
            m_NotificationManager = (NotificationManager) getSystemService(NOTIFICATION_SERVICE);

            //获取4个按钮对象
            m_Button1 = (Button) findViewById(R.id.Button01);
            m_Button2 = (Button) findViewById(R.id.Button02);
            m_Button3 = (Button) findViewById(R.id.Button03);
            m_Button4 = (Button) findViewById(R.id.Button04);

            //点击通知时转移内容
            m_Intent = new Intent(Activity01.this, Activity02.class);
            //主要是设置点击通知时显示内容的类
            m_PendingIntent = PendingIntent.getActivity(Activity01.this, 0, m_Intent, 0);
            //构造Notification对象
            m_Notification = new Notification();

            m_Button1.setOnClickListener(new Button.OnClickListener() {
                public void onClick(View v)
                {
                    //设置通知在状态栏显示的图标
                    m_Notification.icon = R.drawable.img1;
                    //当我们点击通知时显示的内容
                    m_Notification.tickerText = "Button1通知内容...........";
                    //通知时发出默认的声音
                    m_Notification.defaults = Notification.DEFAULT_SOUND;
                    //设置通知显示的参数
                    m_Notification.setLatestEventInfo(Activity01.this, "Button1", "Button1通知", m_PendingIntent);
                    //可以理解为执行这个通知
                    m_NotificationManager.notify(0, m_Notification);
                }
            });

            m_Button2.setOnClickListener(new Button.OnClickListener() {
                public void onClick(View v)
                {

                    m_Notification.icon = R.drawable.img2;
                    m_Notification.tickerText = "Button2通知内容...........";
                    //通知时震动
                    m_Notification.defaults = Notification.DEFAULT_VIBRATE;
                    m_Notification.setLatestEventInfo(Activity01.this, "Button2", "Button2通知", m_PendingIntent);
                    m_NotificationManager.notify(0, m_Notification);
                }
            });

            m_Button3.setOnClickListener(new Button.OnClickListener() {
                public void onClick(View v)
                {
                    m_Notification.icon = R.drawable.img3;
                    m_Notification.tickerText = "Button3通知内容...........";
                    //通知时屏幕发亮
                    m_Notification.defaults = Notification.DEFAULT_LIGHTS;
                    m_Notification.setLatestEventInfo(Activity01.this, "Button3", "Button3通知", m_PendingIntent);
                    m_NotificationManager.notify(0, m_Notification);
                }
            });

            m_Button4.setOnClickListener(new Button.OnClickListener() {
                public void onClick(View v)
                {
                    m_Notification.icon = R.drawable.img4;
                    m_Notification.tickerText = "Button4通知内容..........";
                    //通知时既震动又屏幕发亮还有默认的声音
                    m_Notification.defaults = Notification.DEFAULT_ALL;
                    m_Notification.setLatestEventInfo(Activity01.this, "Button4", "Button4通知", m_PendingIntent);
                    m_NotificationManager.notify(0, m_Notification);
                }
            });
        }
    }

	#Activity02.java
	package com.yarin.android.Examples_04_23;
    import android.app.Activity;
    import android.os.Bundle;
    public class Activity02 extends Activity
    {
        public void onCreate(Bundle savedInstanceState)
        {
            super.onCreate(savedInstanceState);
            //这里直接限制一个TextView
            setContentView(R.layout.main2);
        }
    }
    
##对话框中的进度条ProgressDialog
    package com.yarin.android.Examples_04_24;

    import android.app.Activity;
    import android.app.ProgressDialog;
    import android.content.DialogInterface;
    import android.os.Bundle;
    import android.view.View;
    import android.widget.Button;

    public class Activity01 extends Activity
    {
        private Button mButton01,mButton02;
        
        int m_count = 0;
        //声明进度条对话框
        ProgressDialog m_pDialog;
        /** Called when the activity is first created. */
        @Override
        public void onCreate(Bundle savedInstanceState)
        {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.main);
            
            //得到按钮对象
            mButton01 = (Button)findViewById(R.id.Button01);
            mButton02 = (Button)findViewById(R.id.Button02);
            
            //设置mButton01的事件监听
            mButton01.setOnClickListener(new Button.OnClickListener() {
                @Override
                public void onClick(View v)
                {
                    // TODO Auto-generated method stub
                    
                    //创建ProgressDialog对象
                    m_pDialog = new ProgressDialog(Activity01.this);

                    // 设置进度条风格，风格为圆形，旋转的
                    m_pDialog.setProgressStyle(ProgressDialog.STYLE_SPINNER);

                    // 设置ProgressDialog 标题
                    m_pDialog.setTitle("提示");
                    
                    // 设置ProgressDialog 提示信息
                    m_pDialog.setMessage("这是一个圆形进度条对话框");

                    // 设置ProgressDialog 标题图标
                    m_pDialog.setIcon(R.drawable.img1);

                    // 设置ProgressDialog 的进度条是否不明确
                    m_pDialog.setIndeterminate(false);
                    
                    // 设置ProgressDialog 是否可以按退回按键取消
                    m_pDialog.setCancelable(true);
                    
                    // 设置ProgressDialog 的一个Button
                    m_pDialog.setButton("确定", new DialogInterface.OnClickListener() {
                        public void onClick(DialogInterface dialog, int i)
                        {
                            //点击“确定按钮”取消对话框
                            dialog.cancel();
                        }
                    });

                    // 让ProgressDialog显示
                    m_pDialog.show();
                }
            });
            
            //设置mButton02的事件监听
            mButton02.setOnClickListener(new Button.OnClickListener() {
                @Override
                public void onClick(View v)
                {
                    // TODO Auto-generated method stub
                    
                    m_count = 0;
                    
                    // 创建ProgressDialog对象
                    m_pDialog = new ProgressDialog(Activity01.this);
                    
                    // 设置进度条风格，风格为长形
                    m_pDialog.setProgressStyle(ProgressDialog.STYLE_HORIZONTAL);
                    
                    // 设置ProgressDialog 标题
                    m_pDialog.setTitle("提示");
                    
                    // 设置ProgressDialog 提示信息
                    m_pDialog.setMessage("这是一个长形对话框进度条");
                    
                    // 设置ProgressDialog 标题图标
                    m_pDialog.setIcon(R.drawable.img2);
                    
                    // 设置ProgressDialog 进度条进度
                    m_pDialog.setProgress(100);
                    
                    // 设置ProgressDialog 的进度条是否不明确
                    m_pDialog.setIndeterminate(false);
                    
                    // 设置ProgressDialog 是否可以按退回按键取消
                    m_pDialog.setCancelable(true);
                    
                    // 让ProgressDialog显示
                    m_pDialog.show();
                    
                    new Thread() 
                    {
                        public void run()
                        {
                            try
                            {
                                while (m_count <= 100)
                                { 
                                    // 由线程来控制进度。
                                    m_pDialog.setProgress(m_count++);
                                    Thread.sleep(100); 
                                }
                                m_pDialog.cancel();
                            }
                            catch (InterruptedException e)
                            {
                                m_pDialog.cancel();
                            }
                        }
                    }.start();
                    
                }
            });
        }
    }


#界面布局

线性布局LinearLayout、相对布局RelativeLayout、表单布局TableLayout、切换卡TabWidget、FrameLayout、AbsoluteLayout 

    <?xml version="1.0" encoding="utf-8"?>
    <TableLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:stretchColumns="1">
        <TableRow>
            <TextView
                android:layout_column="1"
                android:text="打开..."
                android:padding="3dip" />
            <TextView
                android:text="Ctrl-O"
                android:gravity="right"
                android:padding="3dip" />
        </TableRow>
        <TableRow>
            <TextView
                android:layout_column="1"
                android:text="保存..."
                android:padding="3dip" />
            <TextView
                android:text="Ctrl-S"
                android:gravity="right"
                android:padding="3dip" />
        </TableRow>
        <TableRow>
            <TextView
                android:layout_column="1"
                android:text="另存为..."
                android:padding="3dip" />
            <TextView
                android:text="Ctrl-Shift-S"
                android:gravity="right"
                android:padding="3dip" />
        </TableRow>

        <View
            android:layout_height="2dip"
            android:background="#FF909090" />
        <TableRow>
            <TextView
                android:text="*"
                android:padding="3dip" />
            <TextView
                android:text="导入..."
                android:padding="3dip" />
        </TableRow>
        <TableRow>
            <TextView
                android:text="*"
                android:padding="3dip" />
            <TextView
                android:text="导出..."
                android:padding="3dip" />
            <TextView
                android:text="Ctrl-E"
                android:gravity="right"
                android:padding="3dip" />
        </TableRow>
        <View
            android:layout_height="2dip"
            android:background="#FF909090" />

        <TableRow>
            <TextView
                android:layout_column="1"
                android:text="退出"
                android:padding="3dip" />
        </TableRow>
    </TableLayout>

##TabWidget

    #/layout/main.xml
    <?xml version="1.0" encoding="utf-8"?>
    <TabHost xmlns:android="http://schemas.android.com/apk/res/android"
        android:id="@android:id/tabhost"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent">
        <LinearLayout
            android:orientation="vertical"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent">
            <TabWidget
                android:id="@android:id/tabs"
                android:layout_width="fill_parent"
                android:layout_height="wrap_content" />
            <FrameLayout
                android:id="@android:id/tabcontent"
                android:layout_width="fill_parent"
                android:layout_height="fill_parent">
                <TextView 
                    android:id="@+id/textview1"
                    android:layout_width="fill_parent"
                    android:layout_height="fill_parent" 
                    android:text="this is a tab" />
                <TextView 
                    android:id="@+id/textview2"
                    android:layout_width="fill_parent"
                    android:layout_height="fill_parent" 
                    android:text="this is another tab" />
                <TextView 
                    android:id="@+id/textview3"
                    android:layout_width="fill_parent"
                    android:layout_height="fill_parent" 
                    android:text="this is a third tab" />
            </FrameLayout>
        </LinearLayout>
    </TabHost>

    #Activity01.java

    package com.yarin.android.Examples_04_29;

    import android.app.AlertDialog;
    import android.app.Dialog;
    import android.app.TabActivity;
    import android.content.DialogInterface;
    import android.graphics.Color;
    import android.os.Bundle;
    import android.widget.TabHost;
    import android.widget.TabHost.OnTabChangeListener;

    public class Activity01 extends TabActivity
    {
        //声明TabHost对象
        TabHost mTabHost;
        /** Called when the activity is first created. */
        @Override
        public void onCreate(Bundle savedInstanceState)
        {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.main);
            
            //取得TabHost对象
            mTabHost = getTabHost();
            
            /* 为TabHost添加标签 */
            //新建一个newTabSpec(newTabSpec)
            //设置其标签和图标(setIndicator)
            //设置内容(setContent)
            mTabHost.addTab(mTabHost.newTabSpec("tab_test1")
                    .setIndicator("TAB 1",getResources().getDrawable(R.drawable.img1))
                    .setContent(R.id.textview1));
            mTabHost.addTab(mTabHost.newTabSpec("tab_test2")
                    .setIndicator("TAB 2",getResources().getDrawable(R.drawable.img2))
                    .setContent(R.id.textview2));
            mTabHost.addTab(mTabHost.newTabSpec("tab_test3")
                    .setIndicator("TAB 3",getResources().getDrawable(R.drawable.img3))
                    .setContent(R.id.textview3));
            
            //设置TabHost的背景颜色
            mTabHost.setBackgroundColor(Color.argb(150, 22, 70, 150));
            //设置TabHost的背景图片资源
            //mTabHost.setBackgroundResource(R.drawable.bg0);
            
            //设置当前显示哪一个标签
            mTabHost.setCurrentTab(0);
            
            //标签切换事件处理，setOnTabChangedListener 
            mTabHost.setOnTabChangedListener(new OnTabChangeListener()
            {
                // TODO Auto-generated method stub
                @Override
                public void onTabChanged(String tabId) 
                {
                    Dialog dialog = new AlertDialog.Builder(Activity01.this)
                            .setTitle("提示")
                            .setMessage("当前选中"+tabId+"标签")
                            .setPositiveButton("确定",
                            new DialogInterface.OnClickListener() 
                            {
                                public void onClick(DialogInterface dialog, int whichButton)
                                {
                                    dialog.cancel();
                                }
                            }).create();//创建按钮
                  
                    dialog.show();
                }            
            });
        }
    }



#Android数据存储

Android中一共提供了4种数据存储方式：Shared Preferences、Files、SQLite、NetWork。

Shared Preferences存储的数据只能供本应用程序使用，要想让数据共享，就要用其他3种方式。

##Shared Preferences

类似于我们常用的ini文件，用来保存应用程序的一些属性设置，在安卓平台常用于存储简单的参数设置。安装一个应用程序后，在/data/data目录下都会产生一个文件夹，如果应用程序中使用了Preferences，那么便会在该文件夹下产生一个shared_prefs文件夹，其中就是我们保存的数据。
    
    #Activity01.java
    
    package aom.yarin.android.Examples_06_01;

    import android.app.Activity;
    import android.content.SharedPreferences;
    import android.os.Bundle;
    import android.view.KeyEvent;
    import android.widget.TextView;

    public class Activity01 extends Activity
    {

        private MIDIPlayer	mMIDIPlayer	= null;
        private boolean		mbMusic		= false;
        private TextView	mTextView	= null;


        /** Called when the activity is first created. */
        @Override
        public void onCreate(Bundle savedInstanceState)
        {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.main);

            mTextView = (TextView) this.findViewById(R.id.TextView01);

            mMIDIPlayer = new MIDIPlayer(this);

            /* 装载数据 */
            // 取得活动的preferences对象.
            SharedPreferences settings = getPreferences(Activity.MODE_PRIVATE);

            // 取得值.
            mbMusic = settings.getBoolean("bmusic", false);

            if (mbMusic)
            {
                mTextView.setText("当前音乐状态：开");
                mbMusic = true;
                mMIDIPlayer.PlayMusic();
            }
            else
            {
                mTextView.setText("当前音乐状态：关");
            }

        }


        public boolean onKeyUp(int keyCode, KeyEvent event)
        {
            switch (keyCode)
            {
                case KeyEvent.KEYCODE_DPAD_UP:
                    mTextView.setText("当前音乐状态：开");
                    mbMusic = true;
                    mMIDIPlayer.PlayMusic();
                    break;
                case KeyEvent.KEYCODE_DPAD_DOWN:
                    mTextView.setText("当前音乐状态：关");
                    mbMusic = false;
                    mMIDIPlayer.FreeMusic();
                    break;
            }
            return true;
        }


        public boolean onKeyDown(int keyCode, KeyEvent event)
        {
            if (keyCode == KeyEvent.KEYCODE_BACK)
            {
                /* 这里我们在推出应用程序时保存数据 */
                // 取得活动的preferences对象.
                SharedPreferences uiState = getPreferences(0);

                // 取得编辑对象
                SharedPreferences.Editor editor = uiState.edit();

                // 添加值
                editor.putBoolean("bmusic", mbMusic);
                
                // 提交保存
                editor.commit();
                if ( mbMusic )
                {
                    mMIDIPlayer.FreeMusic();
                }
                this.finish();
                return true;
            }
            return super.onKeyDown(keyCode, event);
        }
    }

    #MiDiPlayer.java

    package aom.yarin.android.Examples_06_01;
    import java.io.IOException;
    import android.content.Context;
    import android.media.MediaPlayer;
    public class MIDIPlayer
    {
        public MediaPlayer	playerMusic	= null;
        private Context		mContext	= null;


        public MIDIPlayer(Context context)
        {
            mContext = context;
        }

        /* 播放音乐 */
        public void PlayMusic()
        {
            /* 装载资源中的音乐 */
            playerMusic = MediaPlayer.create(mContext, R.raw.start);
            
            /* 设置是否循环 */
            playerMusic.setLooping(true);
            try
            {
                playerMusic.prepare();
            }
            catch (IllegalStateException e)
            {
                e.printStackTrace();
            }
            catch (IOException e)
            {
                e.printStackTrace();
            }
            playerMusic.start();
        }

        /* 停止并释放音乐 */
        public void FreeMusic()
        {
            if (playerMusic != null)
            {
                playerMusic.stop();
                playerMusic.release();
            }
        }
    }

##数据存储之Files

安卓中可以在设备本身的存储设备或者外接的存储设备中创建用于保存数据的文件，默认状态下，文件是不能在不同的程序间共享的。用文件来存储数据可以通过openFileOutput方法打开一个文件（如果这个文件不存在就自动创建这个文件）。

如果使用绝对路径来存储文件，那么在其他应用程序中一样不能通过这个绝对路径来访问和操作该文件。

    package com.yarin.android.Examples_06_02;

    import java.io.FileInputStream;
    import java.io.FileNotFoundException;
    import java.io.FileOutputStream;
    import java.io.IOException;
    import java.util.Properties;
    import android.app.Activity;
    import android.content.Context;
    import android.os.Bundle;
    import android.view.KeyEvent;
    import android.widget.TextView;

    public class Activity01 extends Activity
    {
        
        private MIDIPlayer	mMIDIPlayer	= null;
        private boolean		mbMusic		= false;
        private TextView	mTextView	= null;
        /** Called when the activity is first created. */
        @Override
        public void onCreate(Bundle savedInstanceState)
        {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.main);

            mTextView = (TextView) this.findViewById(R.id.TextView01);

            mMIDIPlayer = new MIDIPlayer(this);

            /* 读取文件数据  */
            load();

            if (mbMusic)
            {
                mTextView.setText("当前音乐状态：开");
                mbMusic = true;
                mMIDIPlayer.PlayMusic();
            }
            else
            {
                mTextView.setText("当前音乐状态：关");
            }

        }

        public boolean onKeyUp(int keyCode, KeyEvent event)
        {
            switch (keyCode)
            {
                case KeyEvent.KEYCODE_DPAD_UP:
                    mTextView.setText("当前音乐状态：开");
                    mbMusic = true;
                    mMIDIPlayer.PlayMusic();
                    break;
                case KeyEvent.KEYCODE_DPAD_DOWN:
                    mTextView.setText("当前音乐状态：关");
                    mbMusic = false;
                    mMIDIPlayer.FreeMusic();
                    break;
            }
            return true;
        }

        public boolean onKeyDown(int keyCode, KeyEvent event)
        {
            if (keyCode == KeyEvent.KEYCODE_BACK)
            {
                /* 退出应用程序时保存数据 */
                save();
                
                if ( mbMusic )
                {
                    mMIDIPlayer.FreeMusic();
                }
                this.finish();
                return true;
            }
            return super.onKeyDown(keyCode, event);
        }
        
        /* 装载、读取数据 */
        void load()
        {
            /* 构建Properties对对象 */
            Properties properties = new Properties();
            try
            {
                /* 开发文件 */
                FileInputStream stream = this.openFileInput("music.cfg");
                /* 读取文件内容 */
                properties.load(stream);
            }
            catch (FileNotFoundException e)
            {
                return;
            }
            catch (IOException e)
            {
                return;
            }
            /* 取得数据 */
            mbMusic = Boolean.valueOf(properties.get("bmusic").toString());
        }
        
        /* 保存数据 */
        boolean save()
        {
            Properties properties = new Properties();
            
            /* 将数据打包成Properties */
            properties.put("bmusic", String.valueOf(mbMusic));
            try
            {
                FileOutputStream stream = this.openFileOutput("music.cfg", Context.MODE_WORLD_WRITEABLE);
                
                /* 将打包好的数据写入文件中 */
                properties.store(stream, "");
            }
            catch (FileNotFoundException e)
            {
                return false;
            }
            catch (IOException e)
            {
                return false;
            }

            return true;
        }
    }


##数据存储之Network

    #AndroidManifest.xml
    <?xml version="1.0" encoding="utf-8"?>
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
          package="com.yarin.android.Examples_06_04"
          android:versionCode="1"
          android:versionName="1.0">
        <uses-permission android:name="android.permission.INTERNET" />
        <application android:icon="@drawable/icon" android:label="@string/app_name">
            <activity android:name=".Activity01"
                      android:label="@string/app_name">
                <intent-filter>
                    <action android:name="android.intent.action.MAIN" />
                    <category android:name="android.intent.category.LAUNCHER" />
                </intent-filter>
            </activity>
        </application>
        <uses-sdk android:minSdkVersion="5" />
    </manifest> 

    #Activity01.java
    package com.yarin.android.Examples_06_04;

    import java.io.BufferedInputStream;
    import java.io.InputStream;
    import java.net.URL;
    import java.net.URLConnection;
    import org.apache.http.util.ByteArrayBuffer;
    import android.app.Activity;
    import android.os.Bundle;
    import android.widget.TextView;

    public class Activity01 extends Activity
    {
        /** Called when the activity is first created. */
        @Override
        public void onCreate(Bundle savedInstanceState)
        {
            super.onCreate(savedInstanceState);
            
            setContentView(R.layout.main);

            TextView tv = new TextView(this);
            
            String myString = null;
            
            try
            {
                /* 定义我们要访问的地址url */
                URL uri = new URL("http://192.168.1.110:8080/android.txt");
                
                /* 打开这个url连接 */
                URLConnection ucon = uri.openConnection();
                
                /* 从上面的链接中取得InputStream */
                InputStream is = ucon.getInputStream();
                
                BufferedInputStream bis = new BufferedInputStream(is);
                ByteArrayBuffer baf = new ByteArrayBuffer(100);
                int current = 0;
                /* 一直读到文件结束 */
                while ((current = bis.read()) != -1)
                {
                    baf.append((byte) current);
                }
                myString = new String(baf.toByteArray());
            }
            catch (Exception e)
            {

                myString = e.getMessage();
            }
            /* 将信息设置到TextView */
            tv.setText(myString);
            
            /* 将TextView显示到屏幕上 */
            this.setContentView(tv);

        }
    }

##Android数据库编程

前面3种方式都是存储一些简单的、数据量较小的数据，如果需要对大量的数据进行存储、管理以及升级维护，可以使用SQLite数据库。

SQLite数据库是由c语言编写的开源嵌入式数据库，支持的数据库大小为2TB.特征如下：

 - 轻量级。属于进程内的数据库引擎，不存在数据库的客户端和服务器。
 - 独立性。
 - 隔离性。
 - 跨平台。
 - 多语言接口。

 - 安全性。通过数据库级上的独占性和共享锁来实现独立事务处理。意味着多个进程可以在同一时间从同一数据库读取数据，但只有一个可以写入数据。在某个进程或线程向数据库执行写操作之前，必须获得独占锁定。在发出独占锁定后，其他的读写操作将不会发生。


    package com.yarin.android.Examples_06_05;

    import android.app.Activity;
    import android.content.ContentValues;
    import android.database.Cursor;
    import android.database.sqlite.SQLiteDatabase;
    import android.graphics.Color;
    import android.os.Bundle;
    import android.view.KeyEvent;
    import android.widget.LinearLayout;
    import android.widget.ListAdapter;
    import android.widget.ListView;
    import android.widget.SimpleCursorAdapter;

    public class Activity01 extends Activity
    {
        private static int			miCount			= 0;

        /* 数据库对象 */
        private SQLiteDatabase		mSQLiteDatabase	= null;

        /* 数据库名 */
        private final static String	DATABASE_NAME	= "Examples_06_05.db";
        /* 表名 */
        private final static String	TABLE_NAME		= "table1";

        /* 表中的字段 */
        private final static String	TABLE_ID		= "_id";
        private final static String	TABLE_NUM		= "num";
        private final static String	TABLE_DATA		= "data";

        /* 创建表的sql语句 */
        private final static String	CREATE_TABLE	= "CREATE TABLE " + TABLE_NAME + " (" + TABLE_ID + " INTEGER PRIMARY KEY," + TABLE_NUM + " INTERGER,"+ TABLE_DATA + " TEXT)";

        /* 线性布局 */
        LinearLayout				m_LinearLayout	= null;
        /* 列表视图-显示数据库中的数据 */
        ListView					m_ListView		= null;


        /** Called when the activity is first created. */
        @Override
        public void onCreate(Bundle savedInstanceState)
        {
            super.onCreate(savedInstanceState);

            /* 创建LinearLayout布局对象 */
            m_LinearLayout = new LinearLayout(this);
            /* 设置布局LinearLayout的属性 */
            m_LinearLayout.setOrientation(LinearLayout.VERTICAL);
            m_LinearLayout.setBackgroundColor(android.graphics.Color.BLACK);

            /* 创建ListView对象 */
            m_ListView = new ListView(this);
            LinearLayout.LayoutParams param = new LinearLayout.LayoutParams(LinearLayout.LayoutParams.FILL_PARENT, LinearLayout.LayoutParams.WRAP_CONTENT);
            m_ListView.setBackgroundColor(Color.BLACK);

            /* 添加m_ListView到m_LinearLayout布局 */
            m_LinearLayout.addView(m_ListView, param);

            /* 设置显示m_LinearLayout布局 */
            setContentView(m_LinearLayout);

            // 打开已经存在的数据库
            mSQLiteDatabase = this.openOrCreateDatabase(DATABASE_NAME, MODE_PRIVATE, null);

            // 获取数据库Phones的Cursor
            try
            {
                /* 在数据库mSQLiteDatabase中创建一个表 */
                mSQLiteDatabase.execSQL(CREATE_TABLE);
            }
            catch (Exception e)
            {
                UpdataAdapter();
            }
        }


        public boolean onKeyUp(int keyCode, KeyEvent event)
        {
            switch (keyCode)
            {
                case KeyEvent.KEYCODE_DPAD_LEFT:
                    AddData();
                    break;
                case KeyEvent.KEYCODE_DPAD_RIGHT:
                    DeleteData();
                    break;
                case KeyEvent.KEYCODE_1:
                    UpData();
                    break;
                case KeyEvent.KEYCODE_2:
                    DeleteTable();
                    break;
                case KeyEvent.KEYCODE_3:
                    DeleteDataBase();
                    break;
            }
            return true;
        }


        /* 删除数据库 */
        public void DeleteDataBase()
        {
            this.deleteDatabase(DATABASE_NAME);
            this.finish();
        }


        /* 删除一个表 */
        public void DeleteTable()
        {
            mSQLiteDatabase.execSQL("DROP TABLE " + TABLE_NAME);
            this.finish();
        }


        /* 更新一条数据 */
        public void UpData()
        {
            ContentValues cv = new ContentValues();
            cv.put(TABLE_NUM, miCount);
            cv.put(TABLE_DATA, "修改后的数据" + miCount);

            /* 更新数据 */
            mSQLiteDatabase.update(TABLE_NAME, cv, TABLE_NUM + "=" + Integer.toString(miCount - 1), null);

            UpdataAdapter();
        }


        /* 向表中添加一条数据 */
        public void AddData()
        {
            ContentValues cv = new ContentValues();
            cv.put(TABLE_NUM, miCount);
            cv.put(TABLE_DATA, "测试数据库数据" + miCount);
            /* 插入数据 */
            mSQLiteDatabase.insert(TABLE_NAME, null, cv);
            miCount++;
            UpdataAdapter();
        }


        /* 从表中删除指定的一条数据 */
        public void DeleteData()
        {

            /* 删除数据 */
            mSQLiteDatabase.execSQL("DELETE FROM " + TABLE_NAME + " WHERE _id=" + Integer.toString(miCount));

            miCount--;
            if (miCount < 0)
            {
                miCount = 0;
            }
            UpdataAdapter();
        }


        /* 更行试图显示 */
        public void UpdataAdapter()
        {
            // 获取数据库Phones的Cursor
            Cursor cur = mSQLiteDatabase.query(TABLE_NAME, new String[] { TABLE_ID, TABLE_NUM, TABLE_DATA }, null, null, null, null, null);

            miCount = cur.getCount();
            if (cur != null && cur.getCount() >= 0)
            {
                // ListAdapter是ListView和后台数据的桥梁
                ListAdapter adapter = new SimpleCursorAdapter(this,
                // 定义List中每一行的显示模板
                    // 表示每一行包含两个数据项
                    android.R.layout.simple_list_item_2,
                    // 数据库的Cursor对象
                    cur,
                    // 从数据库的TABLE_NUM和TABLE_DATA两列中取数据
                    new String[] { TABLE_NUM, TABLE_DATA },
                    // 与NAME和NUMBER对应的Views
                    new int[] { android.R.id.text1, android.R.id.text2 });

                /* 将adapter添加到m_ListView中 */
                m_ListView.setAdapter(adapter);
            }
        }


        /* 按键事件处理 */
        public boolean onKeyDown(int keyCode, KeyEvent event)
        {
            if (keyCode == KeyEvent.KEYCODE_BACK)
            {
                /* 退出时，不要忘记关闭 */
                mSQLiteDatabase.close();
                this.finish();
                return true;
            }
            return super.onKeyDown(keyCode, event);
        }
    }

##SQLiteOpenHelper

    #MyDataBaseAdapter.java
    
    package com.yarin.android.Examples_06_06;

    import android.content.ContentValues;
    import android.content.Context;
    import android.database.Cursor;
    import android.database.SQLException;
    import android.database.sqlite.SQLiteDatabase;
    import android.database.sqlite.SQLiteOpenHelper;

    public class MyDataBaseAdapter
    {
        // 用于打印log
        private static final String	TAG				= "MyDataBaseAdapter";

        // 表中一条数据的名称
        public static final String	KEY_ID		= "_id";												

        // 表中一条数据的内容
        public static final String	KEY_NUM		= "num";												

        // 表中一条数据的id
        public static final String	KEY_DATA		= "data";

        // 数据库名称为data
        private static final String	DB_NAME			= "Examples_06_06.db";
        
        // 数据库表名
        private static final String	DB_TABLE		= "table1";
        
        // 数据库版本
        private static final int	DB_VERSION		= 1;

        // 本地Context对象
        private Context				mContext		= null;
        
        //创建一个表
        private static final String	DB_CREATE		= "CREATE TABLE " + DB_TABLE + " (" + KEY_ID + " INTEGER PRIMARY KEY," + KEY_NUM + " INTERGER,"+ KEY_DATA + " TEXT)";

        // 执行open（）打开数据库时，保存返回的数据库对象
        private SQLiteDatabase		mSQLiteDatabase	= null;

        // 由SQLiteOpenHelper继承过来
        private DatabaseHelper		mDatabaseHelper	= null;
        
        
        private static class DatabaseHelper extends SQLiteOpenHelper
        {
            /* 构造函数-创建一个数据库 */
            DatabaseHelper(Context context)
            {
                //当调用getWritableDatabase() 
                //或 getReadableDatabase()方法时
                //则创建一个数据库
                super(context, DB_NAME, null, DB_VERSION);
                
                
            }

            /* 创建一个表 */
            @Override
            public void onCreate(SQLiteDatabase db)
            {
                // 数据库没有表时创建一个
                db.execSQL(DB_CREATE);
            }

            /* 升级数据库 */
            @Override
            public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion)
            {
                db.execSQL("DROP TABLE IF EXISTS notes");
                onCreate(db);
            }
        }
        
        /* 构造函数-取得Context */
        public MyDataBaseAdapter(Context context)
        {
            mContext = context;
        }


        // 打开数据库，返回数据库对象
        public void open() throws SQLException
        {
            mDatabaseHelper = new DatabaseHelper(mContext);
            mSQLiteDatabase = mDatabaseHelper.getWritableDatabase();
        }


        // 关闭数据库
        public void close()
        {
            mDatabaseHelper.close();
        }

        /* 插入一条数据 */
        public long insertData(int num, String data)
        {
            ContentValues initialValues = new ContentValues();
            initialValues.put(KEY_NUM, num);
            initialValues.put(KEY_DATA, data);

            return mSQLiteDatabase.insert(DB_TABLE, KEY_ID, initialValues);
        }

        /* 删除一条数据 */
        public boolean deleteData(long rowId)
        {
            return mSQLiteDatabase.delete(DB_TABLE, KEY_ID + "=" + rowId, null) > 0;
        }

        /* 通过Cursor查询所有数据 */
        public Cursor fetchAllData()
        {
            return mSQLiteDatabase.query(DB_TABLE, new String[] { KEY_ID, KEY_NUM, KEY_DATA }, null, null, null, null, null);
        }

        /* 查询指定数据 */
        public Cursor fetchData(long rowId) throws SQLException
        {

            Cursor mCursor =

            mSQLiteDatabase.query(true, DB_TABLE, new String[] { KEY_ID, KEY_NUM, KEY_DATA }, KEY_ID + "=" + rowId, null, null, null, null, null);

            if (mCursor != null)
            {
                mCursor.moveToFirst();
            }
            return mCursor;

        }

        /* 更新一条数据 */
        public boolean updateData(long rowId, int num, String data)
        {
            ContentValues args = new ContentValues();
            args.put(KEY_NUM, num);
            args.put(KEY_DATA, data);

            return mSQLiteDatabase.update(DB_TABLE, args, KEY_ID + "=" + rowId, null) > 0;
        }
        
    }


    #Activity01.java
    package com.yarin.android.Examples_06_06;

    import android.app.Activity;
    import android.database.Cursor;
    import android.graphics.Color;
    import android.os.Bundle;
    import android.view.KeyEvent;
    import android.widget.LinearLayout;
    import android.widget.ListAdapter;
    import android.widget.ListView;
    import android.widget.SimpleCursorAdapter;

    public class Activity01 extends Activity
    {
        private static int			miCount			= 0;
        
        /* 线性布局 */
        LinearLayout				m_LinearLayout	= null;
        /* 列表视图-显示数据库中的数据 */
        ListView					m_ListView		= null;
        
        MyDataBaseAdapter m_MyDataBaseAdapter;
        /** Called when the activity is first created. */
        @Override
        public void onCreate(Bundle savedInstanceState)
        {
            super.onCreate(savedInstanceState);
            /* 创建LinearLayout布局对象 */
            m_LinearLayout = new LinearLayout(this);
            /* 设置布局LinearLayout的属性 */
            m_LinearLayout.setOrientation(LinearLayout.VERTICAL);
            m_LinearLayout.setBackgroundColor(android.graphics.Color.BLACK);

            /* 创建ListView对象 */
            m_ListView = new ListView(this);
            LinearLayout.LayoutParams param = new LinearLayout.LayoutParams(LinearLayout.LayoutParams.FILL_PARENT, LinearLayout.LayoutParams.WRAP_CONTENT);
            m_ListView.setBackgroundColor(Color.BLACK);

            /* 添加m_ListView到m_LinearLayout布局 */
            m_LinearLayout.addView(m_ListView, param);

            /* 设置显示m_LinearLayout布局 */
            setContentView(m_LinearLayout);
            
            /* 构造MyDataBaseAdapter对象 */
            m_MyDataBaseAdapter = new MyDataBaseAdapter(this);
            
            /* 取得数据库对象 */
            m_MyDataBaseAdapter.open();
            
            UpdataAdapter();
        }
        
        public boolean onKeyUp(int keyCode, KeyEvent event)
        {
            switch (keyCode)
            {
                case KeyEvent.KEYCODE_DPAD_LEFT:
                    AddData();
                    break;
                case KeyEvent.KEYCODE_DPAD_RIGHT:
                    DeleteData();
                    break;
                case KeyEvent.KEYCODE_1:
                    UpData();
                    break;
            }
            return true;
        }
        
        /* 更新一条数据 */
        public void UpData()
        {	
            m_MyDataBaseAdapter.updateData(miCount - 1, miCount, "修改后的数据" + miCount);

            UpdataAdapter();
        }

        /* 向表中添加一条数据 */
        public void AddData()
        {
            m_MyDataBaseAdapter.insertData(miCount, "测试数据库数据" + miCount);
            miCount++;
            UpdataAdapter();
        }

        /* 从表中删除指定的一条数据 */
        public void DeleteData()
        {

            /* 删除数据 */
            m_MyDataBaseAdapter.deleteData(miCount);
            miCount--;
            if (miCount < 0)
            {
                miCount = 0;
            }
            UpdataAdapter();
        }
        
        /* 按键事件处理 */
        public boolean onKeyDown(int keyCode, KeyEvent event)
        {
            if (keyCode == KeyEvent.KEYCODE_BACK)
            {
                /* 退出时，不要忘记关闭 */
                m_MyDataBaseAdapter.close();
                this.finish();
                return true;
            }
            return super.onKeyDown(keyCode, event);
        }
        
        /* 更行试图显示 */
        public void UpdataAdapter()
        {
            // 获取数据库Phones的Cursor
            Cursor cur = m_MyDataBaseAdapter.fetchAllData();

            miCount = cur.getCount();
            if (cur != null && cur.getCount() >= 0)
            {
                // ListAdapter是ListView和后台数据的桥梁
                ListAdapter adapter = new SimpleCursorAdapter(this,
                // 定义List中每一行的显示模板
                    // 表示每一行包含两个数据项
                    android.R.layout.simple_list_item_2,
                    // 数据库的Cursor对象
                    cur,
                    // 从数据库的TABLE_NUM和TABLE_DATA两列中取数据
                    new String[] {MyDataBaseAdapter.KEY_NUM, MyDataBaseAdapter.KEY_DATA },
                    // 与NAME和NUMBER对应的Views
                    new int[] { android.R.id.text1, android.R.id.text2 });

                /* 将adapter添加到m_ListView中 */
                m_ListView.setAdapter(adapter);
            }
        }
    }

#数据共享Content Provides

    #NotePad.java
    package com.yarin.android.Examples_06_07;

    import android.net.Uri;
    import android.provider.BaseColumns;

    public class NotePad
    {
        //ContentProvider的uri
        public static final String	AUTHORITY	= "com.google.provider.NotePad";

        private NotePad(){}

        // 定义基本字段
        public static final class Notes implements BaseColumns
        {
            private Notes(){}

            public static final Uri		CONTENT_URI			= Uri.parse("content://" + AUTHORITY + "/notes");

            // 新的MIME类型-多个
            public static final String	CONTENT_TYPE		= "vnd.android.cursor.dir/vnd.google.note";

            // 新的MIME类型-单个
            public static final String	CONTENT_ITEM_TYPE	= "vnd.android.cursor.item/vnd.google.note";

            public static final String	DEFAULT_SORT_ORDER	= "modified DESC";

            //字段
            public static final String	TITLE				= "title";
            public static final String	NOTE				= "note";
            public static final String	CREATEDDATE		= "created";
            public static final String	MODIFIEDDATE		= "modified";
        }
    }


    #NotePadProvider.java

    package com.yarin.android.Examples_06_07;

    import java.util.HashMap;

    import android.content.ContentProvider;
    import android.content.ContentUris;
    import android.content.ContentValues;
    import android.content.Context;
    import android.content.UriMatcher;
    import android.content.res.Resources;
    import android.database.Cursor;
    import android.database.SQLException;
    import android.database.sqlite.SQLiteDatabase;
    import android.database.sqlite.SQLiteOpenHelper;
    import android.database.sqlite.SQLiteQueryBuilder;
    import android.net.Uri;
    import android.text.TextUtils;

    import com.yarin.android.Examples_06_07.NotePad.Notes;

    public class NotePadProvider extends ContentProvider
    {
        private static final String				TAG					= "NotePadProvider";
        // 数据库名
        private static final String				DATABASE_NAME		= "note_pad.db";
        private static final int				DATABASE_VERSION	= 2;
        // 表名
        private static final String				NOTES_TABLE_NAME	= "notes";
        private static HashMap<String, String>	sNotesProjectionMap;
        private static final int				NOTES				= 1;
        private static final int				NOTE_ID				= 2;
        private static final UriMatcher			sUriMatcher;
        private DatabaseHelper	mOpenHelper;
        //创建表SQL语句
        private static final String				CREATE_TABLE="CREATE TABLE " 
                                                            + NOTES_TABLE_NAME 
                                                            + " (" + Notes._ID 
                                                            + " INTEGER PRIMARY KEY," 
                                                            + Notes.TITLE 
                                                            + " TEXT," 
                                                            + Notes.NOTE 
                                                            + " TEXT,"
                                                            + Notes.CREATEDDATE 
                                                            + " INTEGER," 
                                                            + Notes.MODIFIEDDATE 
                                                            + " INTEGER" + ");";
        
        static
        {
            sUriMatcher = new UriMatcher(UriMatcher.NO_MATCH);
            sUriMatcher.addURI(NotePad.AUTHORITY, "notes", NOTES);
            sUriMatcher.addURI(NotePad.AUTHORITY, "notes/#", NOTE_ID);

            sNotesProjectionMap = new HashMap<String, String>();
            sNotesProjectionMap.put(Notes._ID, Notes._ID);
            sNotesProjectionMap.put(Notes.TITLE, Notes.TITLE);
            sNotesProjectionMap.put(Notes.NOTE, Notes.NOTE);
            sNotesProjectionMap.put(Notes.CREATEDDATE, Notes.CREATEDDATE);
            sNotesProjectionMap.put(Notes.MODIFIEDDATE, Notes.MODIFIEDDATE);
        }
        private static class DatabaseHelper extends SQLiteOpenHelper
        {
            //构造函数-创建数据库
            DatabaseHelper(Context context)
            {
                super(context, DATABASE_NAME, null, DATABASE_VERSION);
            }
            //创建表
            @Override
            public void onCreate(SQLiteDatabase db)
            {
                db.execSQL(CREATE_TABLE);
            }
            //更新数据库
            @Override
            public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion)
            {
                db.execSQL("DROP TABLE IF EXISTS notes");
                onCreate(db);
            }
        }
        @Override
        public boolean onCreate()
        {
            mOpenHelper = new DatabaseHelper(getContext());
            return true;
        }
        @Override
        //查询操作
        public Cursor query(Uri uri, String[] projection, String selection, String[] selectionArgs, String sortOrder)
        {
            SQLiteQueryBuilder qb = new SQLiteQueryBuilder();
            switch (sUriMatcher.match(uri))
            {
                case NOTES:
                    qb.setTables(NOTES_TABLE_NAME);
                    qb.setProjectionMap(sNotesProjectionMap);
                    break;

                case NOTE_ID:
                    qb.setTables(NOTES_TABLE_NAME);
                    qb.setProjectionMap(sNotesProjectionMap);
                    qb.appendWhere(Notes._ID + "=" + uri.getPathSegments().get(1));
                    break;

                default:
                    throw new IllegalArgumentException("Unknown URI " + uri);
            }
            String orderBy;
            if (TextUtils.isEmpty(sortOrder))
            {
                orderBy = NotePad.Notes.DEFAULT_SORT_ORDER;
            }
            else
            {
                orderBy = sortOrder;
            }
            SQLiteDatabase db = mOpenHelper.getReadableDatabase();
            Cursor c = qb.query(db, projection, selection, selectionArgs, null, null, orderBy);
            c.setNotificationUri(getContext().getContentResolver(), uri);
            return c;
        }
        @Override
        // 如果有自定义类型，必须实现该方法
        public String getType(Uri uri)
        {
            switch (sUriMatcher.match(uri))
            {
                case NOTES:
                    return Notes.CONTENT_TYPE;

                case NOTE_ID:
                    return Notes.CONTENT_ITEM_TYPE;

                default:
                    throw new IllegalArgumentException("Unknown URI " + uri);
            }
        }
        @Override
        //插入数据库
        public Uri insert(Uri uri, ContentValues initialValues)
        {
            if (sUriMatcher.match(uri) != NOTES)
            {
                throw new IllegalArgumentException("Unknown URI " + uri);
            }
            ContentValues values;
            if (initialValues != null)
            {
                values = new ContentValues(initialValues);
            }
            else
            {
                values = new ContentValues();
            }
            Long now = Long.valueOf(System.currentTimeMillis());

            if (values.containsKey(NotePad.Notes.CREATEDDATE) == false)
            {
                values.put(NotePad.Notes.CREATEDDATE, now);
            }
            if (values.containsKey(NotePad.Notes.MODIFIEDDATE) == false)
            {
                values.put(NotePad.Notes.MODIFIEDDATE, now);
            }
            if (values.containsKey(NotePad.Notes.TITLE) == false)
            {
                Resources r = Resources.getSystem();
                values.put(NotePad.Notes.TITLE, r.getString(android.R.string.untitled));
            }
            if (values.containsKey(NotePad.Notes.NOTE) == false)
            {
                values.put(NotePad.Notes.NOTE, "");
            }
            SQLiteDatabase db = mOpenHelper.getWritableDatabase();
            long rowId = db.insert(NOTES_TABLE_NAME, Notes.NOTE, values);
            if (rowId > 0)
            {
                Uri noteUri = ContentUris.withAppendedId(NotePad.Notes.CONTENT_URI, rowId);
                getContext().getContentResolver().notifyChange(noteUri, null);
                return noteUri;
            }
            throw new SQLException("Failed to insert row into " + uri);
        }
        @Override
        //删除数据
        public int delete(Uri uri, String where, String[] whereArgs)
        {
            SQLiteDatabase db = mOpenHelper.getWritableDatabase();
            int count;
            switch (sUriMatcher.match(uri))
            {
                case NOTES:
                    count = db.delete(NOTES_TABLE_NAME, where, whereArgs);
                    break;

                case NOTE_ID:
                    String noteId = uri.getPathSegments().get(1);
                    count = db.delete(NOTES_TABLE_NAME, Notes._ID + "=" + noteId + (!TextUtils.isEmpty(where) ? " AND (" + where + ')' : ""), whereArgs);
                    break;

                default:
                    throw new IllegalArgumentException("Unknown URI " + uri);
            }
            getContext().getContentResolver().notifyChange(uri, null);
            return count;
        }
        @Override
        //更新数据
        public int update(Uri uri, ContentValues values, String where, String[] whereArgs)
        {
            SQLiteDatabase db = mOpenHelper.getWritableDatabase();
            int count;
            switch (sUriMatcher.match(uri))
            {
                case NOTES:
                    count = db.update(NOTES_TABLE_NAME, values, where, whereArgs);
                    break;

                case NOTE_ID:
                    String noteId = uri.getPathSegments().get(1);
                    count = db.update(NOTES_TABLE_NAME, values, Notes._ID + "=" + noteId + (!TextUtils.isEmpty(where) ? " AND (" + where + ')' : ""), whereArgs);
                    break;

                default:
                    throw new IllegalArgumentException("Unknown URI " + uri);
            }
            getContext().getContentResolver().notifyChange(uri, null);
            return count;
        }
    }

    #Activity01.java

    package com.yarin.android.Examples_06_07;

    import android.app.Activity;
    import android.content.ContentValues;
    import android.database.Cursor;
    import android.net.Uri;
    import android.os.Bundle;
    import android.view.Gravity;
    import android.widget.Toast;

    public class Activity01 extends Activity
    {
        /** Called when the activity is first created. */
        @Override
        public void onCreate(Bundle savedInstanceState)
        {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.main);
            
            /* 插入数据 */
            ContentValues values = new ContentValues();
            values.put(NotePad.Notes.TITLE, "title1");
            values.put(NotePad.Notes.NOTE, "NOTENOTE1");
            getContentResolver().insert(NotePad.Notes.CONTENT_URI, values);

            values.clear();
            values.put(NotePad.Notes.TITLE, "title2");
            values.put(NotePad.Notes.NOTE, "NOTENOTE2");
            getContentResolver().insert(NotePad.Notes.CONTENT_URI, values);
            
            /* 显示 */
            displayNote();
        }
        
        private void displayNote()
        {
            String columns[] = new String[] { NotePad.Notes._ID, 
                                              NotePad.Notes.TITLE, 
                                              NotePad.Notes.NOTE, 
                                              NotePad.Notes.CREATEDDATE, 
                                              NotePad.Notes.MODIFIEDDATE };
            Uri myUri = NotePad.Notes.CONTENT_URI;
            Cursor cur = managedQuery(myUri, columns, null, null, null);
            if (cur.moveToFirst())
            {
                String id = null;
                String title = null;
                do
                {
                    id = cur.getString(cur.getColumnIndex(NotePad.Notes._ID));
                    title = cur.getString(cur.getColumnIndex(NotePad.Notes.TITLE));
                    Toast toast=Toast.makeText(this, "TITLE："+id + "NOTE：" + title, Toast.LENGTH_LONG);
                    toast.setGravity(Gravity.TOP|Gravity.CENTER, 0, 40);
                    toast.show();
                }
                while (cur.moveToNext());
            }
        }

    }

#闹钟设置

    #AndroidManifest.xml
    <?xml version="1.0" encoding="utf-8"?>
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
          package="com.yarin.android.Examples_07_07"
          android:versionCode="1"
          android:versionName="1.0">
        <application android:icon="@drawable/icon" android:label="@string/app_name">
            <receiver android:name=".AlarmReceiver" android:process=":remote" />
            <activity android:name=".Activity01"
                      android:label="@string/app_name">
                <intent-filter>
                    <action android:name="android.intent.action.MAIN" />
                    <category android:name="android.intent.category.LAUNCHER" />
                </intent-filter>
            </activity>
        </application>
        <uses-sdk android:minSdkVersion="5" />
    </manifest> 


    #AlarmReceiver.java
    package com.yarin.android.Examples_07_07;

    import android.content.BroadcastReceiver;
    import android.content.Context;
    import android.content.Intent;
    import android.widget.Toast;

    public class AlarmReceiver extends BroadcastReceiver
    {
        public void onReceive(Context context, Intent intent)
        {
            Toast.makeText(context, "你设置的闹钟时间到了", Toast.LENGTH_LONG).show();
        }
    }

    #Activity01.java

    package com.yarin.android.Examples_07_07;

    import java.util.Calendar;

    import android.app.Activity;
    import android.app.AlarmManager;
    import android.app.PendingIntent;
    import android.app.TimePickerDialog;
    import android.content.Intent;
    import android.os.Bundle;
    import android.view.View;
    import android.widget.Button;
    import android.widget.TextView;
    import android.widget.TimePicker;

    public class Activity01 extends Activity
    {
        Button	mButton1;
        Button	mButton2;
        
        TextView mTextView;
        
        Calendar calendar;
        /** Called when the activity is first created. */
        @Override
        public void onCreate(Bundle savedInstanceState)
        {
            super.onCreate(savedInstanceState);
            
            setContentView(R.layout.main);
            calendar=Calendar.getInstance();
            
            mTextView=(TextView)findViewById(R.id.TextView01);
            mButton1=(Button)findViewById(R.id.Button01);
            mButton2=(Button)findViewById(R.id.Button02);
            
            mButton1.setOnClickListener(new View.OnClickListener()
            {
              public void onClick(View v)
              {
                calendar.setTimeInMillis(System.currentTimeMillis());
                int mHour=calendar.get(Calendar.HOUR_OF_DAY);
                int mMinute=calendar.get(Calendar.MINUTE);
                new TimePickerDialog(Activity01.this,
                  new TimePickerDialog.OnTimeSetListener()
                  {                
                    public void onTimeSet(TimePicker view,int hourOfDay,int minute)
                    {
                      calendar.setTimeInMillis(System.currentTimeMillis());
                      calendar.set(Calendar.HOUR_OF_DAY,hourOfDay);
                      calendar.set(Calendar.MINUTE,minute);
                      calendar.set(Calendar.SECOND,0);
                      calendar.set(Calendar.MILLISECOND,0);
                      /* 建立Intent和PendingIntent，来调用目标组件 */
                      Intent intent = new Intent(Activity01.this, AlarmReceiver.class);
                      PendingIntent pendingIntent=PendingIntent.getBroadcast(Activity01.this,0, intent, 0);
                      AlarmManager am;
                      /* 获取闹钟管理的实例 */
                      am = (AlarmManager)getSystemService(ALARM_SERVICE);
                      /* 设置闹钟 */
                      am.set(AlarmManager.RTC_WAKEUP, calendar.getTimeInMillis(), pendingIntent);
                      /* 设置周期闹 */
                      am.setRepeating(AlarmManager.RTC_WAKEUP, System.currentTimeMillis() + (10*1000), (24*60*60*1000), pendingIntent); 
                      String tmpS="设置闹钟时间为"+format(hourOfDay)+":"+format(minute);
                      mTextView.setText(tmpS);
                    }          
                  },mHour,mMinute,true).show();
              }
            });
            
            mButton2.setOnClickListener(new View.OnClickListener()
            {
              public void onClick(View v)
              {
                Intent intent = new Intent(Activity01.this, AlarmReceiver.class);
                PendingIntent pendingIntent=PendingIntent.getBroadcast(Activity01.this,0, intent, 0);
                AlarmManager am;
                /* 获取闹钟管理的实例 */
                am =(AlarmManager)getSystemService(ALARM_SERVICE);
                /* 取消 */
                am.cancel(pendingIntent);
                mTextView.setText("闹钟已取消！");
              }
            });
        }		
        /* 格式化字符串(7:3->07:03) */
        private String format(int x)
        {
            String s = "" + x;
            if (s.length() == 1)
                s = "0" + s;
            return s;
        }
    }

#铃声设置

    #AndroidManifest.xml
    <?xml version="1.0" encoding="utf-8"?>
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
          package="com.yarin.android.Examples_07_08"
          android:versionCode="1"
          android:versionName="1.0">
        <application android:icon="@drawable/icon" android:label="@string/app_name">
            <activity android:name=".Activity01"
                      android:label="@string/app_name">
                <intent-filter>
                    <action android:name="android.intent.action.MAIN" />
                    <category android:name="android.intent.category.LAUNCHER" />
                </intent-filter>
            </activity>
        </application>
        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS"/>
        <uses-sdk android:minSdkVersion="5" />
    </manifest> 

    #Activity01.java

    package com.yarin.android.Examples_07_08;

    import java.io.File;

    import android.app.Activity;
    import android.content.Intent;
    import android.media.RingtoneManager;
    import android.net.Uri;
    import android.os.Bundle;
    import android.view.View;
    import android.widget.Button;
    public class Activity01 extends Activity
    {
        /* 3个按钮 */
        private Button mButtonRingtone;
        private Button mButtonAlarm;
        private Button mButtonNotification;

        /* 自定义的类型 */
        public static final int ButtonRingtone			= 0;
        public static final int ButtonAlarm				= 1;
        public static final int ButtonNotification		= 2;
        /* 铃声文件夹 */
        private String strRingtoneFolder = "/sdcard/music/ringtones";
        private String strAlarmFolder = "/sdcard/music/alarms";
        private String strNotificationFolder = "/sdcard/music/notifications";


        /** Called when the activity is first created. */
        @Override
        public void onCreate(Bundle savedInstanceState)
        {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.main);
        
            mButtonRingtone = (Button) findViewById(R.id.ButtonRingtone);
            mButtonAlarm = (Button) findViewById(R.id.ButtonAlarm);
            mButtonNotification = (Button) findViewById(R.id.ButtonNotification);
            /* 设置来电铃声 */
            mButtonRingtone.setOnClickListener(new Button.OnClickListener() 
            {
                @Override
                public void onClick(View arg0)
                {
                    if (bFolder(strRingtoneFolder))
                    {
                        //打开系统铃声设置
                        Intent intent = new Intent(RingtoneManager.ACTION_RINGTONE_PICKER);
                        //类型为来电RINGTONE
                        intent.putExtra(RingtoneManager.EXTRA_RINGTONE_TYPE, RingtoneManager.TYPE_RINGTONE);
                        //设置显示的title
                        intent.putExtra(RingtoneManager.EXTRA_RINGTONE_TITLE, "设置来电铃声");
                        //当设置完成之后返回到当前的Activity
                        startActivityForResult(intent, ButtonRingtone);
                    }
                }
            });
            /* 设置闹钟铃声 */
            mButtonAlarm.setOnClickListener(new Button.OnClickListener() 
            {
                @Override
                public void onClick(View arg0)
                {
                    if (bFolder(strAlarmFolder))
                    {
                        //打开系统铃声设置
                        Intent intent = new Intent(RingtoneManager.ACTION_RINGTONE_PICKER);
                        //设置铃声类型和title
                        intent.putExtra(RingtoneManager.EXTRA_RINGTONE_TYPE, RingtoneManager.TYPE_ALARM);
                        intent.putExtra(RingtoneManager.EXTRA_RINGTONE_TITLE, "设置闹铃铃声");
                        //当设置完成之后返回到当前的Activity
                        startActivityForResult(intent, ButtonAlarm);
                    }
                }
            });
            /* 设置通知铃声 */
            mButtonNotification.setOnClickListener(new Button.OnClickListener() 
            {
                @Override
                public void onClick(View arg0)
                {
                    if (bFolder(strNotificationFolder))
                    {
                        //打开系统铃声设置
                        Intent intent = new Intent(RingtoneManager.ACTION_RINGTONE_PICKER);
                        //设置铃声类型和title
                        intent.putExtra(RingtoneManager.EXTRA_RINGTONE_TYPE, RingtoneManager.TYPE_NOTIFICATION);
                        intent.putExtra(RingtoneManager.EXTRA_RINGTONE_TITLE, "设置通知铃声");
                        //当设置完成之后返回到当前的Activity
                        startActivityForResult(intent, ButtonNotification);
                    }
                }
            });
        }
        /* 当设置铃声之后的回调函数 */
        @Override
        protected void onActivityResult(int requestCode, int resultCode, Intent data)
        {
            // TODO Auto-generated method stub
            if (resultCode != RESULT_OK)
            {
                return;
            }
            switch (requestCode)
            {
                case ButtonRingtone:
                    try
                    {
                        //得到我们选择的铃声
                        Uri pickedUri = data.getParcelableExtra(RingtoneManager.EXTRA_RINGTONE_PICKED_URI);
                        //将我们选择的铃声设置成为默认
                        if (pickedUri != null)
                        {
                            RingtoneManager.setActualDefaultRingtoneUri(Activity01.this, RingtoneManager.TYPE_RINGTONE, pickedUri);
                        }
                    }
                    catch (Exception e)
                    {
                    }
                    break;
                case ButtonAlarm:
                    try
                    {
                        //得到我们选择的铃声
                        Uri pickedUri = data.getParcelableExtra(RingtoneManager.EXTRA_RINGTONE_PICKED_URI);
                        //将我们选择的铃声设置成为默认
                        if (pickedUri != null)
                        {
                            RingtoneManager.setActualDefaultRingtoneUri(Activity01.this, RingtoneManager.TYPE_ALARM, pickedUri);
                        }
                    }
                    catch (Exception e)
                    {
                    }
                    break;
                case ButtonNotification:
                    try
                    {
                        //得到我们选择的铃声
                        Uri pickedUri = data.getParcelableExtra(RingtoneManager.EXTRA_RINGTONE_PICKED_URI);
                        //将我们选择的铃声设置成为默认
                        if (pickedUri != null)
                        {
                            RingtoneManager.setActualDefaultRingtoneUri(Activity01.this, RingtoneManager.TYPE_NOTIFICATION, pickedUri);
                        }
                    }
                    catch (Exception e)
                    {
                    }
                    break;
            }
            super.onActivityResult(requestCode, resultCode, data);
        }
        //检测是否存在指定的文件夹 
        //如果不存在则创建
        private boolean bFolder(String strFolder)
        {
            boolean btmp = false;
            File f = new File(strFolder);
            if (!f.exists())
            {
                if (f.mkdirs())
                {
                    btmp = true;
                }
                else
                {
                    btmp = false;
                }
            }
            else
            {
                btmp = true;
            }
            return btmp;
        }
    }

#网络与通信

Android平台目前有3种网络接口可以使用，它们分别是：标准jaba接口java.net.*,Apache接口org.apache和Android网络接口android.net.*  .  
