### 11. prototype 和 \__proto__的区别

- 每个对象都具有一个名为**\__proto__**的属性；
- 每个对象的**\__proto__**属性指向自身构造函数的**prototype**；
- **prototype**是构造函数独有的属性；

- 每个构造函数（构造函数标准为大写开头，如Function()，Object()等等JS中自带的构造函数，以及自己创建的）都具有一个名为*prototype*的方法（注意：既然是方法，那么就是一个对象（JS中函数同样是对象），所以**prototype**同样带有**\__proto__**属性）；

- 所有构造函数的的**prototype**方法的__都指向__Object.prototype(除了....Object.prototype自身)；

### 12. 继承的实现方式及比较

**构造函数绑定**：将父构造函数的this通过apply/call指向子构造函数

```js
// 人类的构造函数　
function Person(species){
　　　this.species = species;
}
Person.prototype.sex='man'
//中国人的构造函数
function Chinese(species,name,skin){
   　Person.apply(this, arguments); //调用person方法，this指向子构造函数实现继承
     this.name=name;
　　 this.skin = skin ;
}
 
var chinese = new Chinese("会说话","小明","黄色");
console.log(chinese.species); // 会说话
console.log(chinese.sex); //undefined
```

**优点：**

- 简单快捷
- 创建子类实例，可以向父类传递参数
- 可以实现多继承(apply多个父类对象)

**缺点：**

- 不可继承父类的原型属性 / 方法
- 实例只是子类的实例不是父类的实例

**原型链继承**

```js
function Person(){
　　　this.species = "会说话";
}
 
//中国人的构造函数
function Chinese(name,skin){
     this.name=name;
　　 this.skin = skin ;
}
 
Chinese.prototype=new Person();
Chinese.prototype.constructor = Chinese;
//Chinese.prototype=new Person()会把Chinese.prototype的constructor指向了Person
//如果不手动改过来就会导致会导致继承链的紊乱。


var chinese = new Chinese("小明","黄色");
console.log(chinese.species); // 会说话
```

**优点：**

- 纯粹的继承关系，实例既是子类的实例，又是父类的实例
- 父类新增的原型方法、属性，子类都能访问到；
- 简单、易于实现

**缺点：**

- 性能低，要建立父类的实例，浪费内存
- 来自原型对象的所有属性被所有实例共享，改变实例属性会同时改变原型属性

**prototype 继承**

```js
function Person(){
　　　
}
Person.prototype.species="会说话"
function Chinese(name,skin){
     this.name=name;
　　 this.skin = skin ;
}
 
Chinese.prototype=Person.prototype; //指向父类的prototype
Chinese.prototype.constructor = Chinese
 
var chinese = new Chinese("小明","黄色");
console.log(chinese.species); // 会说话
Chinese.prototype.species="好客" 
console.log(Person.prototype.species)// 好客
```

**优点：**

- 不用建立父类的对象，省内存

**缺点：**

- 修改子类的prototype属性会同时修改父类的prototype属性（父类的prototype和子类的prototype指向同一个对象）

**Object.create**

```js
function Person(){
　　　
}
Person.prototype.species="会说话"
function Chinese(name,skin){
     this.name=name;
　　 this.skin = skin ;
}
 
Chinese.prototype=Object.create(Person.prototype); //Object.create方法
Chinese.prototype.constructor = Chinese
 
var chinese = new Chinese("小明","黄色");
console.log(chinese.species); // 会说话
Chinese.prototype.species="好客" 
console.log(Person.prototype.species)// 会说话
```

**优点：**

- 这种方式是目前常用的一种继承方式

- 效率高，又解决了`Chinese.prototype`和`Person.prototype`共享指针内存问题

**拷贝继承**

```js
function Person(){
　　　
}
Person.prototype.species="会说话"
function Chinese(name){
  var person= Person.prototype;
  for(var p in person){
    Chinese.prototype[p] = person[p];
  }
  Chinese.prototype.name = name || 'Tom';
}
 
var chinese= new Chinese();
console.log(chinese.species); //会说话
console.log(chinese instanceof Person); // false
console.log(cinese instanceof Chinese); // true
```

**优点：**

- 支持多继承

**缺点：**

- 效率低，内存占用高（要拷贝父类的属性）
- 无法获取父类不可枚举的方法（不可枚举的方法，无法用for in访问到）

