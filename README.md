# AngularJS

## 简介

 AngularJS是谷歌开发的一款前端MVC框架，是以数据和逻辑为驱动核心的框架，最为核心的是：模块化、数据双向绑定。语义化标签、依赖注入。

```javascript

 var App=angular.module('App',[]);

 App.controller('AppController',['$scope',function($scope){

 $scope.say='Hello World';

 }])

```

## MVC

 MVC是一种开发模式，由模型（Model）、视图（View）、控制器（Controller）3部分构成，采用这种开发模式为合理组织代码提供了方便、降低了代码间的耦合度、功能结构清晰可见。

* 模型（Model）一般用来处理数据（读取/设置），一般指操作数据库。
* 视图（View）一般用来展示数据，比如通过HTML展示。
* 控制器（Controller）一般用做连接模型和视图的桥梁。

## 模块化

使用AngularJS构建应用（App）时是以模块化（Module）的方式组织的，即将整个应用划分成若干模块，每个模块都有各自的职责，最终组合成一个整体。

### 定义应用

 通过为任何一个HTML标签添加ng-app属性，可以指定一个应用，表示此标签所包裹的内容都属于应用（App）的一部分。

```html
 <html ng-app="App">

 </html>
```

> 为html标签添加ng-app表明整个文档懂事应用

> ng-app属性可以不赋值，但是关联相应模块时必须赋值

> 一个页面可以有多个模块，但不能嵌套

> 一般只会有一个

### 定义模块

 AngularJS提供了一个angular全局对象，它上面有很多方法和属性，其中angular.module()是用来定义模块的

```javascript
 //通过module方法定义模块，应用app本质也是一个模块
 //module方法需要两个参数
 //参数一：模块的名称
 //参数二：需要依赖的模块
 var app = angular.module('app',[]);

```

### 定义控制器

 控制器（Controller）作为连接模型（Model）和视图（View）的桥梁存在，所以当我们定义好了控制器以后也就定义好了模型和视图。

```javascript
 // app是一个实例模块对象
 // 通过这个实例对象可以创建控制器
 // controller有两个参数
 // 参数1：控制器名称
 // 参数2：是一个数组，除最后一个是函数外，其他的都是字符串，表明控制器的依赖关系
 app.controller('appController',['$scope',function($scope){
     $scope.people = [{'name':'ZS','age':55}]
 }])

```

 模型（Model）数据是要展示到视图（View）上的，所以需要将控制器（Controller）关联到视图（View）上，通过为HTML标签添加ng-controller属性并赋值相应的控制器（Controller）的名称，就确立了关联关系。

```html
 <!-- 添加ng-controller属性，并赋值为相应的控制器名称 -->
 <table ng-controller="appController">
 <tr>
    <th>姓名</th>
    <th>年龄</th>
 </tr>
 <tr>
     <td>{{people.name}}</td>
     <td>{{people.age}}</td>
 </tr>
 </table>

```

## 指令

 HTML在构建应用（App）时存在诸多不足之处，AngularJS通过扩展一系列的HTML属性或标签来弥补这些缺陷，所谓指令就是AngularJS自定义的HTML属性或标签，这些指令都是以ng-做为前缀的，例如ng-app、ng-controller、ng-repeat等。

### 内置指令

* ng-app 指定应用根元素，至少有一个元素指定了此属性。
* ng-controller 指定控制器
* ng-show控制元素是否显示，true显示、false不显示
* ng-hide控制元素是否隐藏，true隐藏、false不隐藏
* ng-if控制元素是否“存在”，true存在、false不存在
* ng-src增强图片路径
* ng-href增强地址
* ng-class控制类名
* ng-include引入模板
* ng-disabled表单禁用
* ng-readonly表单只读
* ng-checked单/复选框表单选中
* ng-selected下拉框表单选中

### 自定义指令

 AngularJS允许根据实际业务需要自定义指令，通过angular全局对象下的directive方法实现。

