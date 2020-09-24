---
layout: post
title: Exporting Image Asset for Different Screen Resolutions (@2x, @3x)
date: 2020-09-24 18:00:00 
---

Raster images (= non-vector images) displayed on screen are usually resized. They are not shown at their native resolution. This is why image viewers have the option to `Fit` or `Stretch` the image to fit the window or screen (on mobile devices).

## Application Assets

The problem are images assets inside applications. Especially on Android, where there are multiple DPI(Dots per inch) and screen resolutions, images are almost never shown at their native resolution. Yet, the images have to look crisp and clear.

An easy solution might be to add extremely high-density raster images and downsize them on every render according to the device's physical screen resolution. However, such frequent resizings of images are expensive in terms of computing power.

## Pre-Scaled Images

This is why developers sacrifice application size (= increase size) and bundle variants of the same image asset. These variants are distinguished by the suffixes in the file names. (e.g. `check.png` / `check@2x.png` / `check@3x.png`)

Android and iOS loads images according to the device's screen resolution. For example on Android, `720 x 1280` device loads `@2x` asset, and `1080 x 1920` device loads `@3x`. `1080 x 2160` device will probably load `@3x` asset as well.

## So What is `@1x` ?

There are several guides that state that `@2x` images are double in both width and height of the original image. For example, if the image created is `100 x 100`, `@2x` would be `200 x 200`, `@3x` would be `300 x 300` and so on.

However, such definition is not sufficient when designing a `Button` image for example. If a button has to fill 1/3 of the width of a device, what should be the width(px) of a `@1x` button? Since FHD devices are the norm, should it be `360`px wide?

## Quick History

There was a time when iPhone's resolution was `320 x 480` and Android smartphone's resolution was `360 x 640`. Back then, most devices had similar screen resolution. Therefore single scale image assets were bundled. This is `@1x`.

The problem emerged with Apple's 'retina-display'. iPhone 4/4S had a screen resolution of `640 x 960`, double the width and height of its predecessors. To show the images crisp and clear, applications needed higher resolution, `@2x` assets.

Therefore, if one is designing applications at `1080 x 1920` for Android, the raster images created there are basically `@3x` assets. To support `2160 x 3840`, or 2K Android devices, one **might** have to include `@6x` assets for maximum clarity.

## Source

* [Screen resolutions cheatsheet](https://devhints.io/resolutions)