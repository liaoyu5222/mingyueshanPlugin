# mingyueshanPlugin
共建明月山安卓原生module插件使用方法：
1.此插件需要uniapp集成oAuth微信登录模块用于插件内跳转小程序,还需集成高德地图定位模块用于天气定位，需要集成videPlayer视频用于插件顶部banner视频播放，需要集成Payment(支付)中的微信支付和支付宝支付模块，需要集成share(分享)中的微信分享模块
2.在需要集成插件的uniapp的mainfest.json源码配置视图中的app-plus节点下的android节点下增加如下属性：
 "buildFeatures" : {
                    "dataBinding" : true, //开启dataBinding
                    "viewBinding" : true //开启viewBinding
                }
此属性用于开启视图绑定。还需增加一个  "targetSdkVersion" : 29  属性，因为插件中内嵌智慧大竹app中获取权限最低安卓版本API要求29。

3. 此插件有两个可被uniapp调用的方法
方法1
   @UniJSMethod(uiThread = true)
    public void qiFuWeb(String wx_appid,String url,String title){
        ActivityUtil.goToModuleWeb(mUniSDKInstance.getContext(),wx_appid,url,title);
    }
 这个方法可以用来跳转任意web页面。
 
 方法2
    @UniJSMethod(uiThread = true)
    public void gotoNativePage(String wx_appid,String wx_secret,String tag,String phone){
        if(mUniSDKInstance != null) {
            Intent intent = new Intent(mUniSDKInstance.getContext(), MysSplashActivity.class);
            Bundle bundle=new Bundle();
            bundle.putString("wx_appid",wx_appid);
            bundle.putString("wx_secret",wx_secret);
            bundle.putString("phone",phone);
            bundle.putString("verify",tag);
            intent.putExtras(bundle);
            mUniSDKInstance.getContext().startActivity(intent);
        }
    }
    此方法用于跳转插件首页，如果第一个参数-微信appid和第二个参数-微信secret传进来将可以用于插件内小程序的跳转以及微信相关的支付，第三个参数-tag和第四个参数-phone传进来将实现插件的自动登录，如想获取tag的值请联系插件作者 QQ：2326313957
