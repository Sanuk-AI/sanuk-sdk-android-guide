# Sanuk SDK Android

SanukSDK is a powerful tool that enables you to integrate robust driving behavior analysis into your mobile app. By leveraging cutting-edge technology, our SDK provides accurate trip tracking, intelligent behavior detection, and personalized insights to enhance user experiences and promote safer driving.

#### Key Capabilities

* **Recording** – SDK automatically captures every trip taken by the user.
* **Scoring** – Analyzes driving behaviors and assigns a score based on various metrics.
* **Driving Events** – Provide various driving events after trip processing.
* **Collision Detection** - Notify to user and admin when collision is detected.


## Requirements

SanukSDK is designed to work with Android devices running Android API level 24 (Android 7.0 Nougat) or higher.


## Installation

Add this to your root `build.gradle` at the end of repositories:

```
allprojects {
   repositories {
       ...
       maven { url 'https://jitpack.io' }
   }
}
```

And then add the dependency in `app/build.gradle` file:

```
dependencies {
    implementation 'co.drvr.sdk:autosdk:1.0.1'
}
```

## Setup

To configure the SDK, you need to

#### Request Permissions

The SDK uses `Activity Recognition` data and `Location` data to record the trip. Before integrating SanukSDK into your Android project, you'll need to ensure the required permissions are granted.

#### Implement Delegate Method

Implement delegate methods of `SanukSDKDelegate` from any `Activity` or `Fragment` to receive the information from SDK. 

```kotlin
class MainActivity : AppCompatActivity(), SanukSDKDelegate {

    override fun onStateChanged(state: SanukSDKState) {
        TODO("Not yet implemented")
    }

    override fun onTripStarted() {
        TODO("Not yet implemented")
    }

    //implement all other delegate methods

}
```

## Usage

This section of the readme will guide you through using the SDK into your project.

#### Starting the SDK

The `SanukSDKClient` class provides methods to interact with the SDK. To start the SDK, you will need to have a username and password or a SDK key. Here's how to start the SDK:

```kotlin
class MainActivity : AppCompatActivity(), SanukSDKDelegate {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        SanukSDKClient.start(
            login = "login@example.com",
            password = "password",
            application = this.application,
            currentActivity = this,
            delegate = this
        )
    }
}
```

Once the SDK finished starting, it would call the `onStateChanged` function from the `SanukSDKDelegate`.

```kotlin
override fun onStateChanged(state: SanukSDKState) {
    this.updateSDKState(state)
}
```

You can also call `SanukSDKClient.getSDKState()` to get the SDK state.

```kotlin
when (SanukSDKClient.getSDKState()) {
    SanukSDKState.STOPPED -> {...}
    SanukSDKState.STARTING -> {...}
    SanukSDKState.STARTED -> {...}
}
```

To stop the SDK, just simply call the `stop` function.

```kotlin
SanukSDKClient.stop()
```

#### Retrieving the Trips

Once a trip is completed, you can get the trip data by calling the `getTrips` function. The `KamoTrip` model contains various trip-related metrics such as distance traveled, duration, and a calculated trip score.

```kotlin
SanukSDKClient.getTrips { trips ->
    this.displayTrip(trips)
}
```

#### Get Driver's Score

Use the `getDriverScore` function to fetch driver score data. The `KamoScore` model contains driver's overall driving score and event scores such as speeding, phoneUse, breaking, acceleration and turning. 

```kotlin
SanukSDKClient.getDriverScore { score ->
    val overall = score?.totalAverage?.overall
    val speeding = score?.totalAverage?.speeding
    val phoneUse = score?.totalAverage?.phoneUse
    val breaking = score?.totalAverage?.hardBreaking
    val turning = score?.totalAverage?.turning
    val acceleration = score?.totalAverage?.rapidAcceleration
}
```
