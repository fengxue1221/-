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
[发生画面跳动的视频](https://github.com/fengxue1221/docs/blob/main/%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93/resources/230223/problem.mp4)

可以看出，每次从`Discover`页面跳转到`Profile`，或从`Profile`页面跳转到`Discover`页面时，总会出现页面向下或向上绘制。

经过测试，发现`Discover`页面中`StartBar`组件的没有设置`traslucent`属性，而`Profile`页面则设置了`translucent={true}`和`backgroundColor='transparent'`两个属性，所以导致了每次切换都要重新绘制状态栏的高度，导致了问题。

此问题仅仅出现在Android设备端，所以最终采取了，在`Profile`页面设置：
```jsx
<StatusBar
  barStyle="light-content"
  backgroundColor="#000"
/>
```
去掉了`translucent`属性。

因为`Profile`页面的画顶部正好趋向黑色，所以在此直接设置了黑色，最终的结果是好的。

如下视频：[修改之后的视频](https://github.com/fengxue1221/docs/blob/main/%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93/resources/230223/fixed.mp4)