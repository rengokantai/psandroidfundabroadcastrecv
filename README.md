# psandroidfundabroadcastrecv
##3 Creating BroadcastReceiver Statically
###1 Overview and Project setup
03:38 create empty activity


###2 Create out first boradcastreceiver
MyFirstReceiver extends BroadcastReceiver  
override the onReceieve method  

create file MyFirstReceiver.java
```
public class MyFirstReceiver extends BroadcastReceiver{
  private static final String TAG = MyFirstReceiver.class.getSimpleName();
  @Override
  public void onReceive(Context context,Intent intent){
    Log.i(TAG,"Hello");
    Toast.makeText(context,"test",Toast.LENGTH_LONG).show();
  }
}
```

AndroidManifest.xml
```
<application>
  <activity android:name=".MainActivity">
    <intent-filter>
      <action android:name="android.intent.action.MAIN"/>
      <category android:name="android.intent.category.LAUNCHER"/>
    </intent-filter>
  </activity>
  <receiver android:name=".MyFirstReceiver"/>
</application>
```


MainActivity.java
```
public void sendBroadcastMessage(View view){
  Intent intent = new Intent(this,MyFirstReceiver.class);
  sendBroadcast(intent);
}
```
###3 BroadcastReceiver as Inner static
```
<receiver android:name=".MainActivity$MyThirdReceiverInner"/>
```
MainActivity.java
```
public static class MyThirdReceiverInner extends BroadcastReceiver{
    @Override
  public void onReceive(Context context,Intent intent){
    Log.i(TAG,"Hello 3rd");
    Toast.makeText(context,"3rd test",Toast.LENGTH_LONG).show();
  }
}
```
###4 Using Intent Filter and Custom Action name
original
```
<receiver android:name=".MyFirstReceiver" />
```
to
```
<receiver android:name=".MyFirstReceiver">
    //add this part
  <intent-filter>
    <action android:name="my.custom.action.name"/>
  </intent-filter>
</receiver>
```
in  
MainActivity.java
```
public void sendBroadcastMessage(View view){
  //Intent intent = new Intent(this,MyFirstReceiver.class);
  Intent intent = new Intent("my.custom.action.name");
  sendBroadcast(intent);
}
```


###5 Multiple Receivers with Same Action Name
```
<receiver android:name=".MyFirstReceiver">
  <intent-filter>
    <action android:name="my.custom.action.name"/>
  </intent-filter>
</receiver>

<receiver android:name=".MainActivity$MyThirdReceiverInner">
  <intent-filter>
    <action android:name="my.custom.action.name"/>
  </intent-filter>
</receiver>
```
####6 Properties Related to BroadcastReceivers
####00:10
Broadcasts are sent asynchronously
```
public void sendBroadcastMessage(View view){
  //Intent intent = new Intent(this,MyFirstReceiver.class);
  Intent intent = new Intent("my.custom.action.name");
  sendBroadcast(intent);
  //add this
  Toast.makeText(context,"This shows first before all receivers",Toast.LENGTH_LONG).show();
}
```

####01:43
BroadcastReceivers work in main thread (we vannot block the main thread for longer duration of time)
Android system will generate ANR and the app will crash

MyFirstReceiver.java
```
//add
try{
  Thread.sleep(10000);
}catch{
}
```
####05:05
Never perform long-running task insideon the onReceive method of the broadcastreceiver


###7 Passing Data from Activity to Receiver
```
public void sendBroadcastMessage(View view){
  //Intent intent = new Intent(this,MyFirstReceiver.class);
  Intent intent = new Intent("my.custom.action.name");
  intent.putExtra("name","ke");
  //Or
  //Bundle bundle = new Bundle();
  //bundle.putString("name","ke");
  //intent.putExtras(bundle);
  sendBroadcast(intent);
  
 
}
```

MyFirstReceiver.java
```
public class MyFirstReceiver extends BroadcastReceiver{
  private static final String TAG = MyFirstReceiver.class.getSimpleName();
  @Override
  public void onReceive(Context context,Intent intent){
    //add this
    String name = intent.getStringExtra("name");
    Log.i(name);
    Log.i(TAG,"Hello");
    Toast.makeText(context,"test",Toast.LENGTH_LONG).show();
  }
}
```
###8 Some Commom Usage of BroadcastReceiver
Example
```
<receiver android:name=".MyFirstReceiver">
  <intent-filter>
    //change this line
    <action android:name="android.intent.action.AIRPLANE_MODE"/> //if we toggle this in notification panel, the toast will show
    // android.net.wifi.WIFI_CHANGED
  </intent-filter>
</receiver>
```
