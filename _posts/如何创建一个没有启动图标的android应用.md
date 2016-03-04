title: 如何创建一个没有启动图标的android应用
date: 2016-03-04 16:08:20
categories: Android
tags: android,图标
---

	<activity android:name=".MainActivity" android:label="@string/app_name">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
    </activity>

很简单，如果未对您的Activity之一声明 MAIN 操作或 LAUNCHER 类别，那么您的应用图标将不会出现在应用的主屏幕列表中。