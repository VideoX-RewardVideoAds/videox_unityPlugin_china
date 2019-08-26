# videox_unityPlugin_china


## 1、VideoXSDK Unity接入

### 1.1、导入VideoXSDK.unitypackage插件

- 有关接入使用方法，可以参考我们提供的包中 demo 文件 Main.cs

### 1.2 Android平台配置

- 请将下面使用到的广告类型脚本挂靠在 unity 主Scene的 Main Camera 上（如下图2所示）。
  
  - `BannerAd`
  - `InterstitialAd`
  - `RewardAd`

### 1.3 iOS平台配置

- 在 Xcode 工程的 *target* -> *General* -> *Embedded Binaries* 中添加 `VideoXSDK.framework` （如下图1所示）。

![embedded binarise](https://github.com/VideoX-RewardVideoAds/videoxdemo_iOS/blob/master/images/embedded_binarise.jpg)<center>( 图 1 )</center>

- 请将下面使用到的广告类型脚本挂靠在 unity 主Scene的 Main Camera 上（如下图2所示）。
  - `VXBannerAdsClient_iOS`
  - `VXInterstitialAdsClient_iOS` 
  - `VXRewardAdsClient_iOS`
  
  >需要挂载才能正确接受iOS端回调信息。

  
![unity_script](https://github.com/VideoX-RewardVideoAds/videoxdemo_iOS/blob/master/images/unity_script.jpg)
 <center>( 图 2 )</center>
## 2、SDK初始化

### 2.1 初始化SDK

初始化方法：

```javascript
VXAds.InitializeSDK("Main Camera","appId", "pubKey")
```

- `gameObjectString`: unity 主Scene下的 Main Camera 名称

- `appId`: 应用标识id，请在app page查找

- `pubKey`: 加密秘钥，请在account page查找


### 2.2 设置获取SDK方法回调

```javascript
//激励视频
VXRewardsAds.onLoadSuccessHandler += rewardAdOnLoadSuccess;
VXRewardsAds.onLoadErrorHandler   += rewardAdOnLoadError;
VXRewardsAds.onShowHandler        += rewardAdOnShowStart;
VXRewardsAds.onCloseHandler       += rewardAdOnShowFinish;
VXRewardsAds.onShowErrorHandler   += rewardAdOnShowError;
//插屏
VXInterstitialAds.onLoadSuccessHandler += interAdOnLoadSuccess;
VXInterstitialAds.onLoadErrorHandler += interAdOnLoadError;
VXInterstitialAds.onCloseHandler += interAdOnClose;
VXInterstitialAds.onClickedHandler += interAdOnClicked;
//横幅
VXBannerAds.onLoadSuccessHandler += bannerAdOnLoadSuccess;
VXBannerAds.onLoadErrorHandler += bannerAdOnLoadError;
VXBannerAds.onClickedHandler += bannerAdOnClicked;
VXBannerAds.onCloseHandler += bannerAdOnClose;
```

激励视频

- VXRewardsAds.onLoadSuccessHandler(string adUnitId): 激励视频广告加载成功回调
- VXRewardsAds.onLoadErrorHandler(string adUnitId, string error): 激励视频加载失败回调
- VXRewardsAds.onShowHandler(string adUnitId): 激励视频展示开始回调
- VXRewardsAds.onCloseHandler(string adUnitId, bool isReward): 激励视频展示完毕回调
- VXRewardsAds.onShowErrorHandler(string adUnitId, string error): 激励视频展示错误回调

插屏

- VXInterstitialAds.onLoadSuccessHandler(string adUnitId): 插屏广告加载成功回调
- VXInterstitialAds.onLoadErrorHandler(string adUnitId, string error): 插屏广告加载或展示失败回调
- VXInterstitialAds.onCloseHandler(string adUnitId): 插屏广告用户关闭离开回调
- VXInterstitialAds.onClickedHandler(string adUnitId): 插屏广告用户点击广告回调

横幅

- VXBannerAds.onLoadSuccessHandler(string adUnitId): 横幅广告加载成功回调
- VXBannerAds.onLoadErrorHandler += bannerAdOnLoadError: 横幅广告加载失败回调
- VXBannerAds.onClickedHandler(string adUnitId): 横幅广告点击回调
- VXBannerAds.onCloseHandler(string adUnitId, string error): 横幅广告关闭回调

**注意：**
>如果您的项目同时集成了 `AppsFlyer` SDK，您需要：
>
- 找到 AppsFlyer包含的 `AppsFlyerTrackerCallbacks.cs` 文件,并在文件中的回调方法 `didReceiveConversionData` 中实现如下代码：
>
 ```javascript
 public void didReceiveConversionData(string conversionData)
    {
        VXAds.SetAppsFlyerConversionData(conversionData);
    }
 ```


## 3、广告功能

### 3.1 激励视频广告

##### 3.1.1 请求加载广告

```javascript
VXRewardsAds.LoadRewardAd("Main Camera", adUnitId);
```

##### 3.1.2 判断广告是否加载成功

```javascript
bool value = VXRewardsAds.IsAdReady("Main Camera", adUnitId);
```

##### 3.1.3 展示广告

```javascript
VXRewardsAds.PlayRewardAd("Main Camera",adUnitId);
```

- `gameObjectString`: unity 主Scene下的 Main Camera 名称
- `adUnitId`: 广告位id, 由接入方运营负责人统一申请。


##### 注意:

> - 为了更好的用户体检，强烈建议在每次需要展示激励视频广告之前，先调用 `VXRewardsAds.LoadRewardAd("Main Camera", adUnitId)` 做好视频缓存的准备，避免展示时出现加载等待时间。
> - *禁止在 rewardAdOnLoadSuccess 中 调用 VXRewardsAds.PlayRewardAd 方法*
> - *禁止在 rewardAdOnLoadError 中 调用 VXRewardsAds.LoadRewardAd 方法*

### 3.2 插屏广告

##### 3.2.1 请求加载广告

```javascript
VXInterstitialAds.LoadAd("Main Camera", adUnitId);
```

##### 3.2.2 判断广告是否加载成功

```javascript
bool value = VXInterstitialAds.IsAdReady("Main Camera", adUnitId);
```

##### 3.2.3 展示广告

```javascript
VXInterstitialAds.ShowAd("Main Camera", adUnitId);
```

- `gameObjectString`: unity 主Scene下的 Main Camera 名称
- `adUnitId`: 广告位id，请在app page查找

### 3.3 横幅广告

##### 3.3.1 加载并显示广告

```javascript
VXBannerAds.ShowAd("Main Camera", adUnitId, BannerAd.POSTION_BOTTOM);
```

##### 3.3.2 关闭广告

```javascript
VXBannerAds.DestroyAd("Main Camera", adUnitId);
```


- `gameObjectString`: unity 主Scene下的 Main Camera 名称
- `adUnitId`: 广告位id，请在app page查找
- `position`: 广告的位置 ，有两个选项 BannerAd.POSTION*BOTTOM 和 BannerAd.POSTION*TOP , 分别为在屏幕底端和屏幕顶端。


**Android 注意**

- *Android打包需注意在 PlayerSettings 的 Android 标签页下 Publishing Settings 中 勾选 "Use Proguard File" 和 "Custom Gradle Template" 选项, 且在 Manify 中给 Release 选择 Proguard*

- 如果遇到在 Android5.0 版本以下手机上运行崩溃的现象(not found class), 请按照以下要求做:
  - 在打包 android apk 的时候导出 android 项目，在 [UnityPlayerActivity] 类所在目录下新建一个类 [MainApp]
  - 打开 MainApp 让这个类继承自 [MultiDexApplication]，例如：`public class MainApp extends MultiDexApplication { }`
  - 打开 [AndroidManifest.xml] 然后在 Application 节点新增一个 name 属性，例如`<application android:name=".MainApp" android:theme="@style/UnityThemeSelector" ....` ，这个属性的值为上述新建的类


