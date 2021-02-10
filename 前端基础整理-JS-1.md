### 1.介绍 js 的基本数据类型

js 一共有六种基本数据类型：

> 5种简单类型分别是 Undefined、Null、Boolean、Number、String      （巧记 undefined / null，NSB）
>
> 1种复杂数据类型 object（对象）

`ES6` 新增的 symbol 类型：

>Symbol 代表创建后独一无二且不可变的数据类型，它的出现我认为主要是为了解决可能出现的全局变量冲突 的问题。

`ES10` 新增的BigInt 类型：

>BigInt 是一种数字类型的数据，它可以表示任意精度格式的整数，使用 BigInt 可以安全地存储和操作大 整数，即使这个数已经超出了` Number` 能够表示的安全整数范围。

### 2.原始值和引用值

**原始值 **即一些代表原始数据类型的值，也叫基本数据类型，首先说一下js中有哪些原始值，Number，String，Boolean，Null，Undefined这些基本数据类型都是原始值。

原始值存储在**栈**中。

```js
var a = 10 ;
var b = a ;
a = 20;
console.log(b);     //输出10 b的值不会因a的值的改变而该改变
```

**引用值** 是指一些复合类型数据的值，包括Object，function，Array，RegExp，Data，引用值于原始值不同。

引用值由指针（引用变量）指向对象，指针放在栈中，但是对象在堆内存里。

```js
var a = [1,2,3] ;  
var b = a ; 
a.push(4) ;
console.log(b) ;     //输出1,2,3,4 
a = [12] ;
console.log(b) ;  //输出1,2,3,4 
console.log(a) ;  //输出12
```

> 上例中a指向[12]，指向了新的堆空间。

### 3. JS中数据类型的4种判断方法

> **typeof 、 instanceof  、  Object.prototype.toString.call()  、constructor（构造函数）**

**1.typeof **
返回值包括number、boolean、string、symbol、object、undefined、function等7种数据类型
但不能判断`null`、`array`等

```js
typeof Symbol(); // symbol 有效
typeof ''; // string 有效
typeof 1; // number 有效
typeof true; //boolean 有效
typeof undefined; //undefined 有效
typeof new Function(); // function 有效
typeof null; //object 无效
typeof [] ; //object 无效
typeof new Date(); //object 无效
typeof new RegExp(); //object 无效
```

**2.instanceof**

用来判断A是否为B的实例，`A instanceof B`， 返回 `boolean `值。

但**它不能检测 null 和 undefined**

```js
[] instanceof Array; //true
{} instanceof Object;//true
new Date() instanceof Date;//true
new RegExp() instanceof RegExp//true
null instanceof Null//报错
undefined instanceof undefined//报错 
```

**3.Object.prototype.toString.call()**

一般数据类型都能够判断，**最准确最常用**的一种。

> 补充：
>
> Object.prototype.toString.call来获取，而不能直接 new Date().toString(), 从原型链的角度讲，所有对象的原型链最终都指向了Object, 按照JS变量查找规则，其他对象应该也可以直接访问到Object的toString方法，而事实上，大部分的对象都实现了自身的toString方法，这样就可能会导致Object的toString被终止查找，
>
> `因此要用call来强制执行Object的toString方法。` 

```js
Object.prototype.toString.call('') ;   // [object String]
Object.prototype.toString.call(1) ;    // [object Number]
Object.prototype.toString.call(true) ; // [object Boolean]
Object.prototype.toString.call(undefined) ; // [object Undefined]
Object.prototype.toString.call(null) ; // [object Null]
Object.prototype.toString.call(new Function()) ; // [object Function]
Object.prototype.toString.call(new Date()) ; // [object Date]
Object.prototype.toString.call([]) ; // [object Array]
Object.prototype.toString.call(new RegExp()) ; // [object RegExp]
Object.prototype.toString.call(new Error()) ; // [object Error]
```

**4.Constructor**

