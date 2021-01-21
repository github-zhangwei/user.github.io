### fastClick

##### fastClick 简介：
```text
        fastClick是一款解决移动端300ms点击延迟而诞生的插件。
        在移动端，如果对页面没有做任何处理，点击一个元素，触发的事件流程可简单的理解为： touch -> 经过300ms延迟 -> click；

```

##### fastClick 原理：
```text
    1、监听 touchend 事件，在 touchend 时调用 e.preventDafault() 禁用300ms后触发默认的 click 事件。
    2、通过 document.createElement 手动创建一个鼠标事件对象。
    3、通过 eventTarget.dispatchEvent 将 click 事件手动派发到当前目标 DOM 元素上。

```

##### fastClick 问题：
```text
    1、移动端点击 input 标签不灵敏，需要点击多次才有反应，
    2、调起手机原生软键盘卡、慢。
    3、点击穿透。
    4、点击错位（点击A，触发B）。

```

##### 点击300ms 由来：
```text
    在以前（十几年前），当时的网页基本上都是为PC设备所设计，没有什么移动端适配的概念，导致字体看起来非常小，阅读困难。
    为了处理这种情况，工程师们想出了，双击缩放（double tap to zoom）。通过双击，在放大比例和原生比例之间进行切换。

    如果判断用户是点击还是双击呢？苹果的逻辑如下：
        在用户点击完此处后，如果300ms内没有此处进行第二次点击，就默认为 点击（click）事件。
        

```

##### 如何解决：
```html
    <meta name="viewport" content="width=device-width, initial-scale=1">

    增加 user-scalable=no， 明确告诉浏览器页面不需要进缩放。

    IOS系统渲染内核分为  WKWebView 和 UIWebView 。WKWebView 已经完美兼容，UIWebView 存在bug。

```

