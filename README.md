#React Native Andoird的配置说明-2015-09-16修改

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

Gradle中配置的API Level 必须在sdk目录中是已安装的版本


*  ***compileSdkVersion***	

	SDK的版本号，也就是API Level，例如API-19、API-20、API-21等等
* ***buildToolsVersion***

	构建工具的版本，其中包括了打包工具aapt、dx等等。这个工具的目录位于..your_sdk_path/build-tools/XX.XX.XX
	
*  ***targetSdkVersion***

	在编译程序是指定一个API Level，我一般配置为跟 compileSdkVersion一致。
	
##3 打包真机运行的APK文件

	1.执行react-native init androiddemo（根据实际情况来创建）项目。
	2.在androiddemo目录中找到路径/android/app/src/main，并在该目录下新建《assets》文件夹，这个文件夹存放android app的资源文件。
	3.在/androiddemo/路径下执行react-native start命令，待执行完毕以后再执行以下命令
	curl -k 'http://localhost:8081/index.android.bundle' > android/app/src/main/assets/index.android.bundle
	该命令的意思是将index.android.bundle下载并保存到assets资源文件夹中
	4.打包的apk在未签名的情况下,在手机中（非root）是不允许安装的，所以需要添加gradle的android keystore配置，在build.gradle文件中，具体配置如下
	  //签名
    signingConfigs{
        release {
            storeFile file("/Volumes/Android.KeyStore/AndroidDebug.keystore")
            storePassword "密码"
            keyAlias "keyAlias的名字"
            keyPassword "密码"
        }
    }
     buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
            signingConfig signingConfigs.release //添加这句话引用签名配置
        }
    }
    注意一下配置顺序
    5.在androiddemo/android/目录中执行gradle assembleRelease命令，打包后的文件在	androiddemoi/android/app/build/outputs/apk目录中，例如app-release.apk。如果打包碰到	问题可以先执行 gradle clean 清理一下。
    6.将apk复制到手机中安装运行
	

##附 执行run-android时碰到的问题

	第一次执行run-android是一个漫长的过程，主要是需要下载gradle之类的执行文件，如果没有安装sdk或者配置ANDROID_HOME有可能会碰到一下问题。

*	**A problem occurred configuring project ':app'.
	failed to find target with hash string 'android-23' in: /Users/zbmobi/DevelopTools/Android/sdk**
	
	在sdk/platforms目录中找不到gradle配置中compiletargetSdkVersion对应的androidsdk版本号
	解决：修改compileSdkVersion和targetSdkVersion的对应版本号
	如图
	
*	***A problem occurred configuring project ':app'.
 failed to find Build Tools revision 23.0.1***
 
 在sdk/build-tools目录中找不到gradle配置的buildToolsVersion对应的版本号
解决：查看build-tools目录，然后修改为已存在的版本号

*	***A problem occurred configuring project ':app'. Could not resolve all dependencies for configuration ':app:_debugCompile'.
   Could not find com.android.support:appcompat-v7:23.0.0.***
   
在build.gralde文件里 
  dependencies{
  compile ''
  }
  这个的作用主要是导入在android程序所用的类库
  上面的错误是build.gradle文件中配置的com.android.support:appcompat-V7版本号找不到导致的
  解决：查看Android/sdk/extras/android/support/v7目录修改为已存在的版本号
  
  *	***Execution failed for task ':app:installDebug'.
com.android.builder.testing.api.DeviceException: No connected devices***

	找不到连接电脑的设备
解决：连接真机或者模拟器，这里推荐模拟器，在调试的时候方便一些，这里推荐genymotion
	
	
	
	一般解决上面问题，程序应该执行成功了，碰到后续问题再补充。
  
