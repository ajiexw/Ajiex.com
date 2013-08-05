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
	 