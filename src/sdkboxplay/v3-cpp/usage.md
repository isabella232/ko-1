### Initialize SdkboxPlay
Initialize the plugin where appropriate in your code. We recommend to do this in the `AppDelegate::applicationDidFinishLaunching()` or `AppController:didFinishLaunchingWithOptions()`. Make sure to include the appropriate headers:

```cpp
#include "PluginSdkboxPlay/PluginSdkboxPlay.h"
AppDelegate::applicationDidFinishLaunching()
{
     sdkbox::PluginSdkboxPlay::init();
}
```

### Using SdkboxPlay


#### Intro
SdkboxPlay is an abstraction for Google Play and Game Center’s social services. Under a common API exposes access to Leaderboards and Achievements for each platform.
In order to keep the API fit to the two models, some tradeoffs have been made, which will be detailed in each section

##### Logged in user info

Calling the method `sdkbox::SdkboxPlay::getPlayerId()` to get an id per platform that uniquely identifies the logged-in user.
Additionally, you can query more information about the user. 

######iOS/Android fields

These fields are common to ios and android:
* player_id
* name
* display_name

making a call to `sdkbox::SdkboxPlay::getPlayerAccountField( const std::string& field )` will return a string with the field
contents.
If the requested field does not exist, empty string will returned in exchange.
`player_id` will be returned by calling `sdkbox::SdkboxPlay::getPlayerId()` too.

######Android only fields

For Android platform, there are some other available fields:

* title
* icon_image_uri
* hires_image_uri
* last_play_timestamp
* retrieved_timestamp
* server_auth_code

use the same `getPlayerAccountField` to get these values as strings.

#####Achievements

Achievements are defined on the respective platform’s developer console.
There are differences in concept between GooglePlay and GameCenter’s achievements:
+ Google Play differentiates between achievements, and incremental achievements. Google keeps track of incremental achievements progress. Achievements are achieved only once.
+ For Game Center, all achievements are incremental, but Game center does not keep track of its progress. Achievements are expected to be achieved during a game session. Achievements can be set to be unlocked several times.
+ Google Play has the notion of newly unlocked achievement (first time unlocked), and Game Center has the notion of recurrently unlockable achievement. Both concepts are complementary.

To keep things consisten, SdkboxPlay API:

+ Allows you to define non-incremental achievements. For ios, are submitted with an incremental value of 100, which means it will be unlocked.
+ Allows you to define Incremental achievements. In Google play, incremental achievements have defined their unlocking value on the application console. 
+ For consistency, it is recommended to define Google Play’s achievements with a count of 100. This is the value Game Center expects to be reached to unlock an achievement.

##### Leaderboards

Leaderboards are defined on the respective platform’s developer console.
To keep things simple, the current SdkboxPlay implementation does not allow to define group leaderboards from iOS. For both platforms, an arbitrary number of leaderboards can be defined.
Though both, GooglePlay and GameCenter define leaderboards in the same way, in the runtime there are some differences:

+ Google Play creates automatically 3 time frames for each leaderboard: daily, weekly and all time best scores.
+ Game Center creates just one timeframe.

This will be resembled on the observer methods for leaderboard operations as described below.

#### Usage

A call to `sdkbox::SdkboxPlay::init()` will configure the plugin with the selected leaderboards and achievements present in the sdkbox_config.json file.

First, a connection to the games services must be done by calling:

```cpp
sdkbox::PluginSdkboxPlay::signin();
```

If connection is successful, you'll be able to use the SdkboxPlay services with the following API:

##### Leaderboards

```cpp
void submitScore(   const std::string& leaderboard_name, int score )
```

This method submits a update request to the given leaderboard. The leaderboard name must match any of the leaderboard names defined in the configuration block.
If a request is sent to a non existent leaderboard, nothing will happen.
Whether to store the new score or not, can be defined in the developer’s console (store always latest score, only maximum, etc.)
This method will invoke plugin’s observer method: 

