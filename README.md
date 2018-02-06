# 微信支付官方DEMO

### 由于模拟器不能调起微信支付，所以用手机截几张图，可以看到，可以成功调起支付，并能成功支付
<img src="https://github.com/ainiyiwan/WeChatSDK_sample_Android/blob/master/png/1.png" width = "250"/> <img src="https://github.com/ainiyiwan/WeChatSDK_sample_Android/blob/master/png/2.png" width = "250"/> <img src="https://github.com/ainiyiwan/WeChatSDK_sample_Android/blob/master/png/3.png" width = "250"/>

## 集成过程(如果你有任何疑惑，可以提issue)

### 一 添加依赖
官方文档见[这里](https://open.weixin.qq.com/cgi-bin/showdocument?action=dir_list&t=resource/res_list&verify=1&id=open1419319167&token=&lang=zh_CN)
```java
compile 'com.tencent.mm.opensdk:wechat-sdk-android-without-mta:+'
```

### 二 注册APPID
官方文档见[这里](https://pay.weixin.qq.com/wiki/doc/api/app/app_sl.php?chapter=8_5)
```java
final IWXAPI msgApi = WXAPIFactory.createWXAPI(context, null);

// 将该app注册到微信

msgApi.registerApp("wxd930ea5d5a258f4f");
```
### 三 配置签名
配置你在微信开放平台注册时填写的签名
```java
signingConfigs {
        debug {
            storeFile file("你的keystore路径")
            storePassword "xxx"
            keyAlias "xxx"
            keyPassword "xxx"
        }
        release {
            storeFile file("你的keystore路径")
            storePassword "xxx"
            keyAlias "xxx"
            keyPassword "xxx"
        }
}
```
### 四 调起支付
详细用法见项目PayActivity，建议支付前先检查微信版本是否支持支付
```java
//检查微信版本是否支持支付
boolean isPaySupported = api.getWXAppSupportAPI() >= Build.PAY_SUPPORTED_SDK_INT;
```
### 五 支付回调
>参照微信SDK Sample，在net.sourceforge.simcpux.wxapi包路径中实现WXPayEntryActivity类(包名或类名不一致会造成无法回调)，在WXPayEntryActivity类中实现onResp函数，支付完成后，微信APP会返回到商户APP并回调onResp函数，开发者需要在该函数中接收通知，判断返回错误码，支付成功则调用“服务商提供的查询接口”获取到支付状态再展示用户实际支付结果。注意一定不能以客户端返回作为用户支付的结果，应以服务器端的接收的支付通知或查询API返回的结果为准。

以上这句话取自[这里](https://pay.weixin.qq.com/wiki/doc/api/app/app_sl.php?chapter=8_5)

```java
@Override
	public void onResp(BaseResp resp) {
		Log.d(TAG, "onPayFinish, errCode = " + resp.errCode);

		if (resp.getType() == ConstantsAPI.COMMAND_PAY_BY_WX) {
//			AlertDialog.Builder builder = new AlertDialog.Builder(this);
//			builder.setTitle(R.string.app_tip);
//			builder.setMessage(getString(R.string.pay_result_callback_msg, String.valueOf(resp.errCode)));
//			builder.show();
			//参考这里 https://www.jianshu.com/p/c97639279d2e
			if (resp.errCode == 0){
				Toast.makeText(this, "支付成功", Toast.LENGTH_SHORT).show();
			}else {
				Toast.makeText(this, "支付失败，请重试", Toast.LENGTH_SHORT).show();
			}
			finish();
		}
	}
```
### Manifest和网络权限不要忘记配置哦


### 福利
**关于自定义回调见[这篇文章](https://www.jianshu.com/p/c97639279d2e)**

## 如果以上都配置了还是不成功，请给我提Issue


