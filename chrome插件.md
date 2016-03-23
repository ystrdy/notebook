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
// 消息通信
/**
 * content script 和 page script之间通信的类
 * @param {String} content id，取值任意
 * @param {String} page    id，取值任意
 * @param {String} context page|content
 */
var Message = function(content, page, context){
    this.content = content;
    this.page = page;
    this.context = context;

    this.init();
};

Message.prototype = {
    init : function(){
        [this.content, this.page].forEach(function(id){
            var _div = document.getElementById(id);
            if (!_div) {
                _div = document.createElement('div');
                _div.id = id;
                document.getElementsByTagName('body')[0].appendChild(_div);
            }
            if (this[this.context] === id) {
                var self = this;
                _div.addEventListener('change', function(event){
                    if (self.onMessage) {
                        self.onMessage(JSON.parse(this.innerHTML));
                        this.innerHTML = '';
                    }
                    event.preventDefault();
                    event.stopPropagation();
                }, false);
            }
        }, this);
    },
    sendMessage : function(data){
        var _div = document.getElementById(this[this.context === 'page' ? 'content' : 'page']);
        if (_div) {
            var eve = document.createEvent('HTMLEvents');
            eve.initEvent('change', false, true);
            eve.___extra = data;
            _div.innerHTML = JSON.stringify(data);
            _div.dispatchEvent(eve);
        }
    }
};

// 例子
var msg = new Message('content', 'page', 'page');
msg.onMessage = function(data){
    console.log(data);
};
msg.sendMessage({

});
```