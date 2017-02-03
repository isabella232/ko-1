### Initialize Apteligent
Initialize the plugin where appropriate in your code. We recommend to do this in the `AppDelegate::applicationDidFinishLaunching()` or `AppController:didFinishLaunchingWithOptions()`. Make sure to include the appropriate headers. Example:
```cpp
#include "PluginApteligent/PluginApteligent.h"
AppDelegate::applicationDidFinishLaunching()
{
     sdkbox::PluginApteligent::init();
}
```

### Logging Breadcrumbs

```cpp
// leave a breadcrumb
sdkbox::PluginApteligent::leaveBreadcrumb("User tapped start button");

// perform all breadcrumb writes on a background thread
sdkbox::PluginApteligent::setAsyncBreadcrumbMode(true);
```


### Logging User Metadata

```
// Adding a Username
sdkbox::PluginApteligent::setUsername("MrsCritter");

// Adding Arbitrary User Metadata
sdkbox::PluginApteligent::setValueforKey("5", "Game Level");
```


### Logging Userflows
```cpp
// Beginning a Userflow
sdkbox::PluginApteligent::beginUserflow("login");

// Ending a Userflow
sdkbox::PluginApteligent::endUserflow("login");

// Failing a Userflow
sdkbox::PluginApteligent::failUserflow("login");

// Cancelling a Userflow
sdkbox::PluginApteligent::cancelUserflow("login");
```

### Modifying the Value of a Userflow
```cpp
int valueInCents = sdkbox::PluginApteligent::valueForUserflow("my_userflow");
valueInCents += 5;
sdkbox::PluginApteligent::setValueforUserflow(valueInCents, "my_userflow");

```

### Logging Network Requests
```cpp
sdkbox::PluginApteligent::logNetworkRequest("GET", "http://www.abc123def456.com", 2.0, 1000, 100, 200);
```


### Filtering Captured Data
```cpp
sdkbox::PluginApteligent::addFilter("www.gmail.com");
```

### Configuring Location
```cpp
// Beijing, China
sdkbox::PluginApteligent::updateLocation(39.9042, 116.4074);
```

### Other Tasks
```cpp
// Allowing Users to Opt Out of Apteligent
sdkbox::PluginApteligent::setOptOutStatus(true);

// Changing the verbosity of Apteligent’s Logs
// sdkbox::CRLoggingLevelSilent  : Turns off all Apteligent log messages
// sdkbox::CRLoggingLevelError   : Only print errors. An error is an unexpected event that will result not capturing important data
// sdkbox::CRLoggingLevelWarning : (Default) Only print warnings. Currently warning messages are printed when calling Apteligent methods before initializing Apteligent.
// sdkbox::CRLoggingLevelInfo    : The most verbose level of logging
sdkbox::PluginApteligent::setLoggingLevel(sdkbox::CRLoggingLevelInfo);

// Send App Load Data
// You must set `"delay_sending_appload":true` in `sdkbox_config.json` first
sdkbox::PluginApteligent::sendAppLoadData();
```

### Implement ApteligentListener
* You can implement ApteligentListener if you want to receive callbacks like video finish playing.
```cpp
#include "PluginApteligent/PluginApteligent.h"
class MyClass : public sdkbox::ApteligentListener
{
private:
    void onCrashOnLastLoad() {} // not support on android
}
```
