---
layout: post
title: Draggable Components in React Native with Touch Support and Spring Back
date: 2020-09-28 15:00:00 
lastmod : 2020-09-30 15:00:00 
---

Two examples of Draggable Components(Boxes) in React Native. They are from the official website(documentation), and they both support Web as well as iOS and Android.

## Note
If the previews of Expo Snacks are not shown, it is due to width limitation. Please view it in a seperate window by clicking the icon located on the right of the Snack's title.

## Drag and Stay
from [PanResponder](https://reactnative.dev/docs/panresponder) API
<div data-snack-id="vtOFUs!pB" data-snack-platform="web" data-snack-preview="true" data-snack-theme="light" style="overflow:hidden;background:#fafafa;border:1px solid rgba(0,0,0,.08);border-radius:4px;height:505px;width:100%"></div>
<script async src="https://snack.expo.io/embed.js"></script>

### Source Code

```jsx
import React, { useRef } from "react";
import { Animated, View, StyleSheet, PanResponder, Text } from "react-native";

const App = () => {
  const pan = useRef(new Animated.ValueXY()).current;

  const panResponder = useRef(
    PanResponder.create({
      onMoveShouldSetPanResponder: () => true,
      onPanResponderGrant: () => {
        pan.setOffset({
          x: pan.x._value,
          y: pan.y._value
        });
      },
      onPanResponderMove: Animated.event(
        [
          null,
          { dx: pan.x, dy: pan.y }
        ]
      ),
      onPanResponderRelease: () => {
        pan.flattenOffset();
      }
    })
  ).current;

  return (
    <View style={styles.container}>
      <Text style={styles.titleText}>Drag this box!</Text>
      <Animated.View
        style=
        {
          {transform: [{ translateX: pan.x }, { translateY: pan.y }]}
        }
        {...panResponder.panHandlers}
      >
        <View style={styles.box} />
      </Animated.View>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    alignItems: "center",
    justifyContent: "center"
  },
  titleText: {
    fontSize: 14,
    lineHeight: 24,
    fontWeight: "bold"
  },
  box: {
    height: 150,
    width: 150,
    backgroundColor: "blue",
    borderRadius: 5
  }
});

export default App;
```

### Drag with Spring Back
from [Animated.ValueXY](https://reactnative.dev/docs/animatedvaluexy) API
<div data-snack-id="qK!doj8BA" data-snack-platform="web" data-snack-preview="true" data-snack-theme="light" style="overflow:hidden;background:#fafafa;border:1px solid rgba(0,0,0,.08);border-radius:4px;height:505px;width:100%"></div>

#### Using Native Driver

[`useNativeDriver`](https://reactnative.dev/blog/2017/02/14/using-native-driver-for-animated) needs to be explicitly specified.

In the following code, it is newly added in [`Animated.spring`](https://reactnative.dev/docs/animated#spring) and [`Animated.event`](https://reactnative.dev/docs/animated#event). Moreover, [`Animated.event`](https://reactnative.dev/docs/animated#event)'s second argument `config?` is required, and therefore newly added to remove regarding error.

The problem is, `pan.getLayout()` does not support `useNativeDriver: true`. It will cause the below warning. Therefore, `pan.getLayout()` in the original code has been replaced by `pan.getTranslateTransform()`, as guided in this React Native [issue](https://github.com/facebook/react-native/issues/28558).

```
Style property 'left' is not supported by native animated module
```

Similarly, `pan.getLayout()` does not support `useNativeDriver: true`. It will cause the below warning. Therefore, `config`'s `useNativeDriver` is set as `false`, as guided in this React Native [issue](https://github.com/facebook/react-native/issues/13377).

```
config.onPanResponderMove is not a function
```

#### Modified Source Code	

```jsx
import React, { useRef } from "react";
import { Animated, PanResponder, StyleSheet, View } from "react-native";

const DraggableView = () => {
  const pan = useRef(new Animated.ValueXY()).current;

  const panResponder = PanResponder.create({
    onStartShouldSetPanResponder: () => true,
    onPanResponderMove: Animated.event([
      null,
      {
        dx: pan.x, // x,y are Animated.Value
        dy: pan.y,
      }],
      {
        useNativeDriver: false, // Added!
      }
    ),
    onPanResponderRelease: () => {
      Animated.spring(
        pan, // Auto-multiplexed
        { 
          toValue: { x: 0, y: 0 }, // Back to zero
          useNativeDriver: true, // Added!
        } 
      ).start();
    },
  });

  return (
    <View style={styles.container}>
      <Animated.View
        {...panResponder.panHandlers}
        // style={[pan.getLayout(), styles.box]}
        style={[pan.getTranslateTransform(), styles.box]}
      />
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: "center",
    alignItems: "center",
  },
  box: {
    backgroundColor: "#61dafb",
    width: 80,
    height: 80,
    borderRadius: 4,
  },
});

export default DraggableView;
```