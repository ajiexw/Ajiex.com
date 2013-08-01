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
    
    