```js
//constructor这个属性存在于构造函数的原型上，指向构造函数
//对象可以通过__proto__在其所属类的原型上找到这个属性
 
var arr = [];
arr.constructor === Array;//true
arr.constructor === Object;//false
//arr通过原型链查找到的constructor指向了Array，所以Object判断是错误的
 
constructor是比较准确判断对象的所属类型的，但是在类继承时会出错
function Fn() {}
Fn.prototype = new Array();
var fn = new Fn();
fn.constructor === Array; //true
//fn是一个对象，但是它的构造函数指向Array是true
 
function A(){};
function B(){};
A.prototype = new B(); //A继承自B
var aObj = new A();
aobj.constructor === B //true;
aobj.constructor === A // false;
 
对象直接继承和间接继承的都会报true
aobj instanceof B //true;
aobj instanceof B //true;
 
解决construtor的问题通常是让对象的constructor手动指向自己：
aobj.constructor = A; //将自己的类赋值给对象的constructor属性
aobj.constructor === A //true;
aobj.constructor === B //false; //基类不会报true了;
```

### 4.类数组与数组的区别与转换

**类数组：**

1. 拥有length属性，其它属性（索引）为非负整数（对象中的索引会被当做字符串来处理）
2. 不具有数组所具有的方法
3. 是一些元素的集合（普通对象），相当于是个伪数组，数组是Array型

```js
在JavaScript中常见的类数组有：内置对象arguments、DOM方法返回的结果
eg:
document.querySelectorAll('li')，document.getElementsByTagName('div')
```

**类数组转换数组：**

方法1.`Array.prototype.slice.call(arrayLike)`的结果是将arrayLike对象转换成Array对象，`Array.prototype.slice`表示数组的原型中的slice方法。slice方法返回的是一个Array类型的对象。

```js
//方法实现
Array.prototype.slice = function(start,end){ 
var result = new Array(); 
start = start || 0; 
end = end || this.length; //使用call后，改变了this的指向，也就是指向传进来的对象
for(var i = start; i < end; i++){ 
       result.push(this[i]); 
     } 
     return result; 
}
//调用过程
let oDivs = document.getElementsByTagName('div');
oDivs = Array.prototype.slice.call(oDivs);
```

方法2：使用栈开运算符 	`...src`

```js
//例子
let ary1 = [1, 2, 3];
let ary2 = [4, 5, 6];
let ary3 = [...ary1, ...ary2];
//ary1.push(...ary2);
console.log(...ary1);//1, 2, 3
console.log(...ary2);//4, 5, 6
console.log(ary3);//[1,2,3,4,5,6]
//类数组转换
let oDivs = document.getElementsByTagName('div');
oDivs = [...oDivs];
```

方法3：使用ES6里面的Array.from()方法

```js
//例子
let a = Array.from('一二三四五六七') // ["一", "二", "三", "四", "五", "六", "七"] 
let b = Array.from(new Set([1, 2, 1, 2])) // [1,2] 数组去重
let c = Array.from(new Map([[1, 2], [2, 4], [4, 8]])) // 接受一个map
//类数组转换
let d = Array.from(arguments)// 接受一个类数组对象
let e = Array.from(document.querySelectorAll('li')) //接受DOM返回的结果
```

### 5. 数组常见的API

分为以下三大类：

**数组操作方法**（push、pop、shift、unshift、reverse（倒置）、sort、toString、join、concat、slice（数组截取）、splice（删除 / `插入`））

**数组位置方法**(indexOf、lastIndexOf)

**数组迭代方法**（forEach、map、filter、some、every）

详细用法如下：（简单的已经跳过，从sort开始，选择性的讲）

**sort()**

```js
//传递一个回调函数 代表从小到大
var arr=[1,22,33,55,66,77,4];
arr.sort(function(a,b){
    return a-b;      //升序，如果降序则return b-a;
});
console.log(arr);//[1, 4, 22, 33, 55, 66, 77]

arr.sort((a,b) => a-b)//箭头函数写法
```

**toString()**

```js
//返回值以',' 分隔
var color = ["red", "green", "blue"];
console.log(color.toString()); //red,green,blue
```

