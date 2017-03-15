---
title: javascript原型动态性
date: 2017-03-01 8:15:26
---
---
## 实例1
先上代码
```
var friend= new Person();
Person.prototype.sayHi = function(){
    alert("hi");
};
friend.sayHi(); // "hi"(没有一点问题)
```
可以看到,就算我们先创建了实例,后改变了原型里的方法,最后也可以访问到sayHi方法.
**原因:原型链并没有被破坏,当我们调用friend.sayHi()是会在实例中搜索名为sayHi的属性,在没有找到的情况下,会继续搜索原型.因为实例与原型之间的连接只不过是一个指针,而非一个副本,因此就可以在原型中找到新的sayHi属性并返回保存在那里的函数**
<!-- more -->
## 实例2
```
function Person(){

}
var friend = new Person();

Person.prototype = {
	constructor: Person,
	name: "Nicholas",
	age: 29,
	job: "Software",
	sayName: function(){
		alert(this.name);
	}
};
friend.sayName();// "TypeError: friend.sayName is not a function
```
重写原型对象之间
![](/demo/2.png) 
重写原型对象之后
![](/demo/3.png) 
从图中可以看出原型链已经破坏,friend实例还是指向的原来的原型对象.
## 实例3
那我们如果想让让friend访问到sayName()这个方法怎么办,于是我修改了下代码
```
function Person(){

}
var friend = new Person();

friend.__proto__ = {
	constructor: Person,
	name: "Nicholas",
	age: 29,
	job: "Software",
	sayName: function(){
		console.log(this.name);
	}
};
friend.sayName();//"Nicholas"
console.log(Person.prototype == friend.__proto__); // false
```
![](/demo/4.png) 
## 实例4
优化一下
```
function Person(){

}
var friend = new Person();
Object.assign(Object.getPrototypeOf(friend),{
	name: "Nicholas",
	age: 29,
	job: "Software",
	sayName: function(){
		console.log(this.name);
	}
});
friend.sayName(); // "Nicholas"
console.log(Person.prototype == friend.__proto__) // "true"
```
![](/demo/5.png) 