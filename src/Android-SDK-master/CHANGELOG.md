Changelog
=====================

** 0.8.7 (August 25, 2015)
 - Finally Estimote SDK is available on Maven Central (`com.estimote:sdk:0.8.7@aar`).

## 0.8.6 (August 21, 2015)
 - Authentication to beacon is more robust than ever (requires updating beacon to firmware 3.2.0).

## 0.8.5 (August 20, 2015)
 - Security improvements in the beacon authorization mechanism.
 - Beacon update is more stable.

## 0.8.2 (July 24, 2015)
 - This is mainly a bugfix release.
 - Fixed (https://github.com/Estimote/Android-SDK/issues/109): Documentation Bug.
 - Fixed (https://github.com/Estimote/Android-SDK/issues/117): Crash after update to 0.8 version.
 - SDK demos are now using Material Design (h/t @RingoMckraken).

## 0.8.1 (July 15, 2015)
 - Small fixes for Eddystone protocol.

## 0.8 (July 14, 2015)
 - Say hello to [Eddystone](https://developers.google.com/beacons) - an open protocol BLE protocol from Google.
   - Estimote Beacons can broadcast Eddystone protocol.
 - In order to start playing with Eddystone you need to update firmware of your existing Estimote beacons to `3.1.1`. Easiest way is through [Estimote app on Google Play](https://play.google.com/store/apps/details?id=com.estimote.apps.main). Than you can change broadcasting scheme on your beacon to Eddystone-URL or Eddystone-UID.
 - *New in SDK*:
   - find nearby Eddystone beacons (`BeaconManager#startEddystoneScanning`)
   - configure Eddystone related properties:
     - URL property of `Eddystone-URL` (see `BeaconConnection#eddystoneUrl`)
     - namespace & instance properties of `Eddystone-UID` (see `BeaconConnection#eddystoneNamepsace`, `BeaconConnection#eddystoneInstance`)
   - configure broadcasting scheme of beacon to `Estimote Default`, `Eddystone-UID` or `Eddystone-URL` (see `BeaconConnection#broadcastingScheme`)
 - [SDK Examples](https://github.com/Estimote/Android-SDK/tree/master/Demos) have been updated to showcase Eddystone support.

## 0.7 (June 18, 2015)
 - Initial support for nearables. You can discover nearby nearables via `BeaconManager.startNearableDiscovery()`. With nearbles you can read temperature, motion, orientation without need to connect to it. Directly from discovered `Nearable` class.
 - You can change basic & smart power mode in your beacon via `BeaconConnection`. [Read more about power modes.](https://community.estimote.com/hc/en-us/articles/202552866-How-to-optimize-battery-performance-of-Estimote-Beacons-)
 - `android.hardware.bluetooth_le` feature is no longer required
 - You can also change conditional broadcating in beacon (Flip To Sleep). It is great for development. [Read more about Flip To Sleep.](https://community.estimote.com/hc/en-us/articles/205413787-How-to-enable-conditional-broadcasting-and-Flip-to-sleep-mode-)
 - **Breaking changes** (1.0 is approaching, bear with us):
   - most of `BeaconConnection`s write* methods are gone, they are replaced with more appropriate `Property` class

```java
// Before
connection.writeMajor(newMajor, callback);
connection.writeMinor(newMinor, callback);

// After: reading
connection.minor().get()
connection.major().get()

// After: writing in batch
connection.edit()
  .set(connection.proximityUuid(), newUuid)
  .set(connection.major(), newMajor)
  .set(connection.minor(), newMinor)
  .commit(callback);
```

## 0.6.1 (June 2, 2015)
 - Fixed authentication issues (#111).

## 0.6 (May 4, 2015)
 - You can update update firmware in Estimote beacons from the SDK. There are several ways to do it:
    - Use `BeaconOta` class to perform firmware update on selected beacon.
    - Use `BeaconConnection#updateBeacon` which triggers update on the beacon. See updated demos to see how it works.
    - You can also use Estimote app from Play Store to do that.
 - Estimote SDK now includes also `android.permission.ACCESS_NETWORK_STATE` permission to determine internet connectivity.
 - **Breaking changes** (please bear with us, we are approaching stable 1.0 release):
    - `BeaconConnection`'s `ConnectionCallback#onAuthenticated` method does not return `BeaconCharacteristics` object any more. You can read them directly on `BeaconConnection` object.
    - For example read reading broadcasting power is just `connection.getBroadcastingPower()`.

## 0.5 (April 17, 2015)
 - From now Estimote SDK for Android is distributed as [AAR archive](http://tools.android.com/tech-docs/new-build-system/aar-format) rather than jar. That means that you do not need to change your `AndroidManifest.xml`. SDK's `AndroidManifest.xml` will be merged with your application's `AndroidManifest.xml`. See [installation guide](https://github.com/Estimote/Android-SDK#installation) how to add library to your project.
 - Welcome back! We have added support for [Estimote Cloud](http://cloud.estimote.com). You can access it via `EstimoteCloud` class. Remember first to provide your App ID & App Token from App section of [Estimote Cloud](http://cloud.estimote.com) via `EstimoteSDK#initialize` method.
 - From now all connections to beacons needs to be authorized. If a beacon is not registered to your account, you will not be able to connect to it.
 - Estimote SDK's `AndroidManifest.xml` uses `BLUETOOTH`, `BLUETOOTH_ADMIN` and `INTERNET` permissions.
 - Yes, there is single point of initialisation of the SDK.

 ```java
 //  App ID & App Token can be taken from App section of Estimote Cloud.
 EstimoteSDK.initialize(applicationContext, appId, appToken);
 // Optional, debug logging.
 EstimoteSDK.enableDebugLogging(true);
 ```
 - All exceptions within the SDK has been unified and exposed in `com.estimote.sdk.exception` package.
 
 - That means some **breaking changes**:
	 - `L` class is no longer available, in order to turn on debug logging use `EstimoteSDK` class.
	 - `BeaconConnection.ConnectionCallback` & `BeaconConnection.WriteCallback` methods have been changed to contain apropriate exception when happens.

## 0.4.3 (November 12, 2014)
 - Fixes https://github.com/Estimote/Android-SDK/issues/59: compatibilty with Android L

## 0.4.2 (June 24, 2014)
 - Fixes https://github.com/Estimote/Android-SDK/issues/55: it is safe to use library from remote process

## 0.4.1 (March 18, 2014)
 - *CAN BREAK BUILD*: MonitoringListener returns list of beacons the triggered enter region event (https://github.com/Estimote/Android-SDK/issues/18)
 - Better messaging when BeaconManager cannot start service to scan beacons (https://github.com/Estimote/Android-SDK/issues/25)
 - Fixed bug in SDK when other beacons are around (https://github.com/Estimote/Android-SDK/issues/27)

## 0.4 (February 17, 2014)
 - Introducing ability to change beacon's UUID, major, minor, broadcasting power, advertising interval (see BeaconConnection class).
 - Dropping Guava dependency.

## 0.3.1 (February 11, 2014)
 - Fixes bug when simulated beacons were not seen even when using Estimote's proximity UUID.

## 0.3 (February 11, 2014)
 - Background monitoring is more robust and using AlarmService to invoke scanning.
 - Default values for background monitoring were changed. Scanning is performed for 5 seconds and then service sleeps for 25 seconds. Those values can be changed with BeaconManager#setBackgroundScanPeriod.
 - Beacons reported in RangingListener#onBeaconsDiscovered are sorted by accuracy (estimated distance between device and beacon).
 - Bug fixes.

## 0.2 (January 7, 2014)
 - *IMPORTANT*: package changes BeaconService is now in `com.estimote.sdk.service service`. You need to update your `AndroidManifest.xml` service definition to `com.estimote.sdk.service.BeaconService`.
 - Support for monitoring regions in BeaconManager.
 - Region class: it is mandatory to provide region id in its constructor. This matches CLRegion/ESTBeaconRegion from iOS.
 - Beacon, Region classes now follow Java bean conventions (that is getXXX for accessing properties).
 - Debug logging is disabled by default. You can enable it via `com.estimote.sdk.utils.L#enableDebugLogging(boolean)`.

## 0.1 (December 9, 2013)
 - Initial version.