**join**

啥都没有默认加','分隔

```js
var color = ["red", "green", "blue"];
var color1 = color.join(); //默认,
console.log(color1);//red,green,blue
var color2 = color.join('|');
console.log(color2);//red|green|blue
```

**slice 数组截取** 默认`左闭右开`

```js
var color = ["red", "green", "blue"];
var color1 = color.slice(1,2); //默认左闭右开
console.log(color1);//["green"]
```

**splice() 可以用于数组删除 / 插入操作**

```js
//删除
//参数为：起点 和 删除 的个数
var color = ["red", "green", "blue"];
color.splice(2,1);
console.log(color);//["red", "green"]
```

```js
//插入
//参数为：起点  删除0项  插入的新项
var color = ["red", "green", "blue"];
color.splice(2,0,"red","green","pink");
console.log(color);//["red", "green", "red", "green", "pink", "blue"]
```

```js
//替换 （本质为删除某些项然后再添加新的项）
//参数为：起点 删除数量 新插入的项
var color = ["red", "green", "blue"];
color.splice(2,1,"red","green");
console.log(color);//["red", "green", "red", "green"]
```

**indexOf 和 lastIndexOf 一般用于数组去重**

```js
var arr = [1, 1, 1, 2, 2, 2, 3, 5, 6];
var newArr = [];
for (var i = 0; i < arr.length; i++) {
    if (newArr.indexOf(arr[i]) === -1) { //该元素在新数组中不存在(index===-1)则放进去 
        newArr.push(arr[i]);//相当于 newArr[newArr.length]=arr[i];
    }
}
console.log(newArr);//[1, 2, 3, 5, 6]
```

**map(callback)**

map(callback)方法对数组中的每一项运行给定函数，`返回每次函数调用的结果组成的数组`，返回一个新数组；

```js
var arr = [10, 20, 30, 4];
var newArr = arr.map((value) => value > 4);
console.log(newArr);//[true, true, true, false]是给定函数返回的结果组成一个数组
 
var newArr1 = arr.map((value) =>value * 2);
console.log(newArr1);//[20, 40, 60, 8]
```

**filter(call back)**

对数组中的每个元素运行给定函数，`返回该函数会返回true 的项组成的数组`，相当于查找满足条件的元素，而且把`满足条件的元素组成一个新数组返回`

```js
var arr = [10, 20, 30, 4];
var newArr = arr.filter((value) => value > 4);
console.log(newArr);//[10, 20, 30]
```

**some(callback)**

返回布尔值，有一个为`true`就结束循环

```js
var arr = [10, 20, 30, 4];
console.log(arr.some((value) => value == 20));//true
```

**every(callback)**

返回布尔值，全部为`true`返回`true`；有一个为`false`就返回`false`

```js
var arr = [10, 20, 30, 4];
var flag = arr.every((value) => value >= 4);
console.log(flag);//true
```

### 6. call 、apply 、bind 方法的应用与区别

**前言**：每个**函数**都有call、apply、bind方法，通俗点说就是function foo()这样形式就能使用foo.call()/apply()/bind()方法。

**返回值与参数表区别**

* **返回值**：call 和 apply 都是返回undefined；bind返回的是一个函数
* **参数表**：
  * call 参数表为（this,a2,a3....），以逗号分隔开；
  * apply 参数表为（this,[a1,a2,a3....]），第一个参数为this，第二个参数为一个数组，把函数参数放在数组里头
  * bind 参数表为（this,a2,a3....），以逗号分隔开（和call方法一样）

**用法**

```js
//定义一个全局变量obj、foo函数，此时foo函数中的this指向window，要访问obj中的属性，需要先访问window，再从window中取值this.obj.name

var obj = {
   name:'hehe',
   age:23
}

function foo(){
   console.log(this.obj.name)    // 'hehe'
}
```

```js
//上段代码中this指向Window，下段指向obj
var obj = {
 name: 'hehe',
 age: '23',
 foo: function () {
   console.log(this.name)    // 'hehe'
 }
}
obj.foo()
```

