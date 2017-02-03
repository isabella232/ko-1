[&#8249; SOOMLA Doc Home](./)

<h1>SOOMLA Grow Integration Guide</h1>
<<[../../shared/-VERSION-/version.md]


##Prerequisites
* Certain SDKBOX plugins do not work together. If you use __GROW__, then you cannot also use the __AdColony__ and __Fyber__ services, in the same project.

##Integration
1. If you still didn't sign up on the GROW Dashboard, go ahead and do it [here](https://soom.la/signup/sdkbox).

2. Open a terminal and use the following command to install GROW's SDKBOX plugin. Make sure you setup the SDKBOX installer correctly.

  ```bash
  $ sdkbox import soomlagrow
  ```

<<[../../shared/notice.md]

<!--## Configuration
<<[../../shared/sdkbox_cloud.md]
<<[../../shared/remote_application_config.md]-->

### JSON Configuration
SDKBOX Installer will automatically inject a sample configuration to your `res/sdkbox_config.json`, that you will have to modify before you use in your own app.

Here is an example of the GROW configuration, you need to replace `<gameKey>` and `<envkey>` items with the ones you were given by the [GROW Dashboard](http://dashboard.soom.la). You will probably use the same `<gameKey>` and `<envKey>` for Android and iOS but you will still need to specify it twice, once for each platform. Example:

```json
"ios" :
{
  "soomlaGrow":{
              "gameKey":"0cbc07e3-0f0c-4b68-bb0c-061c1b5fb553",
              "envKey":"8b865add-4541-4db1-be18-f6c7e5e00564"
          }
}
"android" :
{
  "soomlaGrow":{
              "gameKey":"0cbc07e3-0f0c-4b68-bb0c-061c1b5fb553",
              "envKey":"8b865add-4541-4db1-be18-f6c7e5e00564"
          }
}
```

<!--<<[sdkbox-config-encrypt.md]-->

##Usage
<<[usage.md]

<<[api-reference.md]

<<[manual_integration.md]

<<[manual_ios.md]

<<[../../shared/manual_integration_android_and_android_studio.md]

<<[manual_android.md]

<<[extra-step.md]
<<[proguard.md]
