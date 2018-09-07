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

    implementation ('net.yshow.sdk:AA20LSDK:+'){
        //当前SDK使用了Glide做图片的三级缓存,如果你在主项目中已经集成了Glide,或者不希望使用Glide,打开下面这行的注释即可,
        //注意,关闭Glide后将不再支持缓存和Gif的广告
        //exclude group:  'com.github.bumptech.glide',module:'glide'

        //当前SDK依赖微信SDK,如果你在主项目中集成了微信SKD,需要打开下面这行的注释
        //exclude group: 'com.tencent.mm.opensdk' ,module:'wechat-sdk-android-without-mta'
    }

> 注意,Glide 使用的支持库版本为 27。如果你需要使用不同的支持库版本，请打开这行`exclude group:  'com.github.bumptech.glide',module:'glide'`的注释,并且在您的build.gradle中引入Glide.
> 
> 假如你想使用 v26 的支持库：
	
	dependencies {
	    implementation ('net.yshow.sdk:AA20LSDK:+'){
	        exclude group:  'com.github.bumptech.glide',module:'glide'
	    }
		implementation ("com.github.bumptech.glide:glide:4.8.0") {
			exclude group: "com.android.support"
		}
		implementation "com.android.support:support-fragment:26.1.0"
	}

####离线线版本引入


####引入

> 1.在app的build.gradle中加入以下配置

	repositories {    
	    flatDir {        
	        dirs 'libs'   // aar目录
	      }
	}

> 2.离线包内提供有`AA20LSDK-X.X.X.aar`文件   将该aar文件拷贝到app/libs目录下
> 
> 3.在dependencies中加入aar引用

	implementation(name: 'AA20LSDK-X.X.X', ext: 'aar')
或

	compile(name: 'AA20LSDK-X.X.X', ext: 'aar')

> 4.在AndroidManifest.xml中申请权限

	<!--必须权限-->
    <uses-permission android:name="android.permission.INTERNET"/>
	<!--非必须权限-->
    <uses-permission android:name="android.permission.READ_PHONE_STATE"/>
	<!--Glide的缓存需要的权限-->
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>

> 5.在dependencies中引用第三方库

	//必须
    implementation 'com.android.support:appcompat-v7:26.+'
    implementation 'com.tencent.mm.opensdk:wechat-sdk-android-without-mta:+'

	//非必须
	//当前SDK使用了Glide做图片的三级缓存,如果你不希望使用Glide,将下面这行的注释即可,
    //注意,关闭Glide后将不再支持缓存和Gif的广告
    implementation 'com.github.bumptech.glide:glide:4.8.0'
> 注意,Glide 使用的支持库版本为 27。如果你需要使用不同的支持库版本，你需要在你的 build.gradle 文件里去从 Glide 的依赖中去除 "com.android.support"。
> 
> 假如你想使用 v26 的支持库：
	
	dependencies {
	  implementation ("com.github.bumptech.glide:glide:4.8.0") {
	    exclude group: "com.android.support"
	  }
	  implementation "com.android.support:support-fragment:26.1.0"
	}

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
	        android:layout_width="match_parent"
	        android:layout_height="wrap_content"
            android:padding="10dp"
	        app:slotCode="你从20L开发后台申请到的slotCode"
	        app:WeChatAppId="你从微信开放平台申请到的AppId"></net.yshow.adsdk.ADView>
	</LinearLayout>

其中`app:WeChatAppId=""`与`AndroidManifest.xml` 中的`meta-data` 配置其中一处即可

注意: `ADView` 会忽略 `android:layout_height`的值,图片固定以680*340的比例显示,文字最多为2行 建议在竖屏界面中`android:layout_width`的值设置为`match_parent`

> 2.2方式二:代码动态添加

	ADView adView = new ADView(context);
	adView.setSlotCode("你从20L开发后台申请到的slotCode");
	viewGroup.addView(adView);

**ADView提供两个开放的方法**
	
	ADView.setSlotCode(String)//重新设置广告位
	ADView.loadImage()//重新载入广告

