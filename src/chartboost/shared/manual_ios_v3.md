## Manual Integration For iOS
Drag and drop the following frameworks from the __plugins/ios__ folder of the `Chartboost` bundle into your Xcode project, check `Copy items if needed` when adding frameworks:

> sdkbox.framework

> PluginChartboost.framework

> Chartboost.framework

The above frameworks depend upon other frameworks. You also need to add the
following system frameworks, if you don't already have them:

> Security.framework

> StoreKit.framework

> Foundation.framework

> CoreGraphics.framework

> UIKit.framework

> AdSupport.framework
