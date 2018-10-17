#贰拾楼广告SDK
##准备工作:


1. 在20L开放平台注册开发者  得到`slotCode`和 `小程序AppID`

2. 在微信开放平台注册开发者  得到`WeChatAppId`

3. 登录微信开放平台，在**“管理中心-移动应用-应用详情-关联小程序信息”**，使用第1步中的`小程序AppID`发起关联小程序操作

##使用方法:



####在线版本引入

> 1.在`build.gradle`的`allprojects`.`repositories`下增加一个仓库

	allprojects {
	    repositories {
	        google()
	        jcenter()
	        maven {
	            url "https://github.com/YshowMofeng/AA20LSDK/raw/master"
	        }
	    }
	}

> 2.在`app/build.gradle`下引入SDK库

    implementation ('net.yshow.sdk:AA20LSDK:+')
	//{
	//当前sdk使用了  com.android.support:appcompat-v7:25.4.0   如果引入后报错了   打开下面这行的注释
	//    exclude group: "com.android.support"
	//当前sdk使用了 com.github.bumptech.glide:glide:4.8.0   如果引入后报错了   打开下面这行的注释
	//    exclude group: "com.github.bumptech.glide"
	//}

>//针对Glide3.7.0版本改为下面这个

    api ('net.yshow.sdk:AA20LSDK_G370:+'){
		exclude group: "com.github.bumptech.glide"
	}

>//如果使用了其他版本的Glide并且以上两个引入方法都报错了,请联系我重新打包一个SDK

####使用

> **1.在 `AndroidManifest.xml` 以`meta-data`的方式配置你的微信AppId**
	
	<?xml version="1.0" encoding="utf-8"?>
	<manifest xmlns:android="http://schemas.android.com/apk/res/android"
	    package="net.yshow.addemo">
	
	    <application
	        android:icon="@mipmap/ic_launcher"
	        android:label="@string/app_name"
	        android:theme="@style/AppTheme">
	
	        <meta-data android:name="WeChatAppId" android:value="你从微信开放平台申请到的AppId"/>
	
	        <activity android:name=".MainActivity">
	            <intent-filter>
	                <action android:name="android.intent.action.MAIN" />
	                <category android:name="android.intent.category.LAUNCHER" />
	            </intent-filter>
	        </activity>
	    </application>
	
	</manifest>



> **2.添加广告插件**
> 
> 2.1 方式一:在布局中添加

	<?xml version="1.0" encoding="utf-8"?>
	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:tools="http://schemas.android.com/tools"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent"
	    android:orientation="vertical"
	    tools:context=".MainActivity">

        <net.yshow.adsdk.ADView 
			xmlns:app="http://schemas.android.com/apk/res-auto"
            android:id="@+id/adview1"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            app:WeChatAppId="你从微信开放平台申请到的AppId"
	        app:slotCode="你从20L开发后台申请到的slotCode"
            app:adType="banner"></net.yshow.adsdk.ADView>

        <net.yshow.adsdk.ADView 
			xmlns:app="http://schemas.android.com/apk/res-auto"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:padding="10dp"
            app:WeChatAppId="你从微信开放平台申请到的AppId"
	        app:slotCode="你从20L开发后台申请到的slotCode"
            app:adType="card"
            app:subscriptTextSize="15dp"
            app:titleTextSize="20dp"></net.yshow.adsdk.ADView>
	</LinearLayout>



- 其中`app:WeChatAppId=""`与`AndroidManifest.xml` 中的`meta-data` 配置其中一处即可

- 广告类型`app:adType`有两个值  分别是卡片广告`card` 和 横幅广告`banner` 缺省值为`card`

- 卡片广告中 `app:titleTextSize`为标题的文字大小  `app:subscriptTextSize`为下标`贰拾楼广告联盟`的字体大小

- 横幅广告中`app:titleTextSize`和`app:subscriptTextSize`无效

- 注意: `ADView` 会忽略 `android:layout_height`的值,在卡片广告类型下图片固定以680x340的比例显示,文字最多为2行;在横幅广告类型下图片以375x90的比例显示;建议在竖屏界面中`android:layout_width`的值设置为`match_parent`

> 2.2方式二:代码动态添加

	//卡片广告
	ADView adView = new ADView(context);
	adView.setSlotCode("你从20L开发后台申请到的slotCode");
	adView.setAdType(ADView.AD_TYPE_CARD);
	adView.setSubscriptTextSize(TypedValue.COMPLEX_UNIT_DIP,30);
	adView.setTitleTextSize(TypedValue.COMPLEX_UNIT_DIP,15);
	viewGroup.addView(adView);

	//横幅广告

    ADView adView = new ADView(context);
    adView.setSlotCode("你从20L开发后台申请到的slotCode");
    adView.setAdType(ADView.AD_TYPE_BANNER);
    viewGroup.addView(adView);

**ADView提供的方法**
	
	ADView.setSlotCode(String)//重新设置广告位
	ADView.loadImage()//重新载入广告
	ADView.setAdType(int)//设置广告类型  ADView.AD_TYPE_CARD  或  ADView.AD_TYPE_BANNER  
	ADView.setTitleTextSize(int,int)//设置卡片广告的标题文字大小,用法等同TextView.setTextSize(int,int)
	ADView.setSubscriptTextSize(int,int)//设置卡片广告的下标文字大小,用法等同TextView.setTextSize(int,int)
