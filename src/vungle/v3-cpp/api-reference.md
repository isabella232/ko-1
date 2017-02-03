## API Reference

### Methods
```cpp
static void init ( ) ;
```
> initialize the plugin instance.

```cpp
static void show ( const std::string & name ) ;
```
> show ad with a provided name.

```cpp
static void setListener ( VungleListener * listener ) ;
```
> set provided listener.

```cpp
static VungleListener * getListener ( ) ;
```
> get provided listener.

```cpp
static void removeListener ( ) ;
```
> remove listeners.

```cpp
static void setDebug ( bool enable ) ;
```
> enable or disable debug mode.

```cpp
static bool isCacheAvailable ( ) ;
```
> is there a cached video available.

```cpp
static void setUserID ( const std::string & userID ) ;
```
> sets the userID for rewarded ads.


### Listeners
```cpp
void onVungleCacheAvailable ( );
```
> ad cache is available.

```cpp
void onVungleStarted ( );
```
> Vungle is running and available.

```cpp
void onVungleFinished ( );
```
> Vungle is not running/has stopped.

```cpp
void onVungleAdViewed ( bool isComplete );
```
> Vungle ad has been viewed.

```cpp
void onVungleAdReward ( const std::string & name );
```
> Vungle reward ad has benviewed


