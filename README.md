# PictureSelectorLight 无裁剪功能版
   最近项目中用到多图选择上传的需求，考虑到android机型众多问题就自己花时间写了一个，测试了大概60款机型，出现过一些问题也都一一修复了，基本上稳定了特分享出来，界面UI也是商用级的开发者不用在做太多修改了，界面高度自定义，可以设置符合你项目主色调的风格，集成完成后就可以拿来用。
  
  项目会一直维护，发现问题欢迎提出会第一时间修复，QQ交流群 619458861，个人QQ 893855882@qq.com  希望用得着的朋友点个start。 
  
  [PictureSelector 2.0完整版](https://github.com/LuckSiege/PictureSelector)
  
  [我的博客地址](http://blog.csdn.net/luck_mw)
  
[![](https://jitpack.io/v/LuckSiege/PictureSelectorLight.svg)](https://jitpack.io/#LuckSiege/PictureSelectorLight)
[![PRs Welcome](https://img.shields.io/badge/PRs-Welcome-brightgreen.svg)](https://github.com/LuckSiege)
[![CSDN](https://img.shields.io/twitter/url/http/blog.csdn.net/luck_mw.svg?style=social)](http://blog.csdn.net/luck_mw)
[![I](https://img.shields.io/github/issues/LuckSiege/PictureSelector.svg)](https://github.com/LuckSiege/PictureSelector/issues)

******功能特点：******  
```
  1.适配android6.0+系统
  2.解决图片过大oom闪退问题
  3.动态获取系统权限，避免闪退
  4.支持相片or视频的单选和多选
  5.支持视频预览
  6.支持gif图片
  7.支持.webp格式图片
  8.支持一些常用场景设置：如:是否预览图片、是否显示相机等
  9.新增自定义主题设置
  10.新增图片勾选样式设置
  11.新增图片压缩处理
  12.新增录视频最大时间设置
  13.新增视频清晰度设置
  14.新增QQ选择风格，带数字效果
  15.新增自定义 文字颜色 背景色让风格和项目更搭配
  16.新增LuBan多图压缩
  17.新增单独拍照功能
  18.新增压缩大小设置
  19.新增Luban压缩档次设置
 
```

******那些遇到拍照闪退问题的同学，请记得看清下面适配6.0的配置~******

重要的事情说三遍记得添加权限

```
  <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
  <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
  <uses-permission android:name="android.permission.CAMERA" />
    
```

******注：适配android6.0以上拍照问题，请在AndroidManifest.xml中添加标签******

```
<provider
   android:name="android.support.v4.content.FileProvider"
   android:authorities="${applicationId}.provider"
   android:exported="false"
   android:grantUriPermissions="true">
     <meta-data
         android:name="android.support.FILE_PROVIDER_PATHS"
         android:resource="@xml/file_paths" />
</provider>

```

******集成步骤******

方式一 compile引入

```
dependencies {
    compile 'com.github.LuckSiege:PictureSelectorLight:v1.1.4'
}

```

项目根目录build.gradle加入 

```
allprojects {
   repositories {
      jcenter()
      maven { url 'https://jitpack.io' }
   }
}
```

方式二 maven引入 

step 1.
```
<repositories>
       <repository>
       <id>jitpack.io</id>
	<url>https://jitpack.io</url>
       </repository>
 </repositories>
```
step 2.
```
<dependency>
   <groupId>com.github.LuckSiege</groupId>
   <artifactId>PictureSelectorLight</artifactId>
   <version>v1.1.4</version>
</dependency>


```

******常见错误*******
```
 问题一：
 rxjava冲突：在app build.gradle下添加
 packagingOptions {
   exclude 'META-INF/rxjava.properties'
 }  
 
 问题二：
 java.lang.NullPointerException: 
 Attempt to invoke virtual method 'android.content.res.XmlResourceParser 
 android.content.pm.ProviderInfo.loadXmlMetaData(android.content.pm.PackageManager, java.lang.String)'
 on a null object reference
 
 application下添加如下节点:
 
 <provider
      android:name="android.support.v4.content.FileProvider"
      android:authorities="${applicationId}.provider"
      android:exported="false"
      android:grantUriPermissions="true">
       <meta-data
         android:name="android.support.FILE_PROVIDER_PATHS"
         android:resource="@xml/file_paths" />
</provider>

注意：如已添加其他sdk或项目中已使用过provider节点，
[请参考我的博客](http://blog.csdn.net/luck_mw/article/details/54970105)的解决方案

问题三：
PhotoView 库冲突，可以删除自己项目中引用的，Picture_library中已经引用过
 
```

******相册启动构造方法******
```
// 进入相册 以下是例子：不需要的api可以不写
   PictureSelector.create(MainActivity.this)
         .openGallery(chooseMode)// 全部.PictureMimeType.ofAll()、图片.ofImage()、视频.ofVideo()
         .theme(themeId)// 主题样式设置 具体参考 values/styles
         .maxSelectNum(maxSelectNum)// 最大图片选择数量
         .minSelectNum(1)// 最小选择数量
         .selectionMode(cb_choose_mode.isChecked() ?
         PictureConfig.MULTIPLE : PictureConfig.SINGLE)// 多选 or 单选
         .previewImage(cb_preview_img.isChecked())// 是否可预览图片
         .previewVideo(cb_preview_video.isChecked())// 是否可预览视频
         .compressGrade(Luban.THIRD_GEAR)// luban压缩档次，默认3档 Luban.FIRST_GEAR、Luban.CUSTOM_GEAR
         .isCamera(cb_isCamera.isChecked())// 是否显示拍照按钮
         .compress(cb_compress.isChecked())// 是否压缩
         .compressMode(compressMode)//系统自带 or 鲁班压缩 PictureConfig.SYSTEM_COMPRESS_MODE or 					LUBAN_COMPRESS_MODE
         .glideOverride(160, 160)// glide 加载宽高，越小图片列表越流畅，但会影响列表图片浏览的清晰度
         .isGif(cb_isGif.isChecked())// 是否显示gif图片
         .openClickSound(cb_voice.isChecked())// 是否开启点击声音
         .selectionMedia(selectList)// 是否传入已选图片
         //.previewEggs(false)// 预览图片时 是否增强左右滑动图片体验(图片滑动一半即可看到上一张是否选中)
         //.isRemove(true)//是否移除图片列表已损坏的图片
         //.compressMaxKB()//压缩最大值kb compressGrade()为Luban.CUSTOM_GEAR有效
         //.compressWH() // 压缩宽高比 compressGrade()为Luban.CUSTOM_GEAR有效
         //.videoQuality()// 视频录制质量 0 or 1
         //.videoSecond()//显示多少秒以内的视频
         .forResult(PictureConfig.CHOOSE_REQUEST);//结果回调onActivityResult code
```

******PictureSelector 2.0 主题配置****** 

```
<!--默认样式 注意* 样式只可修改，不能删除任何一项 否则报错-->
    <style name="picture.default.style" parent="Theme.AppCompat.Light.DarkActionBar">
        <!-- Customize your theme here. -->
        <!--标题栏背景色-->
        <item name="colorPrimary">@color/bar_grey</item>
        <!--状态栏背景色-->
        <item name="colorPrimaryDark">@color/bar_grey</item>
        <!--是否改变图片列表界面状态栏字体颜色为黑色-->
        <item name="picture.statusFontColor">false</item>
        <!--返回键图标-->
        <item name="picture.leftBack.icon">@drawable/picture_back</item>
        <!--标题下拉箭头-->
        <item name="picture.arrow_down.icon">@drawable/arrow_down</item>
        <!--标题上拉箭头-->
        <item name="picture.arrow_up.icon">@drawable/arrow_up</item>
        <!--标题文字颜色-->
        <item name="picture.title.textColor">@color/white</item>
        <!--标题栏右边文字-->
        <item name="picture.right.textColor">@color/white</item>
        <!--图片列表勾选样式-->
        <item name="picture.checked.style">@drawable/checkbox_selector</item>
        <!--开启图片列表勾选数字模式-->
        <item name="picture.style.checkNumMode">false</item>
        <!--选择图片样式0/9-->
        <item name="picture.style.numComplete">false</item>
        <!--图片列表底部背景色-->
        <item name="picture.bottom.bg">@color/color_fa</item>
        <!--图片列表预览文字颜色-->
        <item name="picture.preview.textColor">@color/tab_color_true</item>
        <!--图片列表已完成文字颜色-->
        <item name="picture.complete.textColor">@color/tab_color_true</item>
        <!--图片已选数量圆点背景色-->
        <item name="picture.num.style">@drawable/num_oval</item>
        <!--预览界面标题文字颜色-->
        <item name="picture.ac_preview.title.textColor">@color/white</item>
        <!--预览界面已完成文字颜色-->
        <item name="picture.ac_preview.complete.textColor">@color/tab_color_true</item>
        <!--预览界面标题栏背景色-->
        <item name="picture.ac_preview.title.bg">@color/bar_grey</item>
        <!--预览界面底部背景色-->
        <item name="picture.ac_preview.bottom.bg">@color/bar_grey_90</item>
        <!--预览界面状态栏颜色-->
        <item name="picture.status.color">@color/bar_grey_90</item>
        <!--预览界面返回箭头-->
        <item name="picture.preview.leftBack.icon">@drawable/picture_back</item>
        <!--是否改变预览界面状态栏字体颜色为黑色-->
        <item name="picture.preview.statusFontColor">false</item>
        <!--裁剪页面标题背景色-->
        <item name="picture.crop.toolbar.bg">@color/bar_grey</item>
        <!--裁剪页面状态栏颜色-->
        <item name="picture.crop.status.color">@color/bar_grey</item>
        <!--裁剪页面标题文字颜色-->
        <item name="picture.crop.title.color">@color/white</item>
        <!--相册文件夹列表选中图标-->
        <item name="picture.folder_checked_dot">@drawable/orange_oval</item>
    </style>

```

******启动相册并拍照******       
```
  PictureSelector.create(MainActivity.this)
       .openGallery(PictureMimeType.ofImage())
       .forResult(PictureConfig.CHOOSE_REQUEST);
```

******单独启动拍照或视频 根据type自动识别******       
```
 PictureSelector.create(MainActivity.this)
       .openCamera(PictureMimeType.ofImage())
       .forResult(PictureConfig.CHOOSE_REQUEST);
```
******预览图片******       
```
 // 预览图片 可自定长按保存路径
PictureSelector.create(MainActivity.this).externalPicturePreview(position, "/custom_file", selectList);
PictureSelector.create(MainActivity.this).externalPicturePreview(position, selectList);

```
******预览视频****** 
```
PictureSelector.create(MainActivity.this).externalPictureVideo(video_path);
```
******图片回调完成结果返回******
```
@Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (resultCode == RESULT_OK) {
            switch (requestCode) {
                case PictureConfig.CHOOSE_REQUEST:
                    // 图片选择结果回调
                    selectList = PictureSelector.obtainMultipleResult(data);
                    adapter.setList(selectList);
                    adapter.notifyDataSetChanged();
                    DebugUtil.i(TAG, "onActivityResult:" + selectList.size());
                    break;
            }
        }
    }
    
```

# 更新日志：

###### 版本 v2.0.0
###### PictureSelector 2.0 UI界面大改版
###### PictureSelector 2.0 新增全部模式查询 包括图片or视频
###### PictureSelector 2.0 启动模式由单例模式，改为链式调用
###### UI主题 改为style.xml 配置，各界面随意定制更加方便
###### 优化部分代码和体验去除多余逻辑
###### 重构PictureSelector和urop库关系，解耦两者
###### 修复1.0版本在fragment不回调onActivityResult()

# 项目使用第三方库：
###### 1.glide:3.7.0
###### 2.rxjava:2.0.5
###### 3.rxandroid:2.0.1
###### 4.PhotoView:1.2.4
###### 5.luban

# 混淆配置
```
#PictureSelector 2.0
 -keep class com.luck.picture.lib.** { *; }
   
 #rxjava
-dontwarn sun.misc.**
-keepclassmembers class rx.internal.util.unsafe.*ArrayQueue*Field* {
 long producerIndex;
 long consumerIndex;
}
-keepclassmembers class rx.internal.util.unsafe.BaseLinkedQueueProducerNodeRef {
 rx.internal.util.atomic.LinkedQueueNode producerNode;
}
-keepclassmembers class rx.internal.util.unsafe.BaseLinkedQueueConsumerNodeRef {
 rx.internal.util.atomic.LinkedQueueNode consumerNode;
}

#rxandroid
-dontwarn sun.misc.**
-keepclassmembers class rx.internal.util.unsafe.*ArrayQueue*Field* {
   long producerIndex;
   long consumerIndex;
}
-keepclassmembers class rx.internal.util.unsafe.BaseLinkedQueueProducerNodeRef {
    rx.internal.util.atomic.LinkedQueueNode producerNode;
}
-keepclassmembers class rx.internal.util.unsafe.BaseLinkedQueueConsumerNodeRef {
    rx.internal.util.atomic.LinkedQueueNode consumerNode;
}

#glide
-keep public class * implements com.bumptech.glide.module.GlideModule
-keep public enum com.bumptech.glide.load.resource.bitmap.ImageHeaderParser$** {
  **[] $VALUES;
  public *;
}
```

# 兼容性测试：
******腾讯优测-深度测试-通过率达到100%******

![image](https://github.com/LuckSiege/PictureSelectorLight/blob/master/image/test.png)

# 演示效果：

![image](https://github.com/LuckSiege/PictureSelectorLight/blob/master/image/1.jpg)
![image](https://github.com/LuckSiege/PictureSelectorLight/blob/master/image/2.jpg)
![image](https://github.com/LuckSiege/PictureSelectorLight/blob/master/image/3.jpg)
![image](https://github.com/LuckSiege/PictureSelectorLight/blob/master/image/4.jpg)
![image](https://github.com/LuckSiege/PictureSelectorLight/blob/master/image/white.jpg)
![image](https://github.com/LuckSiege/PictureSelectorLight/blob/master/image/blue.jpg)
![image](https://github.com/LuckSiege/PictureSelectorLight/blob/master/image/7.jpg)
![image](https://github.com/LuckSiege/PictureSelectorLight/blob/master/image/8.jpg)
![image](https://github.com/LuckSiege/PictureSelectorLight/blob/master/image/9.jpg)
![image](https://github.com/LuckSiege/PictureSelectorLight/blob/master/image/10.jpg)