```javascript
 var App = angular.module('App',[]);
 // 自定义指令
 App.dierctive('tag',function(){
     return {
     // 自定义指令的类型
     // E element
     // A attribute
     // C class
     // M mark replace 必须为true
     restrict:'ECMA',

     // 是否替换原有标签
     replace:true,

     // 指令模版
     template:'<h1>Hello Angular</h1>'
     // 指令外部模版
     // template:'header.html'
     }
 })
```

## 数据绑定

 AngularJS是以数据做为驱动的MVC框架，所有模型（Model）里的数据经由控制器（Controller）展示到视图（View）中。

 所谓数据绑定指的就是将模型（Model）中的数据与相应的视图（View）进行关联，分为单向绑定和双向绑定两种方式。

### 单项绑定

 单向数据绑定是指将模型（Model）数据，按着写好的视图（View）模板生成HTML标签，然后追加到DOM中显示，如之前所学的artTemplate 模板引擎的工作方式。只能模型（Model）数据向视图（View）传递。

### 双向绑定

 双向绑定则可以实现模型（Model）数据和视图（View）模板的双向传递。

### 相关指令

 在AngularJS中通过“{{}}”和ng-bind指令来实现模型（Model）数据向视图模板（View）的绑定，模型数据通过一个内置服务$scope来提供，这个$scope是一个空对象，通过为这个对象添加属性或者方法便可以在相应的视图（View）模板里被访问。

 注：“{{}}”是ng-bind的简写形式，其区别在于通过“{{}}”绑定数据时会有“闪烁”现象，添加ng-cloak也可以解决“闪烁”现象，通过ng-bind-template可以绑定多个数据。

 通过为表单元素添加ng-model指令实现视图（View）模板向模型（Model）数据的绑定。

 通过ng-init可以初始化模型（Model）也就是$scope。

 AngularJS对事件也进行了扩展，无需显式的获取DOM元素便可以添加事件，易用性变的更强。通过在原有事件名称基础上添加ng-做为前缀，然后以属性的形式添加到相应的HTML标签上即可。如ng-click、ng-dblclick、ng-blur等。

 通过ng-repeat可以将数组或对象数据迭代到视图模板中，ng-switch、on、ng-switch-when可以对数据进行筛选。

## 作用域

 通常AngularJS中应用（App）是由若干个视图（View）组合成而成的，而视图（View）又都是HTML元素，并且HTML元素是可以互相嵌套的，另一方面视图都隶属于某个控制器（Controller），进而控制器之间也必然会产生嵌套关系。

 每个控制器（Controller）又都对应一个模型（Model）也就是$scope对象，不同层级控制器（Controller）下的$scope便产生了作用域。

### 根作用域

 一个AngularJS的应用（App）在启动时会自动创建一个根作用域$rootScope，这个根作用域在整个应用范围（ng-app所在标签以内）都是可以被访问到的。

```html
 <!-- 指定一个div为应用的根元素，这个div根元素对应的便是$rootScope -->
 <!-- 通过ng-init为$rootScope添加数据 -->
 <div ng-app="App" ng-init="name='ZS'">
     <span>{{name}}</span>
 </div>
```

### 子作用域

 通过ng-controller指令可以创建一个子作用域，新建的作用域可以访问其父作用域的数据。

## 过滤器

 在AngularJS中使用过滤器格式化展示数据，在“{{}}”中使用“|”来调用过滤器，使用“:”传递参数。

### 内置过滤器

1. currency将数值格式化为货币格式
2. date日期格式化，年（y）、月（M）、日（d）、星期（EEEE/EEE）、时（H/h）、分（m）、秒（s）、毫秒（.sss），也可以组合到一起使用。
3. filter在给定数组中选择满足条件的一个子集，并返回一个新数组，其条件可以是一个字符串、对象、函数
4. json将Javascrip对象转成JSON字符串。
5. limitTo取出字符串或数组的前（正数）几位或后（负数）几位
6. lowercase将文本转换成小写格式
7. uppercase将文本转换成大写格式
8. number数字格式化，可控制小位位数
9. orderBy对数组进行排序，第2个参数可控制方向

### 自定义过滤器

 通过模块对象实例提供的filter方法自定义过滤器。

