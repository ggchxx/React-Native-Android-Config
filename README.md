#React Native Andoird的配置说明

##1 关于Android SDK

由于现在官网被墙,可以去[AndroidDevtools](http://www.androiddevtools.cn'AndroidDevtools')下载相关资料
下载解压的目录一般是android/sdk/**，如果缺少 不同sdk版本的build-tools之类的可以从下面子目录单独配置

比较重要的目录

 * 【build-tools】里面是不同版本(例如21.1.1)的build工具，这些工具包括了aapt打包工具、dx.bat、aidl.exe等等

* 【platform】是存放不同API-level版本SDK目录的地方

* 【platform-tools】是一些android平台相关的工具，adb、fastboot等

* 【tools】是指的安卓开发相关的工具，例如android.bat、ddms.bat(Dalvik debug Monitor Service)、draw9patch.bat等等

##2 关于Gradle配置
在React Native Android 中打包是有Gradle来实现，主要配置文件《build.gradle》，在执行react-native run-android时 会根据这个配置来打包apk并安装。
参考文档

* [Android Gradle插件用户指南](http://rinvay.github.io/android/2015/03/26/Gradle-Plugin-User-Guide%28Translation%29/#1)

在执行run-android 时需要用的配置参数

Gradle中配置的参数必须在sdk目录中安装


*  ***compileSdkVersion***	

	SDK的版本号，也就是API Level，例如API-19、API-20、API-21等等
* ***buildToolsVersion***

	构建工具的版本，其中包括了打包工具aapt、dx等等。这个工具的目录位于..your_sdk_path/build-tools/XX.XX.XX
	
*  ***targetSdkVersion***

	在编译程序是指定一个API Level，我一般配置为跟 compileSdkVersion一致。
	

##执行run-android时碰到的问题

*	**A problem occurred configuring project ':app'.
	failed to find target with hash string 'android-23' in: /Users/zbmobi/DevelopTools/Android/sdk**
	
	在sdk/platforms目录中找不到gradle配置中compiletargetSdkVersion对应的androidsdk版本号
	解决：修改compileSdkVersion和targetSdkVersion的对应版本号
	如图
*	*** What went wrong:
A problem occurred configuring project ':app'.
 failed to find Build Tools revision 23.0.1***
 
 在sdk/build-tools目录中找不到gradle配置的buildToolsVersion对应的版本号
解决：查看build-tools目录，然后修改为已存在的版本号

*	***What went wrong:
A problem occurred configuring project ':app'. Could not resolve all dependencies for configuration ':app:_debugCompile'.
   Could not find com.android.support:appcompat-v7:23.0.0.***
   
   在build.gralde文件里 
  dependencies{
  compile ''
  }
  这个的作用主要是导入在android程序所用的类库
  上面的错误是com.android.support:appcompat-V7导致的
  解决：查看Android/sdk/extras/android/support/v7目录修改为已存在的版本号
  

  
