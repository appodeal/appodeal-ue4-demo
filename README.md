#Appodeal Unreal Engine 4 Plugin

## Download SDK

[![](https://img.shields.io/badge/download-link-green.svg)](https://www.dropbox.com/s/d909mtodbt90epr/Appodeal-Android-UE4-IRVB-nodex-280617-2.0.2-U4.16.zip?dl=1) for UE 4.16
[![](https://img.shields.io/badge/download-link-green.svg)](https://www.dropbox.com/s/sdibb3huz0k1nvo/Appodeal-Android-UE4-IRVB-nodex-280617-2.0.2-U4.15.zip?dl=1) for UE 4.15 and below

###Requirements:

+ Unreal Engine 4.10+
+ Android 9+
+ iOS 8.0+

## Unreal Engine Integration

### Plugin Files

Place files from archive to your projects `plugin` folder. It must look like `<project_folder>/Plugins/Appodeal/`.
Then run `<project_name>.uproject` file, it will ask you to build binnaries for Appodeal Plugin, press yes.

If you have any problem with buildig, try to remove `Intermediate` and `Binaries` folders from your project folder.

### Unreal Engine Project Configuration

Now you'll need to configure your project. 

#### Android: 

Open `File-> Package Project -> Packaging Settings...` and open `Android` in `Platforms` section.

Then check settings there:

+ Minimum SDK Version should be at least 9. 
+ Target SDK is better to set 24 or above.
+ Android Package Name should be the same as you've set in Appodeal when created app.
+ Add Extra Permissions: 
    + android.permission.ACCESS_NETWORK_STATE
    + android.permission.INTERNET
+ Optional Extra Permissions for better earnings:
    + android.permission.ACCESS_COARSE_LOCATION
    + android.permission.WRITE_EXTERNAL_STORAGE
    + android.permission.ACCESS_WIFI_STATE
+ Google Play Services should be enabled, because Appodeal requires it.

If you see toast message "AdMob not found" in your game and you use Unreal Engine 4.18, you should edit the following file: Plugins/Appodeal/Source/Appodeal/Appodeal_APL.xml

Scroll to line 13 (<AARImports>) ad add the following lines:

```
<insertValue value="com.google.android.gms,play-services-ads,11.0.4"/>
<insertNewline/>
<insertValue value="com.google.android.gms,play-services-ads-lite,11.0.4"/>
<insertNewline/>
```

Check Android SDK settings, Android SDK, NDK and ANT shoud be configured.

Appodeal plugin depends on [MultiDex](https://www.unrealengine.com/marketplace/multidex) plugin. Without this plugin you probably won't be able to build your game for Android. Please read installation guide carefully.

If you’ve encountered the java.lang.OutOfMemoryError it means the java virtual machine has not enough memory to build the code. To increase it set up the environment variable _JAVA_OPTIONS with the value -Xmx1024m -Xms256m -Xss8m.

You might need to increase the -Xmx value to something between 1024m and 2048m.

#### iOS:

Open `File-> Package Project -> Packaging Settings...` and open `iOS` in `Platforms` section.

+ Go to the build settings and set one Additional Linker Flag: `-ObjC`

### Code Integration

#### Ad Types

You can use following ad types:
+ Interstitial
+ Banner
+ Rewarded Video
+ Non Skippable Video
Most of Appodeal Blueprint methods are asking which ones you want to use.

#### Initialization

Please add Appodeal Component somewhere on your actor, to be able to access Events. Also Appodeal Component allows you to configure initialization, auto cache, and some additional parameters without calling functions in buleprint.

All Appodeal actions is under `Actions -> Advertising -> Appodeal`

To Initialize Appodeal SDK, add InitializeWithParameters action somewhere on the app start (best place to do that is on `BeginPlay Event` in starting Scene) then configure app keys (you can find it in App settings in Appodeal dashboard) and check ad types you want to initialize.

Alternatively, if you have added Appodeal component, you can configure parameters of the Appodeal plugin in `Details` tab of the component. 
[![](https://www.dropbox.com/s/p61he1n63umywf0/ue4_component_settings.png?raw=1)]()
After that just add Initialize action with Appodeal component as a target. All settings will be applied automatically.
[![](https://www.dropbox.com/s/ayw70z9v4et0abv/ue4_initialize.png?raw=1)]()

#### Display ad

To show ads use Show action where you want and check ad types you want see.

Also you can combine some ad types, for example, Interstitial and Non Skippable Video. But it is not reccomended to use fullscreen ad types with banner ads.

#### Hiding banner

To hide banner use Hide Banner action.

### Advanced Features

#### Samples

To show ad after it was loaded, for example Interstitial ad:
[![](https://www.dropbox.com/s/g3zb1b4tsuknn0h/ue4_show_after_loaded.png?raw=1)]()

To show ad right after initialization, for example interstitial ad:
[![](https://www.dropbox.com/s/3ql5or9p4fu6hlm/ue4_show_after_start.png?raw=1)]()


To enable test mode use `Set Testing` action


To enable additional logging use `Set Logging` action


To check if ad is loaded use `Is Loaded` action, it will return boolean value for selected ad type.


You can use Appodeal Events for each ad types, it is accessible from Appodeal Component.

##### Interstitial Events:
+ On Interstitial Loaded
+ On Interstitial Falied To Load
+ On Interstitial Clicked
+ On Interstitial Shown
+ On Interstitial Closed

##### Banner Events:
+ On Banner Loaded
+ On Banner Falied To Load
+ On Banner Clicked
+ On Banner Shown

##### Non Skippable Video Events:
+ On Non Skippable Video Loaded
+ On Non Skippable Video Falied To Load
+ On Non Skippable Video Finished
+ On Non Skippable Video Shown
+ On Non Skippable Video Closed

##### Rewarded Video Events:
+ On Rewarded Video Loaded
+ On Rewarded Video Falied To Load
+ On Rewarded Video Finished (also returns reward amount and reward name from Segment Settings)
+ On Rewarded Video Shown
+ On Rewarded Video Closed


If you want to use manual ad caching, disable it before Initialization using `Disable Auto Cache` action.


Then use `Cache` action where you want to cache ads.


To disable network use `Disable Network` action, you can fill network name tere and choose ad type you want to disable it for. 

Available parameters: "adcolony", "admob", "amazon_ads", "applovin", "avocarrot", "chartboost", "facebook", "flurry", "inmobi", "inner-active", "ironsource", "mailru", "mopub", "ogury", "openx", "pubnative", "smaato", "startapp", "tapjoy", "unity_ads", "vungle", "yandex".


To disable ACCESS_COARSE_LOCATION permission usage and toast message for missing permission use `Disable Location Permission Check` action.


`Track In App Purchase` action tracks in-app purchase information and sends info to our servers for analytics.

## Changelog:

2.0.2 (28.06.2017)
+ Appodeal Android SDK updated to 2.0.2
+ Appodeal.confirm removed
+ Interstitials and skippable videos are merged into one type: Appodeal.INTERSTITIAL
+ The plugin depends on MultiDex plugin https://www.unrealengine.com/marketplace/multidex

1.0.1 (03.11.2016)

+ Android 1.15.7
+ Initialize -> Initialize With Parameters
+ Confugured parameters for Appodeal Component
+ Appodeal Disable Banner Animation action added
+ Appodeal Disable 728x90 Banners action added
+ Appodeal Track In App Purchase action fixed

1.0.0 (12/09/2016)

+ Plugin release
+ Android 1.15.3
+ iOS 0.10.9
