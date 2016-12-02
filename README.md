# psandroidfundabroadcastrecv
##3 Creating BroadcastReceiver Statically
###1 Overview and Project setup
03:38 create empty activity


###2 Create out first boradcastreceiver
MyFirstReceiver extends BroadcastReceiver  
override the onReceieve method  

create file MyFirstReceiver
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
