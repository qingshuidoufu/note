# 1.活动

1. 继承activity类

2. 重写onCreate方法

   加载布局: setContentView(R.layout.first_layout)

   AndroidMainifest中注册 android:name=".FirstActivity"

- 使用Toast

      Button button = (Button) findViewById(R.id.**button_1**);
       button.setOnClickListener(**new** View.OnClickListener() {
         @Override
         **public void** onClick(View v) {
           Toast.*makeText*(FirstActivity.**this**, 

  ​         **"****你點擊了按鈕****"**, Toast.**LENGTH_LONG**).show();
  ​       }

- 使用Menu

  <**item**   **android:id****="@+id/****add_item****"
  **   **android:title****="Add"**/>

- 重写onCreateOptionMenu()

  **public** **boolean** onCreateOptionsMenu(Menu menu) {
     getMenuInflater().inflate(R.menu.**main**, menu);
     **return true**;
   }

- 重写onOptionsItemSelect方法

  **public** **boolean** onOptionsItemSelected(MenuItem item) {
     **switch** (item.getItemId()) {
       **case** R.id.**add_item**:
         Toast.*makeText*(**this**, **"You clicked Add"**,    

  ​          Toast.**LENGTH_SHORT**).show();
  ​       **break**;
  ​     **case** R.id.**remove_item**:
  ​       Toast.*makeText*(**this**, **"You clicked Remove"**,

  ​        Toast.**LENGTH_SHORT**).show();
  ​       **break**;
  ​     **default**:
     }

     **return** **super**.onOptionsItemSelected(item);
   }

销毁活动: finish()

显式使用Intent

	Intent intent = **new** Intent(FirstActivity.**this**, SecondActivity.**class**);
	 startActivity(intent);
- 隐式使用Intent

  配置androidManiFest.xml

  <**activity** **android:name****=".****SecondActivity****"**>
     <**intent-filter**>
       <**action** **android:name****="****com.example.activitytest.ACTION_START****"** />
       <**category** **android:name****="****android.intent.category.DEFAULT****"** />
     </**intent-filter**>
   </**activity**>

  使用:

    **public void** onClick(View v) {
       Intent intent = **new** Intent(**"****com.example.activitytest.ACTION_START****"**);
       startActivity(intent);
   }

Intent传递数据:

**intent.putExtra****("****extra_data****", data);**

使用数据:

**Intent** **intent** **=** **getIntent****();**
     **String** **data =** **intent.getStringExtra****("extra_data");**

![image-20210626134139113](images/android复习/image-20210626134139113.png)

![image-20210626134153923](images/android复习/image-20210626134153923.png)

![image-20210626134400042](images/android复习/image-20210626134400042.png)

存放SaveInstance的Bundle类型数据

**protected void** **onSaveInstanceState****(Bundle** **outState****) {**
   **super.onSaveInstanceState****(****outState****);**
   **String** **tempData** **= "Something you just typed";**
   **outState.putString****("****data_key****",** **tempData****);**
 **}**

获得SaveInstanceState的数据

  **if** **(****savedInstanceState** **!= null) {**
   **String** **tempData** **=** **savedInstanceState.getString****("****data_key****");**
   **Log.d****(TAG****,** **tempData****);**
   **}**

![image-20210626134736209](images/android复习/image-20210626134736209.png)

![image-20210626134756585](images/android复习/image-20210626134756585.png)![image-20210626134820428](images/android复习/image-20210626134820428.png)

![image-20210626141253579](images/android复习/image-20210626141253579.png)

![image-20210626141303436](images/android复习/image-20210626141303436.png)

![image-20210626141421060](images/android复习/image-20210626141421060.png)

![image-20210626141515228](images/android复习/image-20210626141515228.png)

![image-20210626141531563](images/android复习/image-20210626141531563.png)

![image-20210626141554502](images/android复习/image-20210626141554502.png)

![image-20210626142015046](images/android复习/image-20210626142015046.png)

![image-20210626142041135](images/android复习/image-20210626142041135.png)

![image-20210626142055212](images/android复习/image-20210626142055212.png)

![image-20210626142108837](images/android复习/image-20210626142108837.png)

![image-20210626142129242](images/android复习/image-20210626142129242.png)

![image-20210626142212015](images/android复习/image-20210626142212015.png)

![image-20210626152724893](images/android复习/image-20210626152724893.png)

![image-20210626152746204](images/android复习/image-20210626152746204.png)

![image-20210626152853180](images/android复习/image-20210626152853180.png)

![image-20210626152919190](images/android复习/image-20210626152919190.png)

![image-20210626152932164](images/android复习/image-20210626152932164.png)

![image-20210626153231190](images/android复习/image-20210626153231190.png)

![image-20210626153241510](images/android复习/image-20210626153241510.png)

![image-20210626153326812](images/android复习/image-20210626153326812.png)

![image-20210626153400829](images/android复习/image-20210626153400829.png)

![image-20210626153608550](images/android复习/image-20210626153608550.png)

![image-20210626153653588](images/android复习/image-20210626153653588.png)

![image-20210626153718099](images/android复习/image-20210626153718099.png)

![image-20210626153806308](images/android复习/image-20210626153806308.png)

![image-20210626153827372](images/android复习/image-20210626153827372.png)

![image-20210626154855996](images/android复习/image-20210626154855996.png)

![image-20210626154927949](images/android复习/image-20210626154927949.png)

![image-20210626155018547](images/android复习/image-20210626155018547.png)

![image-20210626155132794](images/android复习/image-20210626155132794.png)

![image-20210626155303877](images/android复习/image-20210626155303877.png)

![image-20210626155331365](images/android复习/image-20210626155331365.png)

![image-20210626155351414](images/android复习/image-20210626155351414.png)