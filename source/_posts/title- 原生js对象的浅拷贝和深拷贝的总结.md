---

title: 原生js对象的浅拷贝和深拷贝的总结
date: 2017-03-05 00:34:14
---

这里是说明.



----------------------------------------------------------------

在此之前我们先复习两个知识点.
## 第一个知识点:对象方括号表示法
* 一般来说,访问对象属性时使用的都是点表示法,这也是很多面向对象语言中通用的语法.不过在Javascript也可以用方括号表示法来访问对象的属性.在使用方括号语法时,**应该将要访问的属性以字符串的形式放在方括号中**

```
var person = {
    name : "gay"
}
alert(person["name"]); // "gay"

alert(person.name); // "gay
```
### 优点
可以通过变量来访问属性,例如:
```
var propertyName = "name";

alert(person[propertyName]);  // "gay"
```
如果属性名中包含会导致语法错误的字符,或者属性名使用的是关键字火保留字,也可以使用方括号表示法.例如:
```
person["first name"] = "gay";
```
由于"first name"中包含一个空格,所以不能使用点表示发来访问它.然而,属性名中是可以包含非字母非数字的,这时候就可以使用方括号表示法来访问它们.
* 通常,除非必须使用变量来访问属性,否则我们建议使用点表示法.
## 第二个知识点:函数传递参数:
* ECMAScript中所有的函数都是按值传递!!!! -----高程第三版P70
* ECMAScript中所有的函数都是按值传递!!!! -----高程第三版P70
* ECMAScript中所有的函数都是按值传递!!!! -----高程第三版P70
* 函数内部声明的变量都是临时变量!在函数执行完之后也会被销毁!!!
* 函数内部声明的变量都是临时变量!在函数执行完之后也会被销毁!!!
* 函数内部声明的变量都是临时变量!在函数执行完之后也会被销毁!!!
* 在JS中,如果一个引用类型赋值给一个变量,那么这个变量装的是这个对象的地址!!!
* 在JS中,如果一个引用类型赋值给一个变量,那么这个变量装的是这个对象的地址!!!
* 在JS中,如果一个引用类型赋值给一个变量,那么这个变量装的是这个对象的地址!!!
其实记住上面三句话,可以理解好多问题
<!-- more -->
几个例子解决你疑惑
### 第一个例子:
```
function addTen(num){
  num += 10;
}
var count = 20;
addTen(count);
console.log(count);//20
console.log(num);//"ReferenceError: num is not defined
```
分析一下函数 :
这个函数里面声明了一个临时变量num,,然后这个临时变量会在函数结束后消失.
所以当我们即使把count的值传给num,也不会影响count的值.
而最后输出num的值也会出现未定义错误.
### 第二个例子
```
function addTen(num){
  num += 10;
  return num;
}
var count = 20;
var result = addTen(count);
console.log(count);//20
console.log(result);//30
```
分析:
如果我们想要函数里面的那个临时变量num的值该怎么办呢? 那么我们就要把它return出来,
最后可以看出 count输出20 ,result(也就是临时变量num的值,num变量在函数调用后已经消失,它的唯一作用就是临死前告诉result它的值是多少)是30.
### 第三个例子
```
var person = {
  name : "wsy"
}
function setName(obj){
   obj.name = "gay"
}
setName(person);
console.log(person.name);// "gay"
console.log(obj.name);//"ReferenceError: obj is not defined
```
分析:
有人可能会说,你不是说是值传递么,为什么还会改变原来对象name的值.
可是我还说了一句话
* 在JS中,如果一个引用类型赋值给一个变量,那么这个变量装的是这个对象的地址!!!
setName(person);
进入函数后,我们定义一个临时变量obj,这个obj里装的是person对象的地址.注意是地址.学过c语言的同学肯定好理解,这个obj说白了就是一个指针变量呗.
所以,当我们obj.name = "gay"改变的就是原来那个对象的name,因为他们共享一个地址.所以console.log(person.name);// "gay".
又因为obj只是一个临时变量,所以在函数外输出obj.name肯定找不到了.因为obj已经挂了.
### 第四个例子
```
function setName(obj){
   obj.name = "gay"
   obj = new Object();
   obj.name = "les"
}
var person = new Object();
setName(person);
console.log(person.name);//"gay"
```
分析:
我们定义一个person对象.
setName(person);
然后进入函数,首先给临时变量obj给一个值(person的地址).然后obj.name = "gay",因为obj和person共享一个地址,所以person的name属性也变成了"gay".
然后 obj = new Object(); .注意这里 重新new了一个对象,(也就是重新在堆内存里分配了一块地址)给临时变量obj.此时,obj里装的地址和person的地址并不是一个值.也就
是说改变obj.name并不会影响到person.
### 最后一个例子
```
function setName(obj){
   obj.name = "gay"
   obj = new Object();
   obj.name = "les"
   return obj;
}
var person = new Object();
var person1 = setName(person);
console.log(person.name);//"gay"
console.log(person1.name);//"les"
```
这个例子无非就是想把这个新开辟的obj返回出来.so easy~
### 总结
其实我们发现,红皮书说的真好,js函数传递就是值传递.可为什么传递引用类型时会改变原来的值呢?是因为传引用对象时,其实传递的是他的地址.所以他们共享地址了.
这就是传说中的 call by shareing!
OK,让我们进入正题!
## 浅拷贝:
简单讲，浅拷贝就是复制一份引用，所有引用对象都指向一份数据，并且都可以修改这份数据