```html
 <!DOCTYPE html>
 <html ng-app="App">
 <head lang="en">
     <meta charset="UTF-8">
     <title></title>
 </head>
 <body>
     <div ng-controller="DemoController">
         {{content|uppercase}}
     </div>

 <script type="text/javascript">
     var App = angular.module('App',[]);
     App.controller('DemoController', ['$scope', function($scope){
         $scope.content = 'my name is Hanmeimei';
     }])

 // 自定义过滤器
 App.filter('upperCase',function () {
     return function (info) {
         return info[0].toUpperCase() + info.slice(1);
     }
 })
 </script>
 </body>
 </html>

```

## 依赖注入

 AngularJS采用模块化的方式组织代码，将一些通用逻辑封装成一个对象或函数，实现最大程度的复用，这导致了使用者和被使用者之间存在依赖关系。

 所谓依赖注入是指在运行时自动查找依赖关系，然后将查找到依赖传递给使用者的一种机制。

 常见的AngularJS内置服务有$http、$location、$timeout、$rootScope等



### 推断式注入

 没有明确声明依赖，AngularJS会将函数参数名称当成是依赖的名称。

```javascript
 // 控制器依赖$scope和$http模块
 // 但并没有申明依赖，这时候会自动将函数中的参数当做依赖对待
 App.controller('DemoController',function($scope, $http){
     // 发送ajax请求
     $http({
         method:'GET',
         url:'xxx.php',
     });
 })
```

> 这种方式会带来一个问题，当代码经过压缩后函数的参数被压缩，这样便会造成依赖无法找到。

### 行内注入

 以数组形式明确声明依赖，数组元素都是包含依赖名称的字符串，数组最后一个元素是依赖注入的目标函数。

```javascript
 App.controller('DemoController',['$scope', '$http', '$filter', function($scope, $http, $filter){
 // 发送ajax请求
     $http({
         method:'POST',
         url:'xxx.php',
     });
 }])
```

## 服务

 服务是一个对象或函数，对外提供特定的功能。

### 内建服务(内置服务)



1. $location是对原生Javascript中location对象属性和方法的封装。
```javascript
 // $location内置对象
 App.controller('DemoController',['$scope', '$location', function($scope, $loaction){
     $location.absUrl(); // 绝对路径
     $location.protocol(); // 协议
     $location.port(); // 端口
     $location.path(); // 当前路径
     $location.hash(); // 哈希值
     $location.search(); // 查询字符串
 }])
```
2. $timeout&$interval对原生Javascript中的setTimeout和setInterval进行了封装。
```javascript
 App.controller('DemoController',['$timeout', '$interval', '$scope', function($timeout, $interval, $scope){
     $timeout(function(){
         $scope.time = new Date();
     }, 2000);
     $interval(function(){
         $scope.time = new Date();
     }, 1000)
 }])
```
3. $filter在控制器中格式化数据。
```javascript
 // 使用过滤器服务
 App.controller('DemoController', ['$scope', '$filter', function($scope, $filter){
     // 初始内容
     $scope.content = '100';
     // 创建过滤器
     var currency = $filter('currency');
     // 格式化数据
     $scope.content = currency($scope.content);
}])
```
4. $log打印调试信息
```javascript
 // 使用$log服务
 App.controller('DemoController', ['$log', function($log){
     $log.log('日志');
     $log.info('信息');
     $log.warn('警告');
     $log.error('错误')；
     $log.debug('调试');
 }])
```
5. $http用于向服务端发起异步请求。
```javascript
 // 使用$http服务
 App.controller('DemoController', ['$scope', '$http', function($scope, $http){
     // 发送ajax请求
     $http({
         method:'GET', // 请求方式
         url:'xxx.php', // 请求页面
         data:{ // 请求主体
             name:'ZS',
             age:18
         },
         param:{ // 以?key1=value1&key2=value2这种格式将参数拼接到url后面
             key1:'value1',
             key2:'value2'
         },
         headers:{ // 请求头信息
             'Content-Type':'application/x-www-form-urlencoded'
         }.success(function(data, status, headers, config){
             // 成功回调
         }).error(function(data, status, headers ,config){
             // 失败回调
         });
     })
 }])
```
> 同时还支持多种快捷方式，如$http.get()、$http.post()、$http.jsonp。

