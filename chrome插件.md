#chrome插件

##content script向页面中插入js

```javascript
var script = document.createElement('script');
script.scr = url|base64;  // 可以是一个url或者是一个js文件的base64字符串
document.getElementsByTagName('body')[0].appendChild(script);
```

##content script和page script之间的通信方法

在content_script中创建一个隐藏的div，id为content_script_listener，然后在content_script_listener上面监听change事件

在page_script中创建一个隐藏的div，id为page_script_listener，然后在page_script_listener上面监听change事件


当page_script需要向content_script中通信的时候，可以向content_script_listener中发送一个change事件

当content_script需要向page_script中通信的时候，可以向page_script_listener中发送一个change事件

发送事件的方法：
```javascript
var eve = document.createEvent('HTMLEvents');
eve.initEvent('change', true, true);
element.dispatchEvent(eve);
```