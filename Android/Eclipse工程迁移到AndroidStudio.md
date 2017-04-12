###Eclipse中Gradle构建工程（方便迁移到AS）

----





安装插件：
Help-Install new software-add
 ![这里写图片描述](http://img.blog.csdn.net/20170309221819909?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjI3OTcwMzk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
gradle - http://dist.springsource.com/release/TOOLS/gradle
等待安装结束。（需翻墙）
安装完之后下载下载gradle  http://services.gradle.org/distributions
现在完成后解压 然后配置环境变量
 ![这里写图片描述](http://img.blog.csdn.net/20170309222220879?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjI3OTcwMzk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
GRADLE_HOME    到gradle的解压目录gradle-1.12
配置系统变量 path  到gradle的bin 目录
在CMD 中运行gradle –v 可查看当前版本
AS 中默认放在C盘   我这边直接用AS 中的gradle 配置好之后 
在eclipse 中新建一个android application
项目文件右键 选择”Export“导出 选择Generate Gradle build files.
 ![这里写图片描述](http://img.blog.csdn.net/20170309222311926?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjI3OTcwMzk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


打开Gradle 文件夹下的 gradle-wrapper.properties 文件 
distributionUrl=https\://services.gradle.org/distributions/gradle-1.12-all.zip
这里可以看到下载路径的配置文件的位置，在没有gradle的时候会初始化进行下载。 
 ![这里写图片描述](http://img.blog.csdn.net/20170309222359461?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjI3OTcwMzk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


打开build.gradle文件设置文件内容如下：

```
buildscript {
    repositories {
          mavenCentral()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:0.12.+'
//插件版本号与gradle调用编译时是有依赖关系的。插件的版本越高，需要更多gradle的新特性，新的gradle特性需要新版本的gradle才能支持。测试发现gradle2.0之后的版本需要1.5左右的插件版本，最新的3.0需要 2.0 的插件版本。
    }
}


apply plugin: 'com.android.application'
apply plugin: 'maven'
apply plugin: 'eclipse'
repositories {
    mavenCentral()
}
 
dependencies {
    compile fileTree(dir: 'libs', include: '*.jar')
    compile 'com.android.support:appcompat-v7:23.4.0'
    compile 'com.google.code.gson:gson:2.2.4'
}

 
android {
    compileSdkVersion 22
    buildToolsVersion "23.0.3"
 
    defaultConfig {
            applicationId "com.example.chris"
            minSdkVersion 16
            targetSdkVersion 22
    }
     
    buildTypes {
        debug {
            applicationIdSuffix ".debug"
        }
        
    }
 
    lintOptions
            {
                abortOnError false
            }
 
    sourceSets {
        main {
            manifest.srcFile 'AndroidManifest.xml'
            java.srcDirs = ['src']
            resources.srcDirs = ['src']
            aidl.srcDirs = ['src']
            renderscript.srcDirs = ['src']
            res.srcDirs = ['res']
            assets.srcDirs = ['assets']
        }
 
      
        instrumentTest.setRoot('tests')
     
        debug.setRoot('build-types/debug')
        release.setRoot('build-types/release')
    }
}
 
////////////////////// configure eclipse //////////////////////
eclipse.classpath.plusConfigurations += configurations.compile
//.classpath
eclipse.classpath.file {
    beforeMerged { classpath ->
        classpath.entries.removeAll() { c ->
            c.kind == 'src'
        }
    }
    // Direct manipulation of the generated classpath XML
    withXml {
        def node = it.asNode()
      
        node.appendNode('classpathentry kind="src" path="src"')
  
        node.appendNode('classpathentry kind="src" path="gen"')
        node.children().removeAll() { c ->
            def path = c.attribute('path')
            path != null && (
                    path.contains('/com.android.support/support-v7')
            )
        }
    }
}
eclipse.project {
    name = 'Chris'
    natures 'com.android.ide.eclipse.adt.AndroidNature'
    buildCommand 'com.android.ide.eclipse.adt.ResourceManagerBuilder'
    buildCommand 'com.android.ide.eclipse.adt.PreCompilerBuilder'
    buildCommand 'com.android.ide.eclipse.adt.ApkBuilder'
}


```

然后右键工程
选择 configure 中 Convert to Gradle Project之后
再右键选择Gradle  中 Refresh All

最后选择项目属性在Source中加入 src 和gen
 
![这里写图片描述](http://img.blog.csdn.net/20170309222551771?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjI3OTcwMzk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
到这里在eclipse 中用gradle构建项目已完成。
在构建完成后会发现目前工程的为gradle项目，可能还会出现“Android requires compiler compliance level 5.0 or 6.0. Found '1.8' instead. Please use Android Tools > Fix Project Properties.”这样的错误提示。
具体解决步骤：
项目右键  android tools – Fix Projiect properties，如果不行则查看项目项目属性project-properties-java Compiler   确认JDK compliance 被设置为1.6 并选中enable specific seeting 

  还有一种情况在你按上面的步骤操作后并没有反应。无效
原来是我在上面的操作之前点过 project – properties – project facets  
现在将界面中的java版本改为1.6 按apply 即可。

 ![这里写图片描述](http://img.blog.csdn.net/20170309222627177?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjI3OTcwMzk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


    设置编译级别既 Eclipse compiler compliance level为较低版本，只是让编译器相信你的代码是兼容较低版本的，在编译时生成的bytecode(class)兼容较低版本。
    这样设置与你写代码时引用的JDK是没关系的，也就是说你在写代码时仍可以引用较高版本的API.（这样就可能导致错误）设置compiler compliance level为较低版本，这样的好处是当别人使用了较低版本的Jdk时也可以引用你写的编译后的代码。它可以保证编译后的class文件的版本一致性。但是，如果你的代码里面(java source)里面调用了较高版本jdk的API.那么即使设置了compiler compliance level为较低版本，在较低版本的JDK上运行你的代码也会报错。

参考博文：
http://www.eoeandroid.com/thread-555118-1-1.html?_dsign=2c6b2d33
http://yeungeek.com/2014/09/15/eclipse%E4%B8%AD%E4%BD%BF%E7%94%A8gradle%E6%9E%84%E5%BB%BAandroid/

http://stormzhang.com/android/2016/07/02/gradle-for-android-beginners/


##Eclipse 工程迁移到 AS 遇到的一些问题

----


首先export导出选择生成gradle 文件
![这里写图片描述](http://img.blog.csdn.net/20170309223724401?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjI3OTcwMzk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)



下一步
![这里写图片描述](http://img.blog.csdn.net/20170309224033293?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjI3OTcwMzk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)



然后右键工程项目 点击Gradle – refresh all    ok

修改gradle 版本 和插件版本  为你本地已有的版本


然后直接用AS打开

##遇到的一些问题
```
Error: Found Item Style/AppTheme More than one time
```
原因Res style.xml  中有重复的主题定义

```
AAPT: libpng error: Not a PNG file
Execution failed for task ':app:mergeDebugResources'.
> Some file crunching failed, see logs for details
```

图片问题看log提示
如果有图片的格式是直接通过改后缀名的方式更改为PNG的属性  在eclipse中不能识别区别，但是AS中可以
解决办法  选择文件  编辑----另存为   选择PNG格式
另一种是.9文件造成的导到AS后 原先的.9文件的黑边设置没有了 需要重新设置

```
Android Studio AAPT err: libpng error: Not a PNG file


```
如果找不到图片可以这样解决
在build.gradle 文件中android子集添加如下代码

```
android{
aaptOptions{
    cruncherEnabled = false
}
}

//This is disable AAPT optimization for all of your png files.
```

----