### 13. 深拷贝和浅拷贝

**前言：**JS中纯在基本类型和引用类型，基本类型中不存在深浅拷贝，引用类型才有，在JS中，万物皆对象，数组（Array），函数（function），对象（Object），date，regexp，error都是引用类型。

- 浅拷贝：只拷贝对象的引用，两者指向相同地址，改变A，B也会变

- 深拷贝：单独开辟一块新的堆空间，不会相互影响

**深拷贝**

**序列化与反序列化法实现深拷贝：**该方法只能深拷贝对象和数组，对于其他种类的对象，会失真，因此比较适合在开发中使用。

使用`JSON.stringify`将Object转化为Json字符串，然后在用`JSON.parse`将json字符串转为Object对象。

```js
function deepClone(obj){
    let _obj = JSON.stringify(obj),  //转为Json字符串
        objClone = JSON.parse(_obj);  //Json字符串转为Json对象
    return objClone   
}    
let a=[0,1,[2,3],4],
    b=deepClone(a);
a[0]=1;
a[2][0]=1;
console.log(a,b);
```

> 深拷贝手撕部分建议查看*手撕代码*，这里不做举例。

### 14. 防抖和节流

- **防抖(debounce)**：触发事件后一段时间内内函数只会执行一次，如果在这段时间事件再次被触发，则重新计算时间

- **节流(throttle)**：事件被触发，但在一段时间内只会执行一次，所以节流会稀释函数的执行频率

>防抖是将多次执行变为最后一次执行，节流是将多次执行变成每隔一段时间执行。

> 防抖节流有*手撕代码*部分，这里不做举例

### 15. 作用域和作用域链、执行期上下文`（重要）`

- 作用域：

```js
var scope="global";
function t(){
    console.log(scope);
    var scope="local"
    console.log(scope);
}
t(); //输出undefined  local

//上面代码等价于
var scope="global";
function t(){
    var scope  //在函数头部定义  此时值为undefined
    console.log(scope);   //undefined
    scope="local"  //此时为local
    console.log(scope);  //local
}
t();
```

> JS没有块级作用域 而只有全局作用域和函数作用域  也就是变量在函数体 `始终` 都是由定义的，可以把定义提前得到函数体头

```js
//没有块级作用域的证明
var name="global";
if(true){
    var name="local";
    console.log(name)
}
console.log(name);
//local  local
```

- 自测例子

```js
function t(flag){
    if(flag){
        var s="ifscope";
        for(var i=0;i<2;i++)  //i=2
            ;
    }
    console.log(i);
    console.log(s);
}
t(true);

//输出：2  ifscope
```

```js
function t(flag){
    if(flag){
        s="ifscope";  //没有用var声明的变量（除去函数的参数）都具有全局作用域，成为全局变量，所以声明局部变量必须要用var。
        for(var i=0;i<2;i++) 
            ;
    }
    console.log(i);
}
t(true);
console.log(s);

//输出：2 ifscope
```

```js
 var value = 1;
 function foo() {
   console.log(value);
 }
 function bar() {
   var value = 2;
   foo();
 }
 bar();
// 结果是 1
//js采用词法作用域：执行 foo 函数，先从 foo 函数局部作用域中查找是否有变量 value，如果没有，就从foo()定义时的上一层 即，window中查找，就从全局作用域中查找变量value的值，所以结果会打印 1

//相似例子
 var value = 1;
 function bar() {
   var value = 2;
   function foo() {
   		console.log(value); //foo定义的上一层时bar  value =2
   }
   foo(); //调用
 }
 bar(); //输出 2
```

- 欺骗词法作用域：

JS中有两种机制来实现这个目的。欺骗词法作用域会导致性能下降。

```js
1.eval
eval(..)函数如果接受了一个或多个声明的代码，就会修改其所处的词法作用域
可以对一段包含一个或多个声明的“代码”字符串进行演算，借此来修改已经存在的词法作用域（在进行时）
2.with
而with声明实际上是根据你传递给它的对象平凭空创建了一个全新的词法作用域
本质上是通过将一个对象的引用当作作用域来处理，将对象的属性当作作用域中的标识符来处理，从而创建了一个新的词法作用域
```

缺点：这两个机制的副作用是引擎无法在编译时对作用域查找进行优化，因为引擎只能谨慎的认为这样的优化是无效的。使用这其中的任何一个机制都讲导致代码运行变慢，**不要使用它们。**

