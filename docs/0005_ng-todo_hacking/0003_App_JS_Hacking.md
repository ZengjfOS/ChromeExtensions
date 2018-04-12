# App Javascript Hacking

## 参考文档

* [Angularjs $scope 里面的$apply方法 和 $watch方法](https://blog.csdn.net/christine95/article/details/49201443)
* [在使用angularjs过程，ng-repeat中track by的作用](https://blog.csdn.net/rangqiwei/article/details/38020667)

## index.html

```HTML
<!DOCTYPE html>
<html ng-app='app'>
<head>
    <title>Angular Todo Chrome Extension</title>
    <link rel="stylesheet" type="text/css" href="lib/bootstrap/css/bootstrap.min.css">
    <link rel="stylesheet" type="text/css" href="css/style.css">
</head>
<body ng-controller='todoCtrl'>
    <div class='container'>

        <div class='row'>
            <div class='col-md-12'>
                <!-- 过滤出todoList中completed = false的个数 -->
                <h5>{{ (todoList | filter: {completed:false}).length}} pending task(s)</h5>
                <!-- 回调add()处理函数 -->
                <form id='todo-form' ng-submit='add()'>
                    <div class="form-group">
                        <input type='text' id='new-todo' ng-model='newContent' class="form-control" placeholder="What you need to do ?"/>
                    </div>
                </form>
            </div>
        </div>

        <div class='row' id="todo-list">
            <!-- 根据id进行排序，每次都是生成一个div，而不是仅仅里面的内容重复 -->
            <!-- css/style.css
                .todo-item.completed .todo-content {
                    color:darkgray;
                    font-weight: normal;
                    text-decoration: line-through;
                }

                {completed: todo.completed}：这里就是true表示使用，false表示不使用
             -->
            <div class='todo-item col-md-12' ng-repeat='todo in todoList track by todo.id| orderBy: createdAt' ng-class='{completed: todo.completed}'>
                <!-- 让对应的todo.completed与checkbox进行绑定，当进行点击的时候，会进行数据同步 -->
                <input type='checkbox' ng-model='todo.completed' ng-click='toggleCompleted()'/>
                <!-- 显示内容 -->
                <span class='todo-content'>{{todo.content}}</span>
                <!-- 调用删除函数，这里直接传递的todo这个对象 -->
                <a class='btn-remove-todo pull-right' ng-click='remove(todo)'><i class='glyphicon glyphicon-remove'></i> </a>
            </div>
        </div>

        <div class='row'>
            <div class='col-md-12'>
                <div id='toolbar'>
                    <!-- 调用删除所有函数 -->
                    <a id="btn-remove-all"
                        ng-click='removeAll()'
                        class='btn btn-small btn-default pull-right'
                        ng-show="todoList.length > 0">
                        <i class="glyphicon glyphicon-remove"></i> Clear all
                    </a>
                </div>
            </div>
        </div>

    </div>
    <script type="text/javascript" src="lib/jquery/jquery-2.1.3.min.js"></script>
    <script type="text/javascript" src="lib/angular/angular.js"></script>
    <script type="text/javascript" src="lib/bootstrap/js/bootstrap.min.js"></script>
    <script type="text/javascript" src="app/app.js"></script>
    <script type="text/javascript" src="app/todoCtrl.js"></script>
    <script type="text/javascript" src="app/todoStorage.js"></script>
</body>
</html>
```

## app/app.js

```Javascript
'use strict';

angular.module("app", []);          // 声明、注册一个叫app的module
```

## app/todoCtrl.js

```Javascript
'use strict';

// 向叫app的module中添加todoCtrl的处理函数
angular.module('app').controller('todoCtrl', function ($scope, todoStorage) {

    $scope.todoStorage = todoStorage;

    // 监视todoStorage.data的数据变化
    $scope.$watch('todoStorage.data', function() {
        $scope.todoList = $scope.todoStorage.data;
    });

    // 提供回到函数
    $scope.todoStorage.findAll(function(data){
        $scope.todoList = data;                         // 这里获取的就是当前系统里保存的所有的数据，数据已经排好序了，从1开始计算的。
        $scope.$apply();
    });

    $scope.add = function() {
        // 获取当前输入框中的数据，保存，清空文本框
        todoStorage.add($scope.newContent);
        $scope.newContent = '';
    }

    $scope.remove = function(todo) {
        todoStorage.remove(todo);
    }

    $scope.removeAll = function() {
        todoStorage.removeAll();
    }

    $scope.toggleCompleted = function() {
        todoStorage.sync();
    }

});
```

## app/todoStorage.js

```Javascript
// 服务是一个函数或对象，可在你的 AngularJS 应用中使用
// 可以认为这是一个class
angular.module('app').service('todoStorage', function ($q) {
    var _this = this;
    this.data = [];

    this.findAll = function(callback) {
        // 将数据从本地存储提取出来，并执行回调函数
        chrome.storage.sync.get('todo', function(keys) {
            // {todo: this.data}，这里的keys.todo就是因为保存的使用用了这种格式
            if (keys.todo != null) {
                _this.data = keys.todo;                         // 获取data
                for (var i=0; i<_this.data.length; i++) {       // 迭代data
                    /**
                     * var todo = {
                     *     id: id,
                     *     content: newContent,
                     *     completed: false,
                     *     createdAt: new Date()
                     * };
                     */
                    _this.data[i]['id'] = i + 1;        // 序号从1开始算
                }
                console.log(_this.data);
                callback(_this.data);                   // 调用回调函数，将当前存储的所有的数据都取出来
            }
        });
    }

    this.sync = function() {
        // 将数据写入本地存储
        chrome.storage.sync.set({todo: this.data}, function() {
            console.log('Data is stored in Chrome storage');
        });
    }

    this.add = function (newContent) {
        var id = this.data.length + 1;
        var todo = {
            id: id,
            content: newContent,
            completed: false,
            createdAt: new Date()
        };
        this.data.push(todo);
        this.sync();
    }

    this.remove = function(todo) {
        // 直接删除对象
        this.data.splice(this.data.indexOf(todo), 1);
        this.sync();
    }

    this.removeAll = function() {
        this.data = [];
        this.sync();
    }

});

```
