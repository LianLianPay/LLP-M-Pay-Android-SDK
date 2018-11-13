# LLP-M-Pay-Android-SDK
连连支付统一网关Android SDK接入指南
本指南为连连⽀付统一网关SDK接入指南， 阅读对象为接入 PayTokenSDK 的开发者.

1.	SDK文件说明

pay-token-demo	接入Demo，Android Studio工程
base-xxx.aar	SDK库包
mobilebank- xxx.aar	SDK库包
securepay- xxx.aar	SDK库包
连连支付接入指南	接入说明
CustomConfig.properties	客户定制化配置的文件
（xxx为当前sdk版本号，请保持三个库的版本号一致）

2.	集成SDK

1.	将上述的三个aar文件放入工程的libs文件夹
2.	在gradle中添加

repositories {
        flatDir {
            dirs 'libs'
        }
}
3.	在gradle的dependencies{ }中添加：
compile (name:'securepay-xxx',ext:'aar')
compile (name:'base- xxx'',ext:'aar')
compile (name:'mobilebank- xxx'',ext:'aar')
（xxx为当前sdk版本号，请保持三个库的版本号一致）
4.	更改 AndroidManifest.xml 文件
增加如下权限：
   
为了增加安全控制系数，商户可根据需要，添加获取地理位置的权限，没有配置该权限，SDK不会进行位置信息的获取动作。在配置该权限后，部分手机在用户进入支付时，会弹框提示用户应用程序会获取用户位置信息（如小米3），如果业务风险控制严格的请评估用户体验影响后自行配置该功能。如需要配置，添加以下权限：
 
SDK支持部分手机的短信码自动读取功能，如果需要使用，需要添加以下权限：
 
由于各手机厂家对Android原生系统的定制不同，短信的读取规则也不一样。SDK支持以下两种短信读取：
(1).原生系统4.4以下版本的，可以在AndroidManifest.xml中配置权限"android.permission.RECEIVE_SMS"即可。
(2).如果想支持类似小米系统的和Android4.4以上版本的，需要配置"android.permission.READ_SMS"。此权限（可以读取用户短信）有可能会被某些安全软件认为是较危险的，请添加时自行评估。
小米等深度定制手机需要针对该应用开启读取短信功能方可读取。小米设置路径：设置--> 应用--> 该应用--> 权限管理--> 短信记录--> 允许。
三星Galaxy S5 等三星手机4.4以上系统版本的，如果配置了"android.permission.READ_SMS"，可能存在必须允许打开读取权限方可使用的问题，请测试后自行评估是否配置该权限。
注意，部分手机安装了安全工具，有可能会拦截到短信，导致不能自动读取。另外手机厂商对手机短信的安全控制各不相同，不可能适配所有机器。

3.	调用方式

参考Demo调用相应的接口完成相应操作：
1.	支付
首先商户根据demo中的getPayToken（JsonObject oj）进行创单，成功之后取出返回报文中的” gateway_url”，然后调用  SecurePayService.securePay（）方法进性后续功能。
2.	签约
首先商户根据demo中的getSignToken（JsonObject oj）进行创单，成功之后取出返回报文中的” gateway_url”，然后调用 SecurePayService.secureSign（）方法进性后续功能。
3.	参数介绍：
RequestItem: 商户调用连连SDK时的订单信息，Demo中仅供参考，商户需根据接口文档进行完善。

4.	自定义说明

SDK提供部分可定制化的设置，包括标题头颜色、高度等等，如果需要定制，请修改这个文件，并将其放入assets目录下（注意格式UTF-8）。具体修改字段说明，请参考CustomConfig.properties各个字段的说明。比如，要修改标题高度和字体大小，直接改下面变量值即可：
  
如果需要修改图片，可以直接用压缩工具将SecurePay-***.aar 打开，图片在assets目录里面，直接替换掉就可以了。

5.	混淆说明
在混淆文件中添加以下混淆规则：

-keep class com.lianlian.** {*;}
-keep class com.icbc.pay.** {*;}   

6.	注意事项
Demo中Constants.java有配置密钥，为测试用的。建议商户将密钥配置到服务器端避免泄漏。

