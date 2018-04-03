# bpcb manifest.json Hacking

查看manifest可以知道插件支持了哪些功能，里面background可以知道注册哪些事件。

## manifest.json Code Hacking

```JSON
{
    "manifest_version": 2,
    "name": "百度莱茨狗",                                           // 软件名
    "description": "百度莱茨狗抢购插件",
    "version": "2.1.3",
    "background": {
        "scripts": ["js/libs/jquery.min.js", "js/background.js"]    // 后台加载的js文件，background.js有注册图标点击事件，这样才会出现点不是弹出popup页面，而是跳转到index页面
    },
    "browser_action": {
        "default_icon": "images/icon.png",                          // 插件显示的图标和标题
        "default_title": "百度莱茨狗抢购插件"
    },
    "permissions": [                                                // 跨域、本地访问权限
        "tabs",            
        "https://pet-chain.baidu.com/*",         
        "webRequestBlocking",
        "webRequest",
        "browsingData",
        "storage",
        "unlimitedStorage"
    ],
    "icons" : {
        "128": "images/icon.png"
    },
    "options_page":"options.html"                                   // options页面
}
```

## js/background.js Code Hacking

从这里可以知道为什么点击浏览器中的图标，会跳转到index.html界面：

```Javascript
chrome.browserAction.onClicked.addListener(function(){ 
     chrome.tabs.create({ url: 'index.html' });
});
```
