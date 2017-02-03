### Initialize Vungle
* modify your Lua code to `init()` the plugin. This can be done anyplace, however it must be done before trying to use the plugin's features.
```lua
sdkbox.PluginVungle:init()
```

### Showing Ads
Display an ad where ever you want from your code, either __video__ or __reward__:
```lua
sdkbox.PluginVungle:show("video")
sdkbox.PluginVungle:show("reward")
```

### Catch Vungle events (optional)
This allows you to catch the `Vungle` events so that you can perform operations such as providing player rewards for watching the video.

* Create a listener (demonstrated by logging events):
```lua
sdkbox.PluginVungle:setListener(function(name, args)
    if "onVungleCacheAvailable" == name then
        print("onVungleCacheAvailable")
    elseif "onVungleStarted" ==  name then
        print("onVungleStarted")
    elseif "onVungleFinished" ==  name then
        print("onVungleFinished")
    elseif "onVungleAdViewed" ==  name then
        print("onVungleAdViewed:", args)
    elseif "onVungleAdReward" ==  name then
        print("onVungleAdReward:", args)
    end
end)
```

`Note:` DONOT change your game uiNode in the `onVungleAdViewed` or `onVungleAdReward` immediately, becase the cocos opengl is disable when `Vungle` send `onVungleAdViewed` or `onVungleAdReward`. use `schedule` delay change ui.
