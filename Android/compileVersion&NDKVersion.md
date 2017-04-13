Error:Execution failed for task ':hello_motion_tracking:transformNative_libsWithStripDebugSymbolForDebug'


###环境：
* mac
* Android Studio 2.3
* NDK r14

原因是targetSdkVersion & comipleSdkVersion 的版本太低 改为22 就可以了

网络上其他一些解决办法可能对低版本的有用 
将 local.properties 中的 ndk.dir 注释或者删除掉 将项目根目录的 .gradle 删除掉 然后重新构建。


---

Warning:The `android.dexOptions.incremental` property is deprecated and it has no effect on the build

解决办法
删除  gradle 中 dexoptions
在新的android studio 中不需要这个选项 2.2