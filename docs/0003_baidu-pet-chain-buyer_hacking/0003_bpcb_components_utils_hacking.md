# bpcb components utils hacking

这里更多的倾向于学习一下，工具类应该怎么写吧，主要是自己也不是专业的。

```Javascript
var Utils = {
    //合并数组
    arrayMerge : function (array1, array2) {
        $.each(array2, function(k, v) {
            array1[k] = v;
        });
        return array1;
    },
    //获取本地存储
    getStorage : function(key, returnObject, isSession) {
        if(isSession == true) {
            var value = sessionStorage.getItem(key);
        } else {
            var value = localStorage.getItem(key);
        }
        if(returnObject == true && value != "") {
            value = JSON.parse(value);
        } else if(returnObject == true ) {
            value = {}
        }
        return value;
    },
    //获取本地存储列表
    getStorageList : function(returnObject, isSession) {
        if(isSession == true) {
            var storage = sessionStorage;
        } else {
            var storage = localStorage;
        }
        var storageList = {};
        for(var i=0;i < storage.length; i++){
            var key = storage.key(i);
            var value = storage.getItem(key);
            if(returnObject == true && value != "") {
                value = JSON.parse(value);
            } else if(returnObject == true ) {
                value = {}
            }
            storageList[key] = value;
        }
        return storageList;
    },
    //设置本地存储
    setStorage : function(key, value, isSession) {
        if(typeof(value) == "object") {
            //TODO 字符串下标转换失败
            value = JSON.stringify(value);
        }
        if(isSession == true) {
            sessionStorage[key] = value;
        } else {
            localStorage[key] = value;          
        }
        return true;
    },
    isEmptyObject: function (obj){
        for(var n in obj){
            return false;
        }
        return true;
    }
}
```

