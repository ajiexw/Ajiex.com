Date:2013-08-22
Tags: Android

ActivityGroup是Google提供的一个非常优秀的API，而TabActivity是ActivityGroup的唯一子类。

##ActivityGroup的优势

将Activity嵌入到ActivityGroup当中，实现了场景（Context）细分化。ActivityGroup是主场景，用户可以通过导航按钮来切换想要的子场景。主场景能拥有多个逻辑处理模块，不再负责子场景逻辑，只负责切换场景的逻辑，而每一个Activity（子场景）拥有独立的逻辑处理模块。这样就细分化和模块化了逻辑代码。

##TabActivity的不足

TabActivity自己独有的视图（标签页按钮形式）几乎没人使用，大多数开发者用到的特性几乎都是从ActivityGroup继承下来。

TabActivity的强制依赖关系，它的布局文件必须将TabHost作根标签，id必须为”@android:id/tabhost”；必须有TabWidget标签，id必须是”@android:id/tabs”；还有加载Activity的View容器，id必须为@android:id/tabcontent。

##用ActivityGroup来实现的主页面架构

    public class MainActivityGroup extends ActivityGroup implements OnCheckedChangeListener {
      private static final String RECORD = "record";
      private static final String CATEGORY = "category";
      private static final String MORE = "more";

      private ActivityGroupManager manager = null;
      private FrameLayout container = null;
      private RadioGroup radioGroup;

      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.test_main);
          initView();
      }

      private void initView() {
          radioGroup = (RadioGroup) findViewById(R.id.main_radio);
          radioGroup.setOnCheckedChangeListener(this);

          manager = new ActivityGroupManager();
          container = (FrameLayout) findViewById(R.id.container);
          manager.setContainer(container);
          switchActivity(ActivityGroupManager.RECORD_ACTIVITY_VIEW, RECORD, RecordActivity.class);
      }

      @Override
      public void onCheckedChanged(RadioGroup group, int checkedId) {
          switch (checkedId) {
          case R.id.record_button:
              switchActivity(ActivityGroupManager.RECORD_ACTIVITY_VIEW, RECORD, RecordActivity.class);
              break;
          case R.id.category_button:
              switchActivity(ActivityGroupManager.CATEGORY_ACTIVITY_VIEW, CATEGORY, CategoryActivity.class);
              break;
          case R.id.more_button:
              switchActivity(ActivityGroupManager.MORE_AVTIVITY_VIEW, MORE, MoreActivity.class);
              break;
          default:
              break;
          }
      }
      
      private View getActivityView(String activityName, Class<?> activityClass) {
          return getLocalActivityManager().startActivity(activityName,
                  new Intent(MainActivityGroup.this, activityClass))
                  .getDecorView();
      }

      private void switchActivity(int num, String activityName, Class<?> activityClass) {
          manager.showContainer(num, getActivityView(activityName, activityClass));
      }

    }

可以看到在“主场景”MainActivityGroup中基本没有任何逻辑代码，只有各个“子场景”切换的逻辑，而子场景的切换用了一个ActivityGroupManager类来管理，这样又起到了代码分离的作用。

    public class ActivityGroupManager {

      private static final String TAG = "frag_manager";

      public static final int RECORD_ACTIVITY_VIEW = 0;
      public static final int CATEGORY_ACTIVITY_VIEW = 1;
      public static final int MORE_AVTIVITY_VIEW = 2;

      private HashMap<Integer, View> hashMap;
      private ViewGroup container;

      public ActivityGroupManager() {
          hashMap = new HashMap<Integer, View>();
      }

      public void setContainer(ViewGroup container) {
          this.container = container;
      }
      
      public void showContainer(int num, View view) {
          if (!hashMap.containsKey(num)) {
              hashMap.put(num, view);
              container.addView(view);
          }

          for (Iterator<Integer> iter = hashMap.keySet().iterator(); iter.hasNext();) {
              Object key = iter.next();
              View v = hashMap.get(key);
              v.setVisibility(View.INVISIBLE);
          }
          
          view.setVisibility(View.VISIBLE);
      }
    }

值得一说的是在ActivityGroupManager类中的showContainer ()方法并没有像网上的做法这样：


    container.removeAllViews();
    container.addView(view);


这种做法看似代码逻辑更简单，但是这样就会导致每次切换“子场景”的时候都会把已经加载过的View remove掉，一方面性能有所欠缺，另一个方面“子场景”的状态无法记住。而现在的做法就很好的解决了上面的问题。

好了，然后就是各个“子场景”（Activity）的代码了，每个“子场景”的逻辑各自独立，这里就不上代码了。

这里还有一个需要提到的是底部导航栏的实现，由于android没有像iOS那样预定好的组件，只有自己定义一个布局了，这里我用到的是用RadioGroup来实现类似微信的底部导航栏效果，下面是布局代码，在其他使用到的地方直接include进来就可以了。

    <?xml version="1.0" encoding="utf-8"?>
    <RadioGroup xmlns:android="http://schemas.android.com/apk/res/android"
        android:id="@+id/main_radio"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_gravity="bottom|center"
        android:layout_marginBottom="-20dp"
        android:gravity="center"
        android:orientation="horizontal"
        android:padding="0dp" >

        <RadioButton
            android:id="@+id/record_button"
            android:layout_width="fill_parent"
            android:layout_height="wrap_content"
            android:layout_gravity="center"
            android:layout_weight="1.0"
            android:background="@null"
            android:button="@null"
            android:checked="true"
            android:drawableTop="@drawable/main_tab_record_selector"
            android:gravity="center"
            android:tag="record" />

        <RadioButton
            android:id="@+id/category_button"
            android:layout_width="fill_parent"
            android:layout_height="wrap_content"
            android:layout_weight="1.0"
            android:background="@null"
            android:button="@null"
            android:drawableTop="@drawable/main_tab_category_selector"
            android:gravity="center_horizontal"
            android:tag="category" />

        <RadioButton
            android:id="@+id/more_button"
            android:layout_width="fill_parent"
            android:layout_height="wrap_content"
            android:layout_weight="1.0"
            android:background="@null"
            android:button="@null"
            android:drawableTop="@drawable/main_tab_more_selector"
            android:gravity="center_horizontal"
            android:tag="more" />

    </RadioGroup>