```cpp
void onScoreSubmitted(
        const std::string& leaderboard_name, 
        int score, 
        bool maxScoreAllTime, 
        bool maxScoreWeek, 
        bool maxScoreToday )
```

For iOS, this method will have the three boolean flags as false.

```cpp
void showLeaderboard( const std::string& leaderboard_name );
```

Request to show the leaderboard information. This will invoke a platform specific UI.
For iOS, there’s no different UI for requesting leaderboards and achievements, so this method will invoke the UI with the leaderboards view enabled.

##### Achievements

```cpp
void unlockAchievement( const std::string& achievement_name );
```

Unlock a non incremental achievement. In the case of iOS, it will send a request to Game Center of unlock with 100 progress points.
If the achievement type is incorrectly defined in the configuration file (wrong id), or the play services determines it is of the wrong type (Google play) the method will fail silently.
Upon successful call, this method will invoke the listener’s method: onAchievementUnlocked( const std::string& achievement_name, bool newlyUnlocked ).

```cpp
void incrementAchievement( 
    const std::string& achievement_name, 
    int increment );
```

Increment an incremental achievement.
The method will silently fail if the achievement type is incorrectly defined in the configuration file (wrong or non existent id), or the play services determines it is of the wrong type (Google Play).
If the call is successful, this method may invoke two different methods:
+ `onIncrementalAchievementStep( const std::string& achievement_name, int step )` if the achievement is not unlocked.
+ `onIncrementalAchievementUnlocked( const std::string& achievement_name, bool newlyUnlocked )` the first time it's been unlocked.

```cpp
void showAchievements( );
```

Request to show the default Achievements view. This view only shows public achievements.
t will show specific per platform information, like whether it’s been unlocked, remaining unlocking steps (Google Play only), total experience count, etc.


### SdkboxPlay events
This allows you to catch `SdkboxPlay`' events.

* Allow your class to extend `sdkbox::SdkboxPlayListener` and override the functions listed:
```cpp
#include "PluginSdkboxPlay/PluginSdkboxPlay.h"
class MyClass : public sdkbox::SdkboxPlayListener
{
protected:
    /**
     * Call method invoked when the Plugin connection changes its status.
     * Values are as follows:
     *   + GPS_CONNECTED:       successfully connected.
     *   + GPS_DISCONNECTED:    successfully disconnected.
     *   + GPS_CONNECTION_ERROR:error with google play services connection.
     */
    void onConnectionStatusChanged( int status );
    
    /**
     * Callback method invoked when an score has been successfully submitted to a leaderboard.
     * It notifies back with the leaderboard_name (not id, see the sdkbox_config.json file) and the
     * subbmited score, as well as whether the score is the daily, weekly, or all time best score.
     * Since Game center can't determine if submitted score is maximum, it will send the max score flags as false.
     */
    void onScoreSubmitted( const std::string& leaderboard_name, int score, bool maxScoreAllTime, bool maxScoreWeek, bool maxScoreToday );
    
    /**
     * Callback method invoked when the request call to increment an achievement is succeessful and
     * that achievement gets unlocked. This happens when the incremental step count reaches its maximum value. 
     * Maximum step count for an incremental achievement is defined in the google play developer console.
     */
    void onIncrementalAchievementUnlocked( const std::string& achievement_name );
    
    /**
     * Callback method invoked when the request call to increment an achievement is successful.
     * If possible (Google play only) it notifies back with the current achievement step count.
     */
    void onIncrementalAchievementStep( const std::string& achievement_name, int step );
    
    /**
     * Call method invoked when the request call to unlock a non-incremental achievement is successful.
     * If this is the first time the achievement is unlocked, newUnlocked will be true.
     */
    void onAchievementUnlocked( const std::string& achievement_name, bool newlyUnlocked );
};
```

* Create a __listener__ that handles callbacks:
```cpp
sdkbox::PluginSdkboxPlay::setListener( new MyClass() );
```
