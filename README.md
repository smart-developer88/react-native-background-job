# react-native-background-job [![npm version](https://badge.fury.io/js/react-native-background-job.svg)](https://badge.fury.io/js/react-native-background-job)

Schedule background jobs that run your JavaScript when your app is in the background. 



The jobs will run even if the app has been closed and, by default, also persists over restarts.

This library relies on  [`HeadlessJS`](https://facebook.github.io/react-native/docs/headless-js-android.html) which is currently only supported on Android.

## Requirements

-   RN 0.36+
-   Android API 21+

## Supported platforms

-   Android

## Getting started

`$ yarn add react-native-background-job`

or

`$ npm install react-native-background-job --save`

### Mostly automatic installation

`$ react-native link react-native-background-job`

### Manual installation

<!--
#### iOS

1. In XCode, in the project navigator, right click `Libraries` ➜ `Add Files to [your project's name]`
2. Go to `node_modules` ➜ `react-native-background-job` and add `RNBackgroundJob.xcodeproj`
3. In XCode, in the project navigator, select your project. Add `libRNBackgroundJob.a` to your project's `Build Phases` ➜ `Link Binary With Libraries`
4. Run your project (`Cmd+R`)<

-->

#### Android

1.  Open up `android/app/src/main/java/[...]/MainActivity.java`
    -   Add `import com.pilloxa.backgroundjob.BackgroundJobPackage;` to the imports at the top of the file
    -   Add `new BackgroundJobPackage()` to the list returned by the `getPackages()` method
2.  Append the following lines to `android/settings.gradle`:


            include ':react-native-background-job'
            project(':react-native-background-job').projectDir = new File(rootProject.projectDir, 	'../node_modules/react-native-background-job/android')

3.  Insert the following lines inside the dependencies block in `android/app/build.gradle`:


              compile project(':react-native-background-job')

## API

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

### register

Registers jobs and the functions they should run. 

This has to run on each initialization of React Native. Only doing this will not start running the job. It has to be scheduled by `schedule` to start running.

**Parameters**

-   `obj` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** 
    -   `obj.jobKey` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** A unique key for the job
    -   `obj.job` **[function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function)** The JS-function that will be run

**Examples**

```javascript
import BackgroundJob from 'react-native-background-job';

const backgroundJob = {
 jobKey: "myJob",
 job: () => console.log("Running in background")
};

BackgroundJob.register(backgroundJob);
```

### schedule

Schedules a new job. 

This only has to be run once while `register` has to be run on each initialization of React Native.

**Parameters**

-   `obj` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** 
    -   `obj.jobKey` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** A unique key for the job
    -   `obj.timeout` **[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)** How long the JS job may run before being terminated by Android (in ms).
    -   `obj.period` **[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)?** The frequency to run the job with (in ms). This number is not exact, Android may modify it to save batteries. Note: For Android > N, the minimum is 900 0000 (15 min). (optional, default `900000`)
    -   `obj.persist` **[boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?** If the job should persist over a device restart. (optional, default `true`)
    -   `obj.warn` **[boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?** If a warning should be raised if overwriting a job that was already scheduled. (optional, default `true`)

**Examples**

```javascript
import BackgroundJob from 'react-native-background-job';

const backgroundJob = {
 jobKey: "myJob",
 job: () => console.log("Running in background")
};

BackgroundJob.register(backgroundJob);

var backgroundSchedule = {
 jobKey: "myJob",
 timeout: 5000
}

BackgroundJob.schedule(backgroundSchedule);
```

### getAll

Fetches all the currently scheduled jobs

**Parameters**

-   `obj` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** 
    -   `obj.callback` **function ([Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array))** A list of all the scheduled jobs will be passed to the callback

**Examples**

```javascript
import BackgroundJob from 'react-native-background-job';

BackgroundJob.getAll({callback: (jobs) => console.log("Jobs:",jobs)});
```

### cancel

Cancel a specific job

**Parameters**

-   `obj` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** 
    -   `obj.jobKey` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** The unique key for the job
    -   `obj.warn` **[boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?** If one tries to cancel a job that has not been scheduled it will warn (optional, default `true`)

**Examples**

```javascript
import BackgroundJob from 'react-native-background-job';

BackgroundJob.cancel({jobKey: 'myJob'});
```

### cancelAll

Cancels all the scheduled jobs

**Examples**

```javascript
import BackgroundJob from 'react-native-background-job';

BackgroundJob.cancelAll();
```

### setGlobalWarnings

Sets the global warning level

**Parameters**

-   `warn` **[boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** 

**Examples**

```javascript
import BackgroundJob from 'react-native-background-job';

BackgroundJob.setGlobalWarnings(false);
```
