# 解决Android Q 文件读写问题

> Android Q 默认开启沙箱模式 导致出现文件读写失败
>
> 需要在使用动态权限申请的情况下在AndroidManifest.xml中加入
>
> ```java
>  android:requestLegacyExternalStorage="true"
> ```



> AndroidManifest.xml文件内容
>
> ```java
> <?xml version="1.0" encoding="utf-8"?>
> <manifest xmlns:android="http://schemas.android.com/apk/res/android"
>     package="com.example.sdcard">
> 
>     <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
>     <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
> 
>     <application
>         android:allowBackup="true"
>         android:icon="@mipmap/ic_launcher"
>         android:label="@string/app_name"
>         android:requestLegacyExternalStorage="true"
>         android:roundIcon="@mipmap/ic_launcher_round"
>         android:supportsRtl="true"
>         android:theme="@style/AppTheme">
>         <activity android:name=".MainActivity">
>             <intent-filter>
>                 <action android:name="android.intent.action.MAIN" />
> 
>                 <category android:name="android.intent.category.LAUNCHER" />
>             </intent-filter>
>         </activity>
>     </application>
> 
> </manifest>
> ```



> 动态权限申请
>
> ```java
> public Boolean checkPermission() {
>     boolean isGranted = true;
>     if (android.os.Build.VERSION.SDK_INT >= 23) {
>         if (this.checkSelfPermission(Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED) {
>             //如果没有写sd卡权限
>             isGranted = false;
>         }
>         if (this.checkSelfPermission(Manifest.permission.READ_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED) {
>             isGranted = false;
>         }
>         Log.i("cbs","isGranted == "+isGranted);
>         if (!isGranted) {
>             this.requestPermissions(
>                     new String[]{Manifest.permission.ACCESS_COARSE_LOCATION, Manifest.permission
>                             .ACCESS_FINE_LOCATION,
>                             Manifest.permission.READ_EXTERNAL_STORAGE,
>                             Manifest.permission.WRITE_EXTERNAL_STORAGE},
>                     102);
>         }
>     }
>     return isGranted;
> }
> ```



> 实例代码链接
>
> https://github.com/skkily/AndroidQ_SDcardWriter
