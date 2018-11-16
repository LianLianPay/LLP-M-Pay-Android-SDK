# LLP-M-Pay-Android-SDK

# 连连支付统一网关 Android SDK 接入指南 

> 本指南为连连支付统一网关 SDK 接入指南， 阅读对象为接入 PayTokenSDK 的开发者. 
  

## 一. SDK 文件说明

|文件名|                       说明|
|------------------                     |-------------------                        |
|pay-token-demo               |     接入 Demo，Android Studio 工程  |
|base-xxx.aar                        |SDK 库包  |
|mobilebank- xxx.aar                               |SDK 库包  |
|securepay- xxx.aar     |  SDK 库包 |
|连连支付接入指南                        |接入说明    |
|CustomConfig.properties               |    客户定制化配置的文件  |
（xxx 为当前 sdk 版本号，请保持三个库的版本号一致） 

## 二. 集成 SDK



1. 将上述的三个 aar 文件放入工程的 libs 文件夹 
2. 在 gradle 中添加   
```objc
    repositories {   

        flatDir {  
            dirs 'libs' 
        }   
    } 
```
3. 在 gradle 的 dependencies{ }中添加：   
```objc
     compile (name:'securepay-xxx',ext:'aar')  
     compile (name:'base- xxx'',ext:'aar')  
     compile (name:'mobilebank- xxx'',ext:'aar') 
```
     （xxx 为当前 sdk 版本号，请保持三个库的版本号一致）  
4. 更改 AndroidManifest.xml 文件 增加如下权限： 
```objc  
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```
  为了增加安全控制系数，商户可根据需要，添加获取地理位置的权限，没 有配置该权限，
SDK 不会进行位置信息的获取动作。在配置该权限后，部分手机在用户进入支付时，会弹框
提示用户应用程序会获取用户位置信息（如小米 3），如果业务风险控制严格的请评估用户
体验影响后自行配置该功能。如需要配置，添加以下权限： 
  ```objc  
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
  ```
  SDK 支持部分手机的短信码自动读取功能，如果需要使用，需要添加以下权限： 
 ```objc  
    <uses-permission android:name="android.permission.READ_SMS" />
    <uses-permission android:name="android.permission.RECEIVE_SMS" />  
 ```
 由于各手机厂家对 Android 原生系统的定制不同，短信的读取规则也不一样。SDK 支持
以下两种短信读取：   
(1). 原 生 系 统 4.4 以 下 版 本 的 ， 可 以 在 AndroidManifest.xml 中 配 置 权 限
"android.permission.RECEIVE_SMS"即可。   
(2). 如 果 想 支 持 类 似 小 米 系 统 的 和 Android4.4 以 上 版 本 的 ， 需 要 配 置
"android.permission.READ_SMS"。此权限（可以读取用户短信）有可能会被某些安全软件认
为是较危险的，请添加时自行评估。 
小米等深度定制手机需要针对该应用开启读取短信功能方可读取。小米设置路径：设置
--> 应用--> 该应用--> 权限管理--> 短信记录--> 允许。 
三星 Galaxy S5 等 三 星 手 机 4.4 以 上 系 统 版 本 的 ， 如 果 配 置 了
"android.permission.READ_SMS"，可能存在必须允许打开读取权限方可使用的问题，请测试
后自行评估是否配置该权限。 
注意，部分手机安装了安全工具，有可能会拦截到短信，导致不能自动读取。另外手机
厂商对手机短信的安全控制各不相同，不可能适配所有机器。 


## 三.  调用方式     
   

  
 > 参考 Demo 调用相应的接口完成相应操作：   
  
 1. 支付 首先商户根据 demo 中的 getPayToken（JsonObject oj）进行创单，成功之后取出 返回报文中的” gateway_url”，然后调用  SecurePayService.securePay（）方法进性后 续功能。   
 2. 签约 首先商户根据 demo 中的 getSignToken（JsonObject oj）进行创单，成功之后取 出返回报文中的” gateway_url”，然后调用 SecurePayService.secureSign（）方法进性 后续功能。  
 3. 参数介绍： 
RequestItem: 商户调用连连 SDK 时的订单信息，Demo 中仅供参考，商户需根据
接口文档进行完善。 



## 四.  自定义说明 
  
SDK 提供部分可定制化的设置，包括标题头颜色、高度等等，如果需要定制，请修
改这个文件，并将其放入 assets 目录下（注意格式 UTF-8）。具体修改字段说明，请参考
CustomConfig.properties 各个字段的说明。比如，要修改标题高度和字体大小，直接改
下面变量值即可： 
  ```objc  
标题部分 标题背景色
ll_title_bg = #028ad7
标题文字色
ll_title_text_color=#FFFFFFFF
 ```
 如果需要修改图片，可以直接用压缩工具将 SecurePay-***.aar 打开，图片在 assets
目录里面，直接替换掉就可以了。 
## 五. 混淆说明 

在混淆文件中添加以下混淆规则： 
   ```objc 
-keep class com.lianlian.** {*;} 
-keep class com.icbc.pay.** {*;} 
```
## 六. 注意事项 
Demo 中 Constants.java 有配置密钥，为测试用的。建议商户将密钥配置到服务器端避
免泄漏。  
## License

© 2003-2018 Lianlian Yintong Electronic Payment Co., Ltd. All rights reserved.