#### call

```js
//使用call可以改变执行上下文, foo中的this指向obj而不是Window了

var obj = {
    name:'hehe',
    age:23
}

function foo(){
    console.log(this.name)    // 'hehe'
}

foo.call(obj)
```

```js
//同理原先指向obj的写法，使用call后，可以使this指向其他变量。

var o = {
  name: 'aaaaaaa'
}
var obj = {
  name: 'hehe',
  age: '23',
  foo: function () {
    console.log(this.name)    // 'aaaaaaa'
  }
}
obj.foo.call(o)  //指向o这个对象 call可以函数改变上下文
```

- 对于call第一个参数之后的参数，即为**函数可以使用的参数**，例如：

```js
var o = {
  name: 'aaaaaaa'
}
var obj = {
  name: 'hehe',
  age: '23',
  foo: function (n, m) {
    console.log(this, n, m)    // { name: 'aaaaaaa' } { a: '4' } '6'
  }
}
obj.foo.call(o, { a: '4' }, '6')  // { a: '4' } '6'  函数可以使用的参数
```

#### apply

- apply改变this的使用和call**一毛一样**，只是**后面的参数为数组**：
- call 和 apply 本质上没有区别 只是参数表不同

```js
var o = {
  name: 'aaaaaaa'
}
var obj = {
  name: 'hehe',
  age: '23',
  foo: function (n, m) {
    console.log(this, n, m)    // 同样分别获得值 { name: 'aaaaaaa' } { a: '4' } '6'
  }
}
obj.foo.apply(o, [{ a: '4' }, '6']) //{ a: '4' } '6'将数组的内容解析作为函数参数
```

> call的应用，例如`Object.prototype.toString.call()`、`Array.prototype.slice.call() `    //apply

```js
//Object.prototype.toString.call()
//所有的对象都继承自Object，Object.prototype 指向 Object.prototype原型对象，

1.使用`arr.toString()`时,不能进行复杂数据类型的判断，因为它调用的是`Array.prototype.toString`

2.而使用`toString.call(arr)`实际上是通过原型链调用了`Object.prototype.toString`
```

```js
//Array.prototype.slice.call(arguments,1)  
//call()方法的第二个参数表示传递给slice的参数即截取数组的起始位置。

function list() {
  return Array.prototype.slice.call(arguments,1); //apply(arguments,[1])
}
var list1 = list(1, 2, 3); // [2, 3]
```

#### bind

- bind返回的是个**函数**，但**不会立即执行**，所以我加了个()执行它。

```js
var o = {
  name: 'aaaaaaa'
}
var obj = {
  name: 'hehe',
  age: '23',
  foo: function (n, m) {
    console.log(this, n, m)   // { name: 'aaaaaaa' } { a: '4' } '6'
  }
}
obj.foo.bind(o, { a: '4' }, '6')()   // 这里执行了bind返回的函数 foo
```

- bind 的应用：

```js
//例如在react中写了一个方法，调用这个方法之前需要将这个方法的this指向绑定到constructor上下文。例如：

constructor(props){
    super(props)
    this.foo = this.foo.bind(this)
}
```

这样在jsx中调用foo方法this指向组件实例。 **不使用bind改变this指向，this的指向就会丢失**。例如：

```js
var obj = {
  name: 'hehe',
  age: '23',
  foo: function () {
    console.log(this)   // { name: 'hehe', age: '23', foo: [Function: foo] }
  }
}
obj.foo()
var Fo = obj.foo
Fo()    // window 这里指向了 window  this指向最终调用它的执行环境
```

**总结bind**：在react中使用bind**改变上下文执行环境后**，由于react是**虚拟DOM结构**，所以在div标签中使用**onClick事件**，this指向的不是DOM**而是组件**。

### 7. new 的原理

