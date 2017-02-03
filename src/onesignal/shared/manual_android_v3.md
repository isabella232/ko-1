### Copy Files
Copy the everything from `plugin/android/libs` folder of this
bundle into your project’s __<project_root>/libs__ folder.


* If you're using cocos2d-x from source copy the __jar__ files to:

```
cocos2d/cocos/platform/android/java/libs
```

* If you're using cocos2d-js or lua copy the __jar__ files to:

```
frameworks/cocos2d-x/cocos/platform/android/java/libs
```

* If you're using prebuilt cocos2d-x copy the __jar__ files to:

```
<project_root>/libs
```

<<[../../shared/copy_jni_lib.md]


### Edit `AndroidManifest.xml`
Include the following permissions above the __application tag__:
```xml
<!--Mandatory permissions to receive ads-->
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>

<permission android:name="__manifestApplicationId__.permission.C2D_MESSAGE"
            android:protectionLevel="signature" />
<uses-permission android:name="__manifestApplicationId__.permission.C2D_MESSAGE" />
<uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
<uses-permission android:name="android.permission.WAKE_LOCK" />
<uses-permission android:name="android.permission.VIBRATE" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.CHANGE_WIFI_STATE" />

<!-- START: ShortcutBadger -->
<!--for Samsung-->
<uses-permission android:name="com.sec.android.provider.badge.permission.READ"/>
<uses-permission android:name="com.sec.android.provider.badge.permission.WRITE"/>

<!--for htc-->
<uses-permission android:name="com.htc.launcher.permission.READ_SETTINGS"/>
<uses-permission android:name="com.htc.launcher.permission.UPDATE_SHORTCUT"/>

<!--for sony-->
<uses-permission android:name="com.sonyericsson.home.permission.BROADCAST_BADGE"/>

<!--for apex-->
<uses-permission android:name="com.anddoes.launcher.permission.UPDATE_COUNT"/>

<!--for solid-->
<uses-permission android:name="com.majeur.launcher.permission.UPDATE_BADGE"/>
<!-- End: ShortcutBadger -->
```

Additionally, you will need to add the Kochava broadcast receiver and the
following meta-data tag between the __application tags__, this is needed for the Google Play referral data capture:
```xml
<meta-data
      android:name="com.google.android.gms.version"
      android:value="@integer/google_play_services_version" />
<receiver android:name="com.onesignal.GcmBroadcastReceiver"
          android:permission="com.google.android.c2dm.permission.SEND" >
  <intent-filter>
    <action android:name="com.google.android.c2dm.intent.RECEIVE" />
    <category android:name="__manifestApplicationId__" />
  </intent-filter>
</receiver>
<receiver android:name="com.onesignal.NotificationOpenedReceiver" />
<service android:name="com.onesignal.GcmIntentService" />
<service android:name="com.onesignal.SyncService" android:stopWithTask="false" />
<activity android:name="com.onesignal.PermissionsActivity" android:theme="@android:style/Theme.Translucent.NoTitleBar" />
```
```

### Edit `Android.mk`
Edit `<project_root>/jni/Android.mk` to:

Add additional requirements to __LOCAL_WHOLE_STATIC_LIBRARIES__:
```
LOCAL_WHOLE_STATIC_LIBRARIES += PluginOneSignal
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
$(call import-module, ./pluginonesignal)
```

This means that your ordering should look similar to this:
```
$(call import-add-path,$(LOCAL_PATH))
$(call import-module, ./sdkbox)
$(call import-module, ./pluginonesignal)
```

  __Note:__ It is important to make sure these statements are above the existing `$(call import-module,./prebuilt-mk)` statement, if you are using the pre-built libraries.

### Modify `Application.mk` (Cocos2d-x v3.0 to v3.2 only)
Edit `<project_root>/jni/Application.mk` to make sure __APP_STL__ is defined
correctly. If `Application.mk` contains `APP_STL := c++_static`, it should be
changed to:
```
APP_STL := gnustl_static
```