```
var person = {
  name : "wsy"
}
var person1 = person;
person1.name = "yxy";
console.log(person.name); // yxy
```

从上述代码中我们可以发现,改变person1的name值然后person的值也跟着改变.
内存分析图:

再看一段代码:

```
var Chinese = {
    name : "China"
}
function extendCopy(p) {
    var c = {};
    for (var i in p) {
        c[i] = p[i];
    }
    return c;
}
var Doctor = extendCopy(Chinese);
alert(Doctor.name);//China
Doctor.name = "USA"
alert(Chinese.name);//China
```

解释下这个函数,创建一个c对象,然后  c["name"] = p["name"];
但是 改变c的name属性并不影响p的属性.
再往下看

```
Chinese.birthPlaces = ['北京','上海','香港'];
var Doctor = extendCopy(Chinese);
Doctor.birthPlaces.push('厦门');
alert(Doctor.birthPlaces); //北京, 上海, 香港, 厦门
alert(Chinese.birthPlaces); //北京, 上海, 香港, 厦门
```
假如我们给Chinese添加一个属性,这个属性为一个数组对象.
然后再进行extendCopy函数赋值给Doctor.我们会发现改变Doctor的值会影响Chinese的值.
也就是说Doctor.birthPlaces 和 ChinesePlaces指向了同一块内存.
经实验,我们发现在extendCopy(p)函数中:
如果参数p的某一个属性为基本类型.则为值传递(也就是仅仅简简单单的赋值)
如果参数p的某一个属性为引用类型(对象),则为引用传递(这俩个对象的这个属性指向同一块内存)
所以，extendCopy() 只是拷贝了基本类型的数据，我们把这种拷贝叫做“浅拷贝”。
知乎用户*MickeyHong*:Javascript 对于复制的问题其实有些模糊 不过总的来说 你只要记住Object在Javascript里是pass by reference的 其余的都是pass一个复制值 (我知道有人会吵>javascript都是pass by value的 而obj的value就是reference什么什么的)
## 深拷贝
* 所谓”深拷贝”，就是能够实现真正意义上的数组和对象的拷贝。它的实现并不难.深拷贝则是复制变量值，对于非基本类型的变量，则递归至基本类型变量后，再复制
* 所谓”深拷贝”，就是能够实现真正意义上的数组和对象的拷贝。它的实现并不难.深拷贝则是复制变量值，对于非基本类型的变量，则递归至基本类型变量后，再复制
* 所谓”深拷贝”，就是能够实现真正意义上的数组和对象的拷贝。它的实现并不难.深拷贝则是复制变量值，对于非基本类型的变量，则递归至基本类型变量后，再复制
直接撸代码:
```
var Chinese = {
    birthPlaces : ['北京','上海','香港']
}
function deepCopy(p,c) {
    var c = c || {};
    for (var i in p) {
        if (typeof p[i] === 'object') {
            c[i] = (p[i].constructor === Array) ? [] : {};
            //alert(i);  // i = birthPlace
            //alert( c[i]);//空对象
            //alert(p[i]);//['北京','上海','香港'];
            deepCopy(p[i], c[i]);
        } else {
            c[i] = p[i];
        }
    }
    return c;
}
var Doctor = deepCopy(Chinese);
Doctor.birthPlaces.push('厦门');
alert(Doctor.birthPlaces); //北京, 上海, 香港, 厦门
alert(Chinese.birthPlaces); //北京, 上海, 香港
```
这里我们实现了就算这个对象的某一个属性为Object类型的,我们可以让这两个对象的这个属性指向不同的内存.
* 所谓”深拷贝”，就是能够实现真正意义上的数组和对象的拷贝。它的实现并不难.深拷贝则是复制变量值，对于非基本类型的变量，则递归至基本类型变量后，再复制