```js
//要自己实现一个new功能，实现要知道new操作符都干了些什么，其实总的来说就四步：
//1.创建一个空对象
//2.链接到原型
//3.绑定this值
//4.返回新对象
//知道了这些步骤，我们就可以自己模拟实现new方法了

function _new () {
    var obj = new Object();  //1.创建一个空对象
    var [func, ...args] = [...arguments]; //把参数解构赋值 第一个参数是Animal函数赋值给func
    obj.__proto__ = func.prototype; //2.链接到原型，func原型Animal函数
    var result = func.call(obj,...args); //3.用call函数，绑定this值
    if(result && ( typeof(result) === 'object' ||  typeof(result) === 'Function')){
        return result; //4.返回新对象
    }
    return obj;   //4.返回新对象，空对象
}
function Animal(name,age){
    this.name = name;
    this.age = age;
}
//通过new创建构造实例
let dog1 = new Animal('dog1',2);
console.log(dog1.name) // dog1
console.log(dog1.age)//2

//通过_new方法创造实例
let dog2 = _new(Animal,'dog2',3);  //传递Animal函数 以及相应的call 参数
console.log(dog2.name)//dog2
console.log(dog2.age) //3
```

### 8. 如何正确判断this?

> 总结了一下 常见的有以下几种类型

- 直接调用 func()
- 创建对象 const c =new func()
- 对象调用 obj.func()   （也称为隐式绑定）
- call bind apply 调用 func.call()
- => 箭头函数

```js
function foo() {
  console.log(this.a)
}
var a = 1
foo()  //直接调用，this指向window

const obj = {
  a: 2,
  foo: foo
}
obj.foo()  //this指向调用的对象 obj

const c = new foo()   //this指向new的对象 c
```

**call  bind  apply 已经了解过了 会指向第一个参数作为this，但有以下几个值得注意的点**

- 如果第一个参数为空，则是 window

- call  bind  apply 传递 null / undefined结果如下

```js
function info(){
    //node环境下，非严格模式 global，严格模式null
    //浏览器环境下，非严格模式 window，严格模式null
    console.log(this);
    console.log(this.age);
}
var person = {
    age:20,
    info
}
var age= 28;
var info = person.info;
//严格模式抛出错误
//非严格模式，node下输出undefined（因为全局的age不会挂在global上）
//非严格模式，浏览器下输出28（因为全局的age挂在window上）
info.call(null);
```

- 多个bind()的情况 ：指向第一个bind() 的this，所以下面的代码指向 window

```js
let a = {}
let fn = function () { console.log(this) }
fn.bind().bind(a)() // => ?  //指向window
```

**箭头函数**     (建议两个例子都看懂) 

```js
function a() {
  return () => {
    return () => {
      console.log(this)
    }
  }
}
console.log(a()()())

//箭头函数中的 this 只取决包裹箭头函数的第一个普通函数的 this。
//在这个例子中，因为包裹箭头函数的第一个普通函数是 a，所以此时的 this 是 window。
//另外对箭头函数使用 bind 这类函数是无效的。
```

```js
var obj = {
    birth: 1990,
    getAge: function () {
        var b = this.birth; // 1990
        var fn = () => new Date().getFullYear() - this.birth; // this指向obj对象
        return fn();
    }
};
obj.getAge(); // 25
//由于obj调用getAge函数，所以getAge指向obj，在箭头函数fn中，第一个包含的普通函数为getAge，所以箭头函数fn的this指向getAge的this，也就是obj
//故可以通过this.birth直接调用对象的属性
```

### 9. 闭包及其作用

- **引例**：先看下面这段代码

```js
<ul>
    <li>0</li>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
</ul>
<script>
	var liList=document.getElementsByTagName("li");
	for(var i=0;i<liList.length;i++){
    	liList[i].onclick=function(){
        	console.log(i);
    	}
	}
</script>

//输出的结果为5 5 5 5 5
//因为在执行点击操作时，for循环已经进行完毕且i值为"5"
```

```js
//修改后的代码如下
<ul>
    <li>0</li>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
</ul>
<script>
    var liList=document.getElementsByTagName("li");
	for(var i=0;i<liList.length;i++){
    	liList[i].onclick=(function(j){  
        	return function(){   //无需参数
            	console.log(j); //会打印出上层函数的i
            }
   	 })(i);  //匿名函数后面加个()并且传递参数才能执行他
	}
</script>
```