- 作用域链

```js
name="lwy";
function t(){
    var name="tlwy";
    function s(){
        var name="slwy";
        console.log(name);
    }
    function ss(){
        console.log(name);
    }
    s();
    ss();
}
t();

//输出： slwy  tlwy
作用域链：
// s()->t()->window
// ss()->t()->window
```

- with

```js
person={name:"yhb",age:22,height:175,wife:{name:"lwy",age:21}};
with(person.wife){
    console.log(name);
}
//with语句将person.wife添加到当前作用域链的头部，所以输出的就是：“lwy"
//with语句结束后，作用域链恢复正常
//with语句主要用来临时扩展作用域链，将语句中的对象添加到作用域的头部,首先执行
```

- 执行期上下文：执行该函数创建一个内部对象，称为 Execution Context（执行期上下文）。执行期上下文定义了一个函数正在执行时的作用域环境。
- 执行期上下文和我们平常说的上下文不同，执行期上下文指的是作用域，平常说的上下文是this的取值指向（call，apply，bind改变上下文）
- 函数定义时的作用域链对象 [[scope]] 是固定的，而 执行期上下文 会根据不同的运行时环境变化，因此对同一函数调用多次，会导致创建多个执行期上下文，一旦函数执行完成，执行期上下文将被销毁。
- 执行期上下文有创建和代码执行的两个阶段。

### 16. DOM常见的操作方式

- 获取DOM

```js
getElementById() 方法匹配元素的id属性来访问元素节点，对元素节点进行操作 
getElementsByTagName() 方法匹配元素的tagName来访问元素节点，对元素节点进行操作 
getElementsByClassName() 方法匹配元素的className来访问元素节点，对元素节点进行操作 
//值得注意的是:
getElementsByTagName() 和 getElementsByClassName()
这两种方法因为其访问的是节点中的可能为复数的属性，所以得到的会是一个以`数组的形式`来体现出来的元素节点集合，我们可以通过打印获取到的DOM节点来判断类型
```

- DOM事件

```js
1.onclick事件—当用户点击时执行
2.onload事件---当用户进入时执行
3.onunload事件---用用户离开时执行
4.onmouseover事件---当用户鼠标指针移入时执行
5.onmouseout事件---当用户鼠标指针移出时执行
6.onmousedown事件---当用户鼠标摁下时执行
7.onmouseup事件---当用户鼠标松开时执行
```

### 17. Array.sort()方法与实现机制

该排序方法每个浏览器中实现的都不太一样

- chrome 目前采用快排(QuickSort)和插入排序(InsertaionSort)

- 火狐，它采用归并排序(MergeSort)。

- IE使用快排。

- V8 引擎 sort 函数只给出了两种排序 InsertionSort 和 QuickSort，数量小于10的数组使用 InsertionSort，比10大的数组则使用 QuickSort。

### 18. Ajax的请求过程

- ajax = 异步 JavaScript 和 XML

- ajax：页面无刷新读取服务器数据：

  > ajax是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术。我们知道，传统的网页（不使用ajax）如果需要更新内容，必须重新加载整个网页。Ajax的出现，使得使网可以实现异步更新，这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。
  > 注意：ajax本身不支持跨域请求，需要在服务器端处理。

- ajax的技术核心是 XMLHttpRequest 对象

  > **get：通过浏览器地址栏传输数据**
  > \- get传输数据小
  > \- 安全性较低
  > \- 有缓存
  > **post：通过http内部传输数据**
  > \- 容量较大，一般可达2G
  > \- 安全性相对较高
  > \- 无缓存

- Ajax请求过程:**创建XMLHttpRequest、连接服务器、发送请求、服务器做出响应、接收响应数据**

```javascript
1.创建一个Ajax对象
//主流浏览器
 if(window.XMLHttpRequest)
 var oAjax = new XMLHttpRequest();
//IE7以下低版本浏览器
 var oAjax = new ActiveXObject('Microsoft.XMLHTTP');

2.连接到服务器
 oAjax.open('get/post,'a.php?t='+new Date().getTime(),true);

3.发送数据
 oAjax.send();

4.接受返回值
onreadystatechange事件通过readyState属性来判断请求状态

readyState：
    0(未初始化)还未调用open方法
    1(载入)已经调用send方法，正在发送请求
    2(载入完成)send发送完成，接受到响应内容
    3(解析)正在解析相应内容
    4(完成)内容解析完成

status属性：200(成功)404(失败)：oAjax.status==200
服务器的返回值：oAjax.responseText
if(oAjax.readyState==4){
    if(oAjax.status==200){
        success(oAjax.responseText);
    }else{
        error(oAjax.status);
    }
}
```

