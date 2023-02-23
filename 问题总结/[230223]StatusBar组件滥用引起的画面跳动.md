# React Native StatusBar组件滥用引起的画面跳动
StatusBar组件是对状态栏进行设置的组件，设置自定义状态栏，可以提高应用美观，让用户在沉浸式的应用中体验。
主要属性有：

- `backgroundColor` 状态栏颜色，仅android
- `barStyle`状态栏文本颜色，Android仅支持6.0及以上版本，`enum('default', 'light-content', 'dark-content')`
- `translucent`指定状态栏是否透明。设置为 true 时，应用会延伸到状态栏之下绘制（即所谓“沉浸式”——被状态栏遮住一部分）。常和带有半透明背景色的状态栏搭配使用，仅android
- `animated`指定状态栏的变化是否应以动画形式呈现。目前支持这几种样式：backgroundColor, barStyle 和 hidden
- `hidden`是否隐藏状态栏
- `networkActivityIndicatorVisible`指定是否显示网络活动提示符,仅ios
- `showHideTransition`通过`hidden`属性来显示或隐藏状态栏时所使用的动画效果。默认值为'fade'，还有'slide'值，仅ios

本文重点描述`translucent`产生的问题进行概括。

由于设置`translucent=true`时，应用页面会延伸到状态栏之下绘制，也就是说，如果是一张图片，那么在状态栏之下显示图片，而默认的都是状态栏下进行绘制.

在应用中发生了画面跳动的现象，如下视频：
[发生问题的视频演示](resources/230223/problem.mp4)