### Initialize Share
Modify your Lua code to `init()` the plugin. This can be done anyplace, however it must be done before trying to use the plugin's features.
```lua
sdkbox.PluginShare:init()
```

### share
After initialization you can begin to use the Share functionality:
```lua
local shareInfo = {}
shareInfo.text = '#sdkbox(www.sdkbox.com) - the cure for sdk fatigue - from lua - '
shareInfo.title = "sdkbox";
-- shareInfo.image = "path/to/image";
shareInfo.link = "http://www.sdkbox.com";
info.showDialog = false; -- if you want share with dialog，set the value true

//sdkbox.SocialPlatform.Platform_Select will show platforms list, let user select which platform want to share
//sdkbox.SocialPlatform.Platform_Twitter will share with twitter directly
//sdkbox.SocialPlatform.Platform_Facebook will share with facebook directly
shareInfo.platform = sdkbox.SocialPlatform.Platform_Select;
plugin:share(shareInfo)
```

all value of sdkbox.SocialPlatform

- Platform_Unknow
- Platform_Twitter
- Platform_Facebook
- Platform_SMS
- Platform_Mail
- Platform_Native
- Platform_Select
- Platform_All


all value of sdkbox.SocialShareState

- SocialShareStateNone
- SocialShareStateUnkonw
- SocialShareStateBegin
- SocialShareStateSuccess
- SocialShareStateFail
- SocialShareStateCancelled
- SocialShareStateSelectShow
- SocialShareStateSelected
- SocialShareStateSelectCancelled

### Native Share

you can use ios/andrid system native share:
```lua
local shareInfo = {}
shareInfo.text = "#sdkbox(www.sdkbox.com) - the cure for sdk fatigue ";
shareInfo.title = "sdkbox";
//shareInfo.image = "path/to/image"
shareInfo.link = "http://www.sdkbox.com";

// the follow property will be ignored in nativeShare
//shareInfo.showDialog = false;
//shareInfo.platform = sdkbox.SocialPlatform.Platform_Select;

sdkbox.PluginShare:nativeShare(info);
```

*Note*:

* IOS: when trigger share success event, action name will pass by error in sdkbox::SocialShareResponse
* Android: share success event will trigger, but this is not real share success, just show share panel success, because can't get real share success event on android
* please make sure you have permission `NSPhotoLibraryUsageDescription`, if you want to use `native share` to share image.

### Catch Share events (optional)
This allows you to catch the `Share` events so that you can perform operations based upon responses. A simple example might look like this:
```lua
local plugin = sdkbox.PluginShare
plugin:setListener(function(responsed)
	local event = responsed.event

    dump(responsed, "PluginShare share listener info:")
    if responsed.response.state == sdkbox.SocialShareState.SocialShareStateSuccess then
        print('share success')
    end

end)
plugin:init()
```
