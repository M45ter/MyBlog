---
title: Android实现开机自启动
date: 2016-09-27 11:13:41
tags:
- Android
- 自启动
categories: 
- Android 
---
### 目的
有时我们会想在android设备启动的时候就在自己的app中执行某些代码（比如启动某些service，记录某些数据等）
### 1.在`AndroidManifest.xml`中注册广播接收器，加上接收广播的权限
```xml
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        
        <receiver android:name="BootBroadcastReceiver" >
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
            </intent-filter>
        </receiver>
```
### 2.在广播接收器的`onReceive`方法中写执行代码
```java
public class BootBroadcastReceiver extends BroadcastReceiver {

	private final static String TAG = "BootBroadcastReceiver";

	@Override
	public void onReceive(Context context, Intent intent) {
		// TODO Auto-generated method stub
		String action = intent.getAction();
		Log.d(TAG, "onReceive action = " + action);
		Intent bootIntent = new Intent(context, MainActivity.class);
		bootIntent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
		context.startActivity(bootIntent);
	}

}
```
### 当然有些厂家对开机自启动有权限管理，要打开权限才能执行。