ok,分析一下函数:
```
function deepCopy(p,c) {
    var c = c || {};
    for (var i in p) {
        if (typeof p[i] === 'object') {
            c[i] = (p[i].constructor === Array) ? [] : {};
            //alert(i);  // i = "birthPlace"
            //alert( c[i]);//空对象
            //alert(p[i]);//['北京','上海','香港'];
            deepCopy(p[i], c[i]);
        } else {
            c[i] = p[i];
        }
    }
    return c;
}
```


分析一下运行过程:
**var Chinese = {
    birthPlaces : ['北京','上海','香港']
}**
现在Chinese只有一个属性,这个属性的值是一个数组(Array)
一步一步分析:

**var Doctor = deepCopy(Chinese);**
给deepCopy传进入一个参数Chineese.

**var c = c || {};**
定义一个变量 c, 这个c的值是怎么计算的呢? 其实这里用了||的特性,如果传进来的c不为null那么新定义的c的值就是传进来的c的值,否则新定义的c等于一个空对象({ })
.而我们第一次调用deepCopy时,只传进来一个一个参数,所以.   这里 var c = {}; 也就是定义c是一个空对象.\

**for (var i in p)**
这里由于p是一个对象,所以这里面的i值是循环p的属性.由于Chinese只有birthPlaces一个属性,所以只循环一次,i的值就是 "birthPlaces"(string类型).

**if (typeof p[i] === 'object')**
判断p[i]是不是Object类型的.这里面p[i]就是p["birthPlaces"]  那么肯定是Object啊(这里用到的前面复习的第一个知识点::对象方括号表示法)

**c[i] = (p[i].constructor === Array) ? [] : {};**
判断p[i]到底是哪个Object类型的,如果是数组那么;c["birthPlaces"]为空数组,如果是对象那么:c["birthPlaces"]为空对象.

**deepCopy(p[i], c[i]);**
也就是deepCopy(p["birthPlaces"],c["birthPlaces"]) 
也就是deepCopy(['北京','上海','香港'],[]) 

ok,我们再进入一遍这个函数 
注意,刚才我们传进去俩个参数,p = ['北京','上海','香港'],c = [];

 **var c = c || {};**
然后 c = [];

**for (var i in p)**
这里p一个数组,所以i是这个数组的三个索引为 "0", "1" "2"所以进行三次循环 (注意这个索引是string类型的 )

**if (typeof p[i] === 'object')**
当i = "0"时,p["0"] = "北京",   而北京是一个string类型的.依次类推,这三次循环永远不会进入这个if语句里.

```
else {

            c[i] = p[i];
        }
```
循环三次后,因为c本来就是一个数组.所以最后 c = ['北京','上海','香港']因为这个c和c["birthPlaces"]共享地址,所以c["birthPlaces"] = ['北京','上海','香港'];

**return c;**
在函数外var Doctor = deepCopy(Chinese);来接受这个我们在函数内新var的临时变量.
总结:
* 深拷贝则是复制变量值，对于非基本类型的变量，则递归至基本类型变量后，再复制
* 深拷贝则是复制变量值，对于非基本类型的变量，则递归至基本类型变量后，再复制
* 深拷贝则是复制变量值，对于非基本类型的变量，则递归至基本类型变量后，再复制