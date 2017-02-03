### Copy Files
Copy the following __jar__ files from `plugin/android/libs` folder of this
bundle into your project’s __proj.android/libs__ folder.

> PluginAppodeal.jar

> sdkbox.jar

> android-support-v4-22.2.1.jar

> applovin-sdk-6.0.1.jar

> appodeal-1.13.1.jar

> chartboost-5.2.0.jar

> my-target-4.0.13.jar

> unity-ads-1.4.7.jar



<<[../../shared/copy_jni_lib.md]


### Edit `AndroidManifest.xml`
Include the following permissions above the __application tag__:
```xml
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
```

There are also necessary meta-data and activity tags that also need to be added:
```xml
<meta-data android:name="com.appodeal.framework" android:value="sdkbox" />
<activity android:name="com.appodeal.ads.InterstitialActivity"
        android:configChanges="orientation|screenSize"
        android:theme="@android:style/Theme.Translucent.NoTitleBar" />
<activity android:name="com.appodeal.ads.VideoActivity"
        android:configChanges="orientation|screenSize"
        android:theme="@android:style/Theme.Translucent.NoTitleBar" />

<activity android:name="com.appodeal.ads.LoaderActivity"
        android:configChanges="orientation|screenSize"
        android:theme="@android:style/Theme.Translucent.NoTitleBar" />

<meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version" />

<activity android:name="com.google.android.gms.ads.AdActivity"
        android:configChanges="keyboard|keyboardHidden|orientation|screenLayout|uiMode|screenSize|smallestScreenSize"
        android:theme="@android:style/Theme.Translucent" />

<activity android:name="com.chartboost.sdk.CBImpressionActivity"
        android:theme="@android:style/Theme.Translucent"
        android:excludeFromRecents="true" />

<activity android:name="com.applovin.adview.AppLovinInterstitialActivity"
        android:theme="@android:style/Theme.Translucent" />

<activity android:name="com.mopub.mobileads.MoPubActivity"
        android:configChanges="keyboardHidden|orientation|screenSize"
        android:theme="@android:style/Theme.Translucent" />
<activity android:name="com.mopub.common.MoPubBrowser"
        android:configChanges="keyboardHidden|orientation|screenSize" />
<activity android:name="com.mopub.mobileads.MraidActivity"
        android:configChanges="keyboardHidden|orientation|screenSize" />
<activity android:name="com.mopub.mobileads.MraidVideoPlayerActivity"
        android:configChanges="keyboardHidden|orientation|screenSize" />

<activity android:name="org.nexage.sourcekit.mraid.MRAIDBrowser"
        android:configChanges="orientation|keyboard|keyboardHidden|screenSize"
        android:theme="@android:style/Theme.Translucent" />

<activity android:name="com.amazon.device.ads.AdActivity"
        android:configChanges="keyboardHidden|orientation|screenSize"/>

<activity android:name="com.unity3d.ads.android.view.UnityAdsFullscreenActivity"
        android:configChanges="fontScale|keyboard|keyboardHidden|locale|mnc|mcc|navigation|orientation|screenLayout|screenSize|smallestScreenSize|uiMode|touchscreen"
        android:theme="@android:style/Theme.NoTitleBar.Fullscreen"
        android:hardwareAccelerated="true" />

<activity android:name="ru.mail.android.mytarget.ads.MyTargetActivity"
        android:configChanges="keyboard|keyboardHidden|orientation|screenLayout|uiMode|screenSize|smallestScreenSize"/>

<activity android:name="org.nexage.sourcekit.vast.activity.VASTActivity"
        android:theme="@android:style/Theme.NoTitleBar.Fullscreen" />

<activity android:name="com.facebook.ads.InterstitialAdActivity"
        android:configChanges="keyboardHidden|orientation|screenSize" />

<activity android:name="com.jirbo.adcolony.AdColonyOverlay"
        android:configChanges="keyboardHidden|orientation|screenSize"
        android:theme="@android:style/Theme.Translucent.NoTitleBar.Fullscreen" />
<activity android:name="com.jirbo.adcolony.AdColonyFullscreen"
        android:configChanges="keyboardHidden|orientation|screenSize"
        android:theme="@android:style/Theme.Black.NoTitleBar.Fullscreen" />
<activity android:name="com.jirbo.adcolony.AdColonyBrowser"
        android:configChanges="keyboardHidden|orientation|screenSize"
        android:theme="@android:style/Theme.Black.NoTitleBar.Fullscreen" />

<activity android:name="com.vungle.publisher.FullScreenAdActivity"
        android:configChanges="keyboardHidden|orientation|screenSize"
        android:theme="@android:style/Theme.NoTitleBar.Fullscreen"/>
```

### Edit `Android.mk`
Edit `proj.android/jni/Android.mk` to:

Add additional requirements to __LOCAL_WHOLE_STATIC_LIBRARIES__:
```
LOCAL_WHOLE_STATIC_LIBRARIES += android_native_app_glue
LOCAL_LDLIBS += -landroid
LOCAL_LDLIBS += -llog
LOCAL_WHOLE_STATIC_LIBRARIES += PluginAppodeal
LOCAL_WHOLE_STATIC_LIBRARIES += sdkbox
```

Add a call to:
```
$(call import-add-path,$(LOCAL_PATH))
```
before any __import-module__ statements.

Add additional __import-module__ statements at the end:
```
$(call import-module, ./sdkbox)
$(call import-module, ./pluginappodeal)
```

This means that your ordering should look similar to this:
```
$(call import-add-path,$(LOCAL_PATH))
$(call import-module, ./sdkbox)
$(call import-module, ./pluginappodeal)
```

### Edit `Application.mk`
Edit `proj.android/jni/Application.mk` to:

Add __APP_PATFORM__ version requirements:
```
APP_PLATFORM := android-9
```
