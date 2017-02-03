[&#8249; YouTube Doc Home](./)

<h1>YouTube Integration Guide</h1>
<<[../../shared/-VERSION-/version.md]

##Integration
Open a terminal and use the following command to install the SDKBOX Youtube plugin.
```bash
$ sdkbox import youtube
```

<<[../../shared/notice.md]

<!--## Configuration
<<[../../shared/sdkbox_cloud.md]
<<[../../shared/remote_application_config.md]-->

### JSON Configuration
SDKBOX Installer will automatically inject a sample configuration to your `sdkbox_config.json`, that you have to modify it before you can use it for your own app

If you want to display youtube vidoe in your app, you have to register a new youtube API key [here](https://developers.google.com/youtube/android/player/register#Create_API_Keys) and put in `developer_key` section of the `sdkbox_config.json`

#### Option for iOS

- "show_close_button": [bool] display the close button or not

- "close_button_text": [string] set close button text, conflicts with "close_button_image"

- "close_button_image": [string] set button image, conflicts with "close_button_text"

```json
{
    "ios" :
    {
        "Youtube":
        {
            "developer_key":"AIzaSyDMuDjrVSL3uj_QvlI3bbjKn5I4nNB1XZk"
        }
    },
    "android" :
    {
        "Youtube":
        {
            "developer_key":"AIzaSyDMuDjrVSL3uj_QvlI3bbjKn5I4nNB1XZk"
        }
    }
}
```

##Usage
<<[usage.md]

<<[api-reference.md]

<<[manual_integration.md]

<<[manual_ios.md]

<<[../../shared/manual_integration_android_and_android_studio.md]

<<[manual_android.md]

<<[extra-step.md]

<<[../../shared/manual_integration_google_play_step.md]

<<[proguard.md]
