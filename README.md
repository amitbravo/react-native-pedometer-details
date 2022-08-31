# react-native-pedometer-details

react native pedometer, The extension is implemented for Android. I was going to do the same thing on IOS. Fortunately, `react-native-health` realized my idea very well. `react-native-pedometer-details` performs well on Android. If you have problems, you can find me on GitHub. 

## Installation

```sh

npm install react-native-pedometer-details 
# or
yarn add react-native-pedometer-details 
```

## AndroidManifest.xml

```xml
<uses-permission android:name = "android.permission.RECEIVE_BOOT_COMPLETED" />
<uses-permission android:name = "android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name = "android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name = "android.permission.FOREGROUND_SERVICE"/>
<uses-permission android:name = "android.permission.ACTIVITY_RECOGNITION"/>

<application>
.... 
// put below code under application
<receiver android:name = "com.reactnativepedometerdetails.step.background.RebootActionReceiver"
    android:exported = "false">
    <intent-filter >
        <action android:name = "android.intent.action.BOOT_COMPLETED"/>
    </intent-filter>
</receiver>
<receiver
    android:name = "com.reactnativepedometerdetails.step.background.Restarter"
    android:enabled = "true"
    android:exported = "true"
    android:permission = "false">
    <intent-filter>
        <action android:name = "restartservice" />
    </intent-filter>
</receiver>
<service
    android:name="com.reactnativepedometerdetails.step.background.StepCounterService"
    android:enabled = "true"
    android:exported = "false" />
</application>
```

```
if you face a kotlin version issue then define kotlin version under app/build.gralde such as:
buildscript {
     ...   
    ext {
        ....
        kotlin_version = '1.6.10'
```        
        


## Usage

```js
import PedometerDetails from 'react-native-pedometer-details';

PedometerDetails.requestPermission()
.then((permissionsStatuses) => {
    if (typeof stateText != 'string' || stateText != 'granted') {
        return;
    }
    PedometerDetails.getDaysSteps(20211211).then(res => {
        // res.day ==> 20211211 (YYYYMMDD format)
        // res.stepCount ==> 100
    });
})
.catch(error => {
    console.log('error to get permission',error)
});

// For more usage, please see
// react-native-pedometer-details/src/API.js
```
### Permissions statuses

Permission checks and requests resolve into one of these statuses:

| Return value          | Notes                                                             |
| --------------------- | ----------------------------------------------------------------- |
| `RESULTS.UNAVAILABLE` | This feature is not available (on this device / in this context)  |
| `RESULTS.DENIED`      | The permission has not been requested / is denied but requestable |
| `RESULTS.GRANTED`     | The permission is granted                                         |
| `RESULTS.LIMITED`     | The permission is granted but with limitations                    |
| `RESULTS.BLOCKED`     | The permission is denied and not requestable anymore              |

## License

MIT