### 19. JS的垃圾回收机制

前言：JavaScript 中的内存管理是**自动执行**的，而且是不可见的。我们创建基本类型、对象、函数……所有这些都需要内存。

可达性：“可达性” 值就是那些以某种方式`可访问`或`可用的值`，它们被保证`存储在内存中`

**1.标记-清除**

- 垃圾回收器获取根并**“标记”**(记住)它们。
- 然后它访问并“标记”所有来自它们的引用。
- 然后它访问标记的对象并标记它们的引用。所有被访问的对象都被记住，以便以后不再访问同一个对象两次。
- 以此类推，直到有未访问的引用(可以从根访问)为止。
- 除标记的对象外，所有对象都被删除。

![preview](https://segmentfault.com/img/bVbqd8A/view)

- 面试：

```js
1.什么是垃圾？
一般来说没有被引用的对象就是垃圾，就是要被清除， 有个例外如果几个对象引用形成一个环，互相引用，但根访问不到它们，这几个对象也是垃圾，也要被清除。
2.如何检垃圾？
常见的有：标记-清除算法
```

[前端面试：谈谈 JS 垃圾回收机制](https://segmentfault.com/a/1190000018605776)

[几种垃圾回收算法](https://www.jianshu.com/p/a8a04fd00c3c)

### 20.  JS中的String、Array和Math方法

- **String:**

```js
1.charAt():返回字符的位置  长度为1，下标从0开始
    var str="fighting 2018!";
    str.charAt(3);//h
2.indexOf()：返回某个字符首次出现位置
    var str="fighting 2018!";
    str.indexOf("t");//4
3.split()：将字符串分割成字符串数组，返回值为该数组
语法：stringObject.split(separator,limit) //limit 可选，分割的次数  separator分割位置
    "2:3:4:5".split(":")    //将返回["2", "3", "4", "5"]
    "|a|b|c".split("|")    //将返回["", "a", "b", "c"]
4.substring()：截取start到stop-1的字符，长度为stop-start
语法：stringObject.substring(start,stop)  //start必须，stop可选
	var str="Hello world!";
    document.write(str.substring(3));//lo world!
    document.write(str.substring(3,8));//lo wo 
5.substr()：start 下标开始的指定数目的字符
语法：stringObject.substr(start,length) // start必须  length截取长度 可选
    var str="Hello world!";
    document.write(str.substr(3));//lo world!
    document.write(str.substr(3,7));//lo worl  //空格也是长度
6.charCodeAt():返回指定位置的字符的 Unicode 编码  0 - 65535
    var str="Hello world!";
    document.write(str.charCodeAt(1));//101
```

- **Math:**

```js
1.ceil()：向上取整 // Math.ceil(x)
2.floor()：向下取整 //Math.floor(x) 
3.round()：四舍五入取整 //Math.round(x)
4.random()：返回介于0~1之间的随机整数 //Math.random()
5.max()：返回较大值 //Math.max(x,y,...)
6.min()：返回较小值 //Math.min(x,y,...)  
//max,min  如果有某个参数为 NaN，或是不能转换成数字的非数字值，则返回 NaN
```

- **Array:**

```js
1.concat()：连接两个或多个数组，返回一个新数组
2.join()：将数组中的所有元素放入字符串，返回一个字符串 //无参默认逗号
    var arr=["I","like","Front-end"];
    var str=arr.join(" ");
    document.write(str);//I like Front-end
3.reverse()：倒置数组
4.slice()：选数组中的元素构成新数组newArr，start到end 左闭又开
5.sort()：排序，要传递回调函数
6.splice()：删除或者插入 参数依次为，开始位置，删除个数，插入元素...
7.map()：对数组中的元素一次执行某个function，返回值为一个处理后的新数组
```

- 我只列举了大部分常用的，少数其他方法请参考[string、array、math方法](https://www.cnblogs.com/lihuijuan/p/8490578.html)

