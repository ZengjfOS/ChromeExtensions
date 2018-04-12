# manifest Hacking

```
{
  "manifest_version": 2,
  "name"            : "ng-Todo",
  "description"     : "Minimal todo app for minimalist",
  "short_name"      : "ng-Todo",
  "version"         : "0.1",
  "browser_action": {
    "default_popup": "index.html",              // 弹出是页面
    "default_icon" : "images/logo.png"          // logo
  },
  "permissions": [                              // 访问权限
    "debugger",
    "storage"
  ],
  "content_security_policy": "script-src 'self' 'unsafe-eval' https://ssl.google-analytics.com; object-src 'self'"
}
```
