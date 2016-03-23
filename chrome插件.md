#chrome���

##content script��ҳ���в���js

```javascript
var script = document.createElement('script');
script.scr = url|base64;  // ������һ��url������һ��js�ļ���base64�ַ���
document.getElementsByTagName('body')[0].appendChild(script);
```

##content script��page script֮���ͨ�ŷ���

��content_script�д���һ�����ص�div��idΪcontent_script_listener��Ȼ����content_script_listener�������change�¼�

��page_script�д���һ�����ص�div��idΪpage_script_listener��Ȼ����page_script_listener�������change�¼�


��page_script��Ҫ��content_script��ͨ�ŵ�ʱ�򣬿�����content_script_listener�з���һ��change�¼�

��content_script��Ҫ��page_script��ͨ�ŵ�ʱ�򣬿�����page_script_listener�з���һ��change�¼�

�����¼��ķ�����
```javascript
// ��Ϣͨ��
/**
 * content script �� page script֮��ͨ�ŵ���
 * @param {String} content id��ȡֵ����
 * @param {String} page    id��ȡֵ����
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

// ����
var msg = new Message('content', 'page', 'page');
msg.onMessage = function(data){
    console.log(data);
};
msg.sendMessage({

});
```