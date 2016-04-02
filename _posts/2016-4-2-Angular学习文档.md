---
layout: post
title: angular学习笔记
---



## Angular 

<div class="message">
  最近刚刚入门angular1.x,  总结了一些我学习过程中的对angular的认知和理解. 对于angular来说, 双向绑定数据的功能能实现反应在DOM,免去了我们操作DOM的步骤, angular还来对于SPA(单页面应用)是也有很大的应用场景,它可以使等我们只需要加载一个index.html主页面, 后面的应用可以通过路由,aiax分布加载, 这也是很大的优点, 但是angular1.x也是有很多的缺点, 2.0已经发布, 小白还是先搞懂1.x吧.....
</div>

### $scope-作用域 $rootScope-根作用域(用来设置路由)

### ng-app 作用域标记
> 指令标记了AngularJs脚本的作用域，在<html>中添加ng-app属性说明了整个html都是Angular脚本作用域。 开发者也可以局部使用ng-app指令， 比如 <div ng-app> , 则说明Angular只能在div中作用。

### ng-repeat 遍历属性 
> Angular中遍历属性 用在一个数组上
> <li ng-repeat="i in [1,2,3,4,5]"><span>{{i}}</span></li>

### ng-controller 控制器属性
> 定义了控制器

### ng-init 初始化数据

	{% highlight js %}
	<div ng-app ng-init="quantity=1; cost=5">
		<p>总价: {{ quantity * cost }} </p>
	</div>
	{% endhighlight %}

### ng-click 定义元素响应行为
> eg: 

 	 {% highlight js %}
	<div ng-controller="controller">
		<p>{{greeting.text}}, AngularJs.</p>
		<button ng-click="test1()">test1</button>
	</div>
	<script>
		function controller($scope) {
			$scope.greeting = {
				text: "hello world"
			};
			$scope.test1 = function() {
				console.log("test one");
			};
		}
	</script>
	{% endhighlight %}

## 路由 模块 依赖注入  

