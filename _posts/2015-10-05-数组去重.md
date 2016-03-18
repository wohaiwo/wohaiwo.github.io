---
layout: post
title: Js数组去重
---


###  JavaScript 数组去重


### 1. 第一种和第二种方法都是利用了ES5中indexOf方法来去重数组

{% highlight js %}
<pre>
function unique(arr) {
	var n = [];
	for(var i = 0, len = arr.length; i < len; i++) {
		if(arr.indexOf(arr[i]) === i) {
			n.push(arr[i]);
		}
	}
	return n;
}
</pre>		
{% endhighlight %}


### 2. 

{% highlight js %}
<pre>
function unique(arr) {
	var n = [];
	for(var i = 0, len = arr.length; i < len; i++) {
		if(n.indexOf(arr[i]) === -1) {
			n.push(arr[i]);
		}
	}
	return n;
}	
</pre>
{% endhighlight  %}

### 3. 这个算法需要使用Array.prototype.sort() 方法来排序 但是sort()是利用字典码来排序的 也就是说  [1, 111, 2, 3]在sort()中是成立的。 使用需要在sort传入一个比较函数 

{% highlight js %}
<pre> 
function unique(arr) {
	arr.sort(function(a, b) { return a - b);
	var n = [arr[0]];
	for(var i = 0, len = arr.length; i < len; i++) {
		if(arr[i] !== n[n.length-1])
			n.push(arr[i])
	}
	return n;
}
</pre>
{% endhighlight %}

### 4. hash + es5 来实现  速度最快 
返回的对象 里面的数是字符串 不是数字

{% highlight js %}
<pre>
function unique(arr) {
	var obj = {};
	for(var i = 0, len = arr.length; i < len; i++) {
		obj[arr[i]] = true;
	}
	return Object.keys(obj);
}
</pre>
{% endhighlight  %}

### 5. 方法5与方法4的不同 只是多添加了一个obj对象 来储存数组的boolean属性

{% highlight js %}
<pre>
function unique(arr) {
	var obj = {}, result = [];
	for(var i = 0, len = arr.length; i < len; i++) {
		var key = arr[i];
		if(!obj[key]) {
			result.push(key);
			obj[key] = true;
		}
	}
	return result;
}
</pre>	
{% endhighlight %}

### 6. 该算法实现比较乱 不利于了解   

{% highlight js %}
<pre>
function unique(arr) {
	var n = [];
	for(var i = 0, len = arr.length; i < len; i++) {
		for(var j = i + 1; j < len; j++) {
			if(arr[i] === arr[j]) 
				j = ++i;
			}
				n.push(arr[i]);
	}
	return n;
}
</pre>
{% endhighlight %}