### 自定义服务

 服务是将一些通用性的功能逻辑进行封装方便使用，AngularJS允许将自定义服务。

1. factory方法
```javascript
     // 自定义显示当前日期服务
     App.factory('showTime', ['$filter', function($filter){
         var now = new Date();
         return $filter('date')(now, 'yyyy/MM/dd');
     }]);
     // 声明依赖调用服务
     App.controller('DemoController', ['$scope', 'showTime', function($scope, showTime){
         $scope.time = showTime;    
     }])
```
2. service方法
```javascript
     // 自定义显示当前日期服务
     App.service('showTime', ['$filter', function($filter){
         var now = new Date();
         return $filter('date')(now, 'yyyy/MM/dd');
     }]);
     // 声明依赖调用服务
     App.controller('DemoController', ['$scope', 'showTime', function($scope, showTime){
         $scope.time = showTime;
     }]);
```
3. value方法定义常量
```javascript
     // 自定义常量服务
     App.value('author', 'Name');
     // 声明依赖调用服务
     App.controller('DemoController', ['$scope', 'author', function($scope, author){
         $scope.author = author;
     }])
```

 在介绍服务时曾提到服务本质就是一个对象或函数，所以自定义服务就是要返回一个对象或函数以供使用。

## 模块加载

 AngularJS模块可以在被加载和执行之前对其自身进行配置。我们可以在应用的加载阶段配置不同的逻辑。

### 配置块

 通过config方法实现对模块的配置，AngularJS中的服务大部分都对应一个“provider”，用来执行与对应服务相同的功能或对其进行配置。比如$log、$http、$location都是内置服务，相对应的“provider”分别是$logProvider、$httpProvider、$locationPorvider。

```javascript
     // 对$log服务进行配置
     App.config(['$logProvider', function($logProvider){
     // 关闭debug级别的信息提示
         $log.debugEnabled(false);
     }]);
     App.controller('DemoController', ['$scope', '$log', function($scope, $log){
         $scope.showLog = function(){
             $log.log('日志');
             $log.warn('警告');
             // 已关闭，将不再生效
             $log.debug('调试');
         };
     }]);
```

```javascript
 // 对$filter服务进行配置,实现相同的功能
 App.config(['$filterProvider', function($filterProvider){
     // 注册study过滤器
     $filterProvider.register('study', function(){
         return function(input, arg){
             return input + 'study AngularJS' + arg;
         }
     });
 }])
```
### 运行块

 服务也是模块形式存在的对且对外提供特定功能，除了将服务做为依赖注入进去的，然后再进行调用外，也可以直接运行相应的服务模块，AngularJS提供了run方法来实现。

```javascript
 // 运行$http,$rootScope服务
 App.run(['$http', '$rootScope', function($http, $rootScope){
     $http({
         method:'post',
         url:'xxx.php'
         }).success(function(data){
             $rootScpoe.data = data;
         });
     }])
```

 不但如此，run方法还是最先执行的，利用这个特点可以将一些需要优先执行的功能通过run方法来运行，比如验证用户是否登录，未登录则不允许进行任何其它操作等。

## 路由

 一个应用是由若个视图组合而成的，根据不同的业务逻辑展示给用户不同的视图，路由则是实现这一功能的关键。

### SPA

 SPA（Single Page Application）指的是通单一页面展示所有功能，通过Ajax动态获取数据然后进行实时渲染，结合CSS3动画模仿原生App交互，然后再进行打包（使用工具把Web应用包一个壳，这个壳本质上是浏览器）变成一个“原生”应用。

 在PC端也有广泛的应用，通常情况下使用Ajax异步请求数据，然后实现内容局部刷新，局部刷新的本质是动态生成DOM，新生成的DOM元素并没有真实存在于文档中，所以当再次刷新页面时新添加的DOM元素会“丢失”，通过单页面应可以很好的解决这个问题。

