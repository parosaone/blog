---
layout: post
title: Full Screen React Native Android Build
date: 2020-09-24 15:00:00
lastmod : 2020-10-8 15:00:00 
---

By default, React Native's Android build shows both the [Status bar](https://material.io/design/platform-guidance/android-bars.html#status-bar) and the [Android navigation bar](https://material.io/design/platform-guidance/android-bars.html#android-navigation-bar). There are several ways to hide these elements and make the Android build full-screen.

![React Native's Default Sample App on Android]({{site.url}}/static/2020-9-24-full-screen-react-native-android-build/01.PNG)

Instead of using external package such as [react-native-full-screen](https://www.npmjs.com/package/react-native-full-screen), this guide focuses on changing the `MainActivity.java` file inside the React Native project's `android` folder.

Since it only changes the Android build setting, it does not affect the iOS build.

## Steps

### 1. Open the Following File.

`android\app\src\main\java\com\app\MainActivity.java`

```java
package com.app;

import com.facebook.react.ReactActivity;

public class MainActivity extends ReactActivity {

    /**
    * Returns the name of the main component registered from JavaScript. This is used to schedule
    * rendering of the component.
    */
    @Override
    protected String getMainComponentName() {
        return "app";
    }
}
```

### 2. Add Import

```java
...

import com.facebook.react.ReactActivity;

import android.os.Bundle;
import android.view.View;

...
```

### 3. Add Fullscreen Setting

The Following Java Code is copied from [Android Developers Documentation](https://developer.android.com/training/system-ui/immersive). Check for other modes such as [Lean back](https://developer.android.com/training/system-ui/immersive#leanback) and [Immersive](https://developer.android.com/training/system-ui/immersive#immersive) in the documentation.

```java
...

public class MainActivity extends ReactActivity {
    @Override
    public void onWindowFocusChanged(boolean hasFocus) {
        super.onWindowFocusChanged(hasFocus);
        if (hasFocus) {
            hideSystemUI();
        }
    }

    private void hideSystemUI() {
        // Enables regular immersive mode.
        // For "lean back" mode, remove SYSTEM_UI_FLAG_IMMERSIVE.
        // Or for "sticky immersive," replace it with SYSTEM_UI_FLAG_IMMERSIVE_STICKY
        View decorView = getWindow().getDecorView();
        decorView.setSystemUiVisibility(
                View.SYSTEM_UI_FLAG_IMMERSIVE
                // Set the content to appear under the system bars so that the
                // content doesn't resize when the system bars hide and show.
                | View.SYSTEM_UI_FLAG_LAYOUT_STABLE
                | View.SYSTEM_UI_FLAG_LAYOUT_HIDE_NAVIGATION
                | View.SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN
                // Hide the nav bar and status bar
                | View.SYSTEM_UI_FLAG_HIDE_NAVIGATION
                | View.SYSTEM_UI_FLAG_FULLSCREEN);
    }

    // Shows the system bars by removing all the flags
    // except for the ones that make the content appear under the system bars.
    private void showSystemUI() {
        View decorView = getWindow().getDecorView();
        decorView.setSystemUiVisibility(
                View.SYSTEM_UI_FLAG_LAYOUT_STABLE
                | View.SYSTEM_UI_FLAG_LAYOUT_HIDE_NAVIGATION
                | View.SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN);
    }

    ...

    @Override
    protected String getMainComponentName() {
        return "app";
    }
}
```

### 4. (Re)Run App

    `$ npx react-native run-android` (or `$ npm run android`)

![React Native's Android Build Running in Fullscreen]({{site.url}}/static/2020-9-24-full-screen-react-native-android-build/02.PNG)


## Alternative?

Eventhough the Android status bar is hidden, react-native does not know about it. Therefore, certain elements(e.g. [Modal](https://reactnative.dev/docs/modal#animationtype)'s backdrop) does not extend to the status bar area. Using [`react-native-extra-dimensions-android`](https://github.com/Sunhat/react-native-extra-dimensions-android) is advised by [`react-native-modal`](https://github.com/react-native-community/react-native-modal), but this might not fix the issue.

![React Native's Modal Background Not Extending to Status Bar Area]({{site.url}}/static/2020-9-24-full-screen-react-native-android-build/03.PNG)

In such case, use [`StatusBar`](https://reactnative.dev/docs/statusbar) component, and use `setHidden(true)` method. Note that this hides status bar in iOS as well.

```jsx
import { StatusBar } from 'react-native';
StatusBar.setHidden(true);
```


## Source

* [Hide Android Navigation Bar in React Native](https://stackoverflow.com/questions/36046055/)
* [Enable fullscreen mode (Android Developers Documentation)](https://developer.android.com/training/system-ui/immersive)