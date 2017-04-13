android 启动优化

[Material Design 启动屏幕](https://material.io/guidelines/patterns/launch-screens.html#launch-screens-placeholder-ui)


* 问题1:点击应用图标后等待时间较长
原因：在应用第一次启动（系统杀掉应用的进程的时候）到Activity的onCreate需要一段时间，具体的流程如下
开始加载并启动应用；
应用启动后，显示一个空白的启动窗口；
创建应用进程信息；

初始化应用中的对象 (比如 Application 中的工作)；
启动主线程 (UI 线程) ；
创建第一个 Activity；
加载内容视图 (Inflating) ；
计算视图在屏幕上的位置排版 (Laying out)；
绘制视图 (draw)。

* 问题2:进入启动页前会先白屏一下
消灭白屏的做法 在主题中设置透明即可
白屏跟主题的设置有关，也有可能是黑色的。
例如：

```
    <style name="splash" parent="AppTheme.NoActionBar">

        <item name="android:windowNoTitle">true</item>
        <item name="android:windowActionBar">false</item>
        <item name="android:windowFullscreen">true</item>
        <item name="android:windowIsTranslucent">true</item>

    </style>
```



对于上面的两个问题具体的优化方式

根据Material Design 设计规范中讲到的 [启动屏](https://material.io/guidelines/patterns/launch-screens.html#)进行了如下优化：

*  首先定义一个drawable 用于设置主题背景（android:windowBackground）

```
<?xml version="1.0" encoding="utf-8"?>
<layer-list
    android:opacity="opaque"
    xmlns:android="http://schemas.android.com/apk/res/android">
    <!--这个item项会对内容进行缩放以适应其容器视图-->
    <item android:drawable="@color/colorPrimaryDark"/>
    <!--增加gravity 避免缩放-->
    <!--<item android:top="@dimen/splash_icon_margin_top">-->
        <!--<bitmap android:src="@drawable/splash_icon"-->
             <!--android:gravity="top|center_horizontal"/>-->
    <!--</item>-->

    <item >
        <bitmap android:src="@drawable/splash_icon"
            android:gravity="center"/>
    </item>

</layer-list>
```

<Layer_list>的相关设置可以参考官方说明，[layer_list](https://developer.android.google.cn/guide/topics/resources/drawable-resource.html?hl=zh-cn)

---

示例代码：

```
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
    <item>
      <bitmap android:src="@drawable/android_red"
        android:gravity="center" />
    </item>
    <item android:top="10dp" android:left="10dp">
      <bitmap android:src="@drawable/android_green"
        android:gravity="center" />
    </item>
    <item android:top="20dp" android:left="20dp">
      <bitmap android:src="@drawable/android_blue"
        android:gravity="center" />
    </item>
</layer-list>
```
这里要注意的地方

默认情况下，所有可绘制项都会缩放以适应包含视图的大小。因此，将图像放在图层列表中的不同位置可能会增大视图的大小，并且有些图像会相应地缩放。为避免缩放列表中的项目，请在 < item> 元素内使用 < bitmap> 元素指定可绘制对象，并且对某些不缩放的项目（例如 "center"）定义重力。例如，以下 < item> 定义缩放以适应其容器视图的项目：

```
<item android:drawable="@drawable/image" />
```

为避免缩放，以下示例使用重力居中的 <bitmap> 元素：

```
<item>
  <bitmap android:src="@drawable/image"
          android:gravity="center" />
</item>
```



* 添加splash主题

```
    <style name="splash" parent="AppTheme.NoActionBar">

        <item name="android:windowBackground">@drawable/bg_splash</item>
        <item name="android:windowFullscreen">true</item>
        </style>
```

* 在manifest 中进行设置（注意这里不是在application的标签中设置主题，因为我们只要第一个activity启动的时候有这个效果就可以了）

```
  <activity android:name=".MainActivity"
        android:label="@string/app_name"
        android:theme="@style/splash"
        android:configChanges="orientation|keyboardHidden|screenSize|screenLayout">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
```

* 启动完成运行到Activity 中的 onCreate 的时候再将主题改回来（setTheme()）

```
@Override
    protected void onCreate(Bundle savedInstanceState) {
        setTheme(R.style.AppTheme_NoActionBar);
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
```