- **闭包的概念**
  - 函数嵌套函数
  - 内层函数访问调用外层函数的变量或参数（将参数或变量长期保存在内存当中）

- **闭包的作用** （闭包通常直接返回函数）

1. 实现公有变量（函数累加器）

```js
//用闭包实现计数器的好处就是可以无限创建新的计数器
function add(){
    var num = 0;
    function a(){
        console.log( ++ num);
    }
    return a;
}
var myadd1 = add();
myadd1();//结果为1
myadd1();//结果为2
myadd1();//结果为3

var myadd2 = add();
myadd2();//结果为1
myadd2();//结果为2
myadd2();//结果为3
```

2. 可以做缓存（存储结构）  
3. 可以实现封装，属性私有化（私有化变量）  

```js
function Deng(name,wife){
    var prepareWife = "xiaozhang";
    this.name = name;
    this.wife = wife;
    this.divorce = function(){
        this.wife = prepareWife;
    }
    this.changePrepareWife = function(target){ //函数嵌套函数实现封装
        prepareWife = target;
    }
    this.sayPrepareWife = function(){
        console.log(prepareWife);
    }
}
var deng = new Deng('deng','xiaoliu');
```

- **其他例子**（setTimeout，其实都一样）

```js
//修改前
for (var i = 0; i < 4; i++) {
  setTimeout(function() {
    console.log(i); 
  }, 300);
}
//修改后
for (var im = 0; im < 4; im++) {
    setTimeout(
        (function(i) {
            return function() {
                console.log(i)
            }
    })(im), 300);
}
```

- **闭包的缺陷：**闭包会导致内存占用过高,因为变量都没有释放内存

### 10. 原型和原型链

- 原型的概念：每当定义一个函数数据类型(`Object`、`Function`、`Arrry`、`Date`等)的时候都会自带一个prototype对象，`prototype`对象就是我们说的`原型`。
- 原型的作用：实现 **数据的共享**

```js
function Person(){
	this.showName=function(){
		console.log("zhangsan")
	}
}
let person1=new Person();
let person2=new Person();
console.log(person1.showName===person2.showName) //false

function Person(name){
	this.name=name;
}
Person.prototype.showName=function(){  
	console.log(this.name)
}
let person1=new Person("zhangsan");
let person2=new Person("lisi");
console.log(person1.showName===person2.showName) //true
// 因为两个实例对象共享同一个方法
```

- 原型链的概念：通过__proto__属性链接起来的一条路线，就叫做原型链(蓝色部分)

```js
原型对象也是一个对象，具有__proto__属性。所以它也有自己的构造函数（本图是Object构造器/构造函数），原型对象的__proto__属性指向自己的构造函数的原型对象（Object.prototype）；而Object.prototype的原型对象是null。
//这样通过__proto__属性链接起来的一条路线，就叫做原型链。
```

<img src="https://img-blog.csdnimg.cn/20210126180053647.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMjMyNTcz,size_16,color_FFFFFF,t_70" alt="img" style="zoom: 50%;" />

- 实例对象可以使用构造函数prototype原型上的属性和方法，由于有\__proto__的存在：

```js
function Person() {}
var person = new Person();
console.log(person.__proto__ === Person.prototype); //true
```

- 基于原型链的查找方式

```js
function Person(){
	this.name="zhangsan";
}
Person.prototype.name="lisi"
Object.prototype.name="wangwu"
let person1=new Person();
console.log(person1.name)  //zhangsan

//首先person1对象会去本身去找，看自己有没有name属性，如果有则查找结束
//如果本身没有就会沿着原型链去原型上找，找到了就结束
//如果原型上还是没有，就会去Object原型对象上去查找
//如果还没有则会报错
```

- 总结：原型是定义函数时产生的prototype对象，原型链是实例对象到Object.prototype用\__proto__连接起来的一条线路，以及需要掌握原型链上的查找方式。