---
title: Android输入法回车键自定义字符问题
date: 2016-09-30 09:08:57
tags:
- Android
- 输入法
categories: 
- Android 
---
### 期望
某个EditText编辑时，软键盘的回车键显示“登录”，点击以后可以执行登录的操作。
布局中的EditText:
```xml
                <EditText
                    android:id="@+id/password"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:hint="@string/prompt_password"
                    android:imeActionId="@+id/login"
                    android:imeActionLabel="@string/action_sign_in"
                    android:imeOptions="actionDone"
                    android:inputType="textPassword"
                    android:singleLine="true" />
```
代码中监听处理该EditText变化代码:
```java
        etPassword.setOnEditorActionListener(new TextView.OnEditorActionListener() {
            @Override
            public boolean onEditorAction(TextView textView, int id, KeyEvent keyEvent) {
                ZLog.d("onEditorAction id = " + id);
                ZLog.d("onEditorAction R.id.login = " + R.id.login);
                if (id == R.id.login || id == EditorInfo.IME_ACTION_DONE) {
                    ZLog.d("onEditorAction in if");
                    attemptLogin();
                    return true;
                }
                return false;
            }
```
### 问题
测试部分系统表现如预期，但是也有部分系统回车字符确实变成了“登录”，然而点击后无作用，通过log看到`onEditorAction`中的`id`为0（`EditorInfo.IME_ACTION_UNSPECIFIED`）而不是预期的6（`EditorInfo.IME_ACTION_DONE`）
猜测是不同系统版本EditText的代码略有不同导致的问题。
### 修改
在EditText监听前，用代码再加一下改变回车键字符和执行的Action的代码
```java
        etPassword.setImeActionLabel(getString(R.string.action_sign_in), EditorInfo.IME_ACTION_DONE);
        etPassword.setOnEditorActionListener(new TextView.OnEditorActionListener() {
        ...
```
修改以后再测试点击回车键都正确触发了id为`EditorInfo.IME_ACTION_DONE`的事件，执行了预期的代码流程。
### 第三方输入法
在某些第三方输入法上（比如搜狗）回车键无法修改成我们需要的自定义字符，如有方法请留言告知下，谢谢！