### 路由

 在后端开发中通过URL地址可以实现页面（视图）的切换，但是AngularJS是一个纯前端MVC框架，在开发单页面应用时，所有功能都在同一页面完成，所以无需切换URL地址（即不允许产生跳转），但Web应用中又经常通过链接（a标签）来更新页面（视图），当点击链接时还要阻止其向服务器发起请求，通过锚点（页内跳转）可以实现这一点。


 实现单页面应用需要具备：

 a. 只有一页面

 b. 链接使用锚点

```javascript
 // 监听锚点变化然后发送请求
 // hashchange事件可以监听锚点变化
 window.addEventListener('hashchange', function () {
     // 获取锚点
     var hash = location.hash;
     // 处理#
     hash = hash.slice(1);
     // 实例异步对象
    var xhr = new XMLHttpRequest;
     // 将锚点做为参数传递给服务端进处理
     xhr.open('get', 'xxx.php?hash=' + hash);
     xhr.send(null);
     xhr.onreadystatechange = function () {
         if(xhr.readyState == 4 && xhr.status == 200) {
             var result = xhr.responseText;
             // 将返回结果添加到页面
             document.querySelector('xxx').innerHTML = result;
         }
     }
 });
```

 通过上面的例子发现在单一页面中可以能过hashchange事件监听到锚点的变化，进而可以实现为不同的锚点准不同的视图，单页面应用就是基于这一原理实现的。

 AngularJS对这一实现原理进行了封装，将锚点的变化封装成路由（Route）,这是与后端路由的根本区别。

 在1.2版前路由功能是包含在AngularJS核心代码当中，之后的版本将路由功能独立成一个模块

#### 使用方法

1. 引入angular-route.js
```html
 <!-- Angular核心框架 -->
 <script type="text/javascript" src="./libs/angular.min.js"></script>
 <!-- Angular路由模块 -->
 <script type="text/javascript" src="./libs/angular-route.js"></script>
```
2. 实例化模块时，当成依赖传进去（模块名称叫ngRoute）
```javascript
 // 作为依赖传入
 var App = angular.module('App', ['ngRoute']);
```
3. 配置路由模块
```javascript
 // 通过$routeProvider配置
 App.config(['$routeProvider', function($routeProvider){
     // 配置路由
     $routeProvider.when('/',{
         template:'index'
     }).when('/2',{
         templateUrl:'2.html'
     });
 }])
```
4. 布局模板

 通过ng-view指令布局模板，路由匹配的视图会被加载渲染到些区域。

```html
 <header>头部</header>
 <div>
     <!-- 视图会被加载并渲染到此处 -->
     <div ng-view></div>
 </div>
 <footer>底部</footer>
```

#### 路由参数

1. 提供两个方法匹配路由，分别是when和otherwise，when方法需要两个参数，otherwise方法做为when方法的补充只需要一个参数，其中when方法可以被多次调用。

2. 第1个参数是一个字符串，代表当前URL中的hash值。

3. 第2个参数是一个对象，配置当前路由的参数，如视图、控制器等。

 a. template 字符串形式的视图模板

 b. templateUrl 引入外部视图模板

 c. controller 视图模板所属的控制器

 d. redirectTo跳转到其它路由

4. 获取参数，在控制中注入$routeParams可以获取传递的参数

```javascript
 // 依赖ngRoute模块
 var App = angular.module('App', ['ngRoute']);
 // 需要对路由模块进行配置，使其正常工作
 App.config(['$routeProvider', function ($routeProvider) {
 // : 表示变量
     $routeProvider.when('/index/:id/:page/:p', {
         templateUrl: 'abc.html',
         controller: 'IndexController'
     })
     .otherwise({
         redirectTo: '/index'
     });
 }]);
 // 提供了一个专门负责获取参数的一个服务$routeParams
 App.controller('IndexController', ['$scope', '$http', '$routeParams', function ($scope, $http, $routeParams) {
     $scope.content = $routeParams;
 }]);
```

## jQlite

 在没有引入jQuery的前提下AngularJS实现了简版的jQuery Lite，通过angular.element不能选择元素，但可以将一个DOM元素转成jQuery对象，如果引提前引入了jQuery则angular.element则完全等于jQuery。
