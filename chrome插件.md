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
var eve = document.createEvent('HTMLEvents');
eve.initEvent('change', true, true);
element.dispatchEvent(eve);
```