### 指令 (需要加强ng中指令的理解)
> 指令基本用法
> tranclude属性
> 其中restrict属性包括四个属性： A(attribute属性) E(element元素) M(comment 注释) C(class 类)
>其中link方法中的attr属性可以提取到自定义元素的属性
	
	{% highlight js %}
	HTML:
	<div ng-app="MyModule" ng-controller="MyCtrl">
		<loader howToLoad="loadData()">loading...</loader>
	</div>
	Js:
	var myModule = angular.module('MyModule', []);
	myModule.controller('MyCtrl', ['$scope', function($scope) {
		$scope.loadData = function() {
			console.log('hello world');
		};
	}]);
	myController.directive("loader", function() {
		return {
			restrict: "AE",
			template: , 	 // 可以用来替换指令中的模板
			/**
			* @scope 作用域 
			* @element 当前指令替换的元素
			* @attr  表示元素里面的属性
			*/  
			link: function(scope, element, attr) {
				element.bind("mouseenter", function() {
					// 注意事项 attr调用方法 里面的属性应该要小写
					// 	可以在不同的控制器中调用同一个方法
					scope.$apply(attr.howtoload);
				});
			}
		};
	};
	{% endhighlight %}

### 独立指令
directive里面可以提交一个scope: {}， 独立指令 就和别的指令不相联系
	
{% highlight js %}
	HTML: 
		<hello></hello>
		<hello></hello>
		<hello></hello>
		<hello></hello>
	JS: 
		myModule.directive("hello", function() {
			return {
				restrict: 'AE',
				scope: {},			/* 独立指令 */
				template: '<div><input type="text" ng-model="userName" /> {{userName}} </div>',
				replace: true
			};
		});
	{% endhighlight %}



### ng-model  双向数据绑定
> 指令把输入域的值绑定到应用程序变量中去 -- **双向绑定**

	{% highlight js %}
	<div> 
		<input type="text" ng-model="name">
		<p>{{name}} , world. </p>
	</div>
	{% endhighlight %}

### 获取当前的URL地址
> 需要一个函数 $location.absUrl() 来获取地址

	{% highlight js %}
	myCtrl.controller("myCtrl", function($scope, $location) {
		$scope.myUrl = $location.absUrl();
	});
	{% endhighlight %}

### $timeout(计时函数) $http(向服务器请求信息)
> $timeout用法

	{% highlight js %}
	myCtrl.controller("myCtrl", function($scope, $timeout) {
		$timeout(function() {
			.....
		}, 2000);
	});
	{% endhighlight %}

> $http用法
	
	{% highlight js %}
	myCtrl.controller("myCtrl", function($scope, $http) {
		$http.get("welcome.html").then(function(response) {
			$scope.myWelcome = response.data;
		});
	});
	{% endhighlight %}

###  过滤器 
> 基本包括 
> currency 格式化数字为货币格式   filter 从数组选项中选择一个子集  
> lowercase 格式化字符串为小写  orderBy 根据某个表达式排列数组 uppercase 格式化字符串为大写    number 显示小数后面的几位  limitTo 可以显示限制出现的数字或者字符串 位数 
>don't bb,  show your code 

	{% highlight js %}
	<p>搜索: <input type="text" ng-model="test"></p>
	<ul>
		<li ng-reapeat="book in books | filter: test | orderBy: 'author'">
			{{book.title}} &nbsp; &nbsp; &nbsp; {{book.author}}
		</li>
	</ul>
	
	<script>
		bookStoreCtrls.controller('BookListCtrl', function($scope) {
			$scope.books = [
				{title: '111',  author: '222'},
				{title: '333',  author: '444'}
			];
		});
	</scipt>
	{% endhighlight %}
	
#### number 显示小数后面的几位

	{% highlight js %}
	Limit{{numbers}} to: <input type="number" step="1" ng-model="{{ numLimit }}">
	output numbers: {{ numbers | limitTo: numLimit }}
	{% endhighlight %}

### 注入依赖 XHR
> 依赖注入服务可以使用web应用构建 

	{% highlight js %}
	function functionName($scope, $http) {
		$http.get('name.json').success(function(data) {
			$scope.phones = data;
		});
	{% endhighlight %}

### ng-show 动态展示显示与隐藏
	
	{% highlight js %}
	// 只有当show为true时,才会显示出包含的标签
	<ul ng-show="show">
		<li ng-click="a()">11</li>
		<li ng-click="b()">22</li>
		<li ng-click="c()">33</li>
	</ul>
	{% endhighlight %}

### ng-class 可以动态绑定css样式
> 原理就是同判断表达式中true,false来实现动态绑定css
	
	{% highlight js %}
	 // 初始化error warning 为false 来不隐藏该元素    只有变为true才会显示出来
	<div ng-class='{error: isError, warning: isWarning}'>
			{{messageText}}
	</div>
	function($scope) {
		$scope.isError = false;
		$scope.isWarning = false;
		$scope.showError = function() {
			$scope.messageText = "你不知道的事";
			$scope.isError = true;
			$scope.isWarning = false;
		};
		$scope.showWarning = function() {
			$scope.messageText = '3.8 妇女节快乐';
			$scope.isError = false;
			$scope.isWarning = true;
		};
	}
	{% endhighlight %}

### ui-router
由于angular本身仅支持在路由转换，而ui-router 根据URL状态或者说是'机器状态'来组织和控制界面UI的渲染



###　基本写法就是:
1.  index.html中声明一个`ui-view` 例如`<div ui-view>` 然后在设置另外一个页面来配置导航栏信息，来说明各个页面的路由转向。 
2.  然后可以创建一个nav.html，里面需要创建一个ul导航栏，例如`<a ui-sref='html页面相对位置，可以不接.html后缀名'>`  然后在该页面里面再嵌套一个`<div ui-view>`  **ui-sref**可以动态指向页面

....(未完待续)
....(坐等踩坑angular2)