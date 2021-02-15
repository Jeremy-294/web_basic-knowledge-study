### 1.let const var的概念和区别

- **let & var:**  预解析 / 作用域

  > 没有预解析的过程，即没有变量提升，var有变量提升
  >
  > - 变量提升：可以把变量的定义提前到函数开头
  >
  > 不可以重复声明
  >
  > let 有块级作用域，var是函数作用域   `{}表示块级作用域`

  - 预解析

  ```js
  function sayname() {
          	console.log(name)
          	console.log(age)
          	var name = "lili"
          	let age = "20"
          }
          sayname()
  // undefined
  // Uncaught ReferenceError
  
  //上述代码等价于：
  function sayname() {
      		var name;  //var可以变量提升
          	console.log(name)  //undefined
          	console.log(age)  //还没定义，报错
          	name = "lili"
          	let age = "20"
          }
          sayname()
  ```

  ```js
  console.log(a) 
  var a = 1
  console.log(a)
  function a(){}
  console.log(a)
  
  //上述代码等价于  （函数可以预编译）
  var a; //undefined
  function a(){} //f a(){}
  console.log(a) //输出 f a(){}  因为上面的undefined被函数的预编译覆盖了
  a=1;
  console.log(a) //输出1
  console.log(a) //输出1
  ```

  **总结：**JS的预解析可以大概概括为：

  >1.从代码开始搜索直到结尾，只去查`var function`和参数等内容
  >
  >2.遇到函数与变量重名的，只留`函数`
  >
  >3.遇到函数与函数重名的只留`最后一个函数`。
  >
  >4.所有函数，在正式运行代码之前，都是整个函数块。

  **块级作用域和函数作用域问题：**

  ```js
  function tes(){
      var a = 'var ok'
      let b = 'let ok'
      for (var i = 0; i < 1; i++) {
          var a = 'var change'
          let b = 'let change'
          }
      console.log(a)
      console.log(b)
  }
  tes()
  
  //var change    
  //let ok
  解析：var只有函数作用域的概念，所以在同一个函数中var的值被覆盖了，let只有块级作用域，function和for属于两个不同的块级作用域，因此打印的结果是function的作用域的b，为let ok
  ```

  ```js
  //同一作用域中，即同{}中
  let a=1;
  let a=2;
  //报错
  ```

  **定时器打印数据两者也有不同：**

  1.var 必须要用闭包：运用闭包参数被引用的缓存机制，使得不会被销毁。

  2.let不需要闭包：let定义的i有两个作用域，let i一直被引用，所以不会被销毁。

  ```js
  //var
  for(var i =0;i<5;i++){
      (function(a){
          setTimeout(function(){
          console.log(a)
      },1000)
      })(i)  //i-->a
  }
  ```

  ```js
  //let 
  for(let i =1;i<5;i++){
      setTimeout(function(){
          console.log(i)
      },1000)
  }
  ```

- **const:**

  const声明常量，只可以声明一次，并且必须赋值，可以防止命名冲突，防止之后发生错误的修改。

  ```js
  const g = 1 
  const g=2  // 报错
  g=2 // 报错
  ```

  ```js
  //因为const只是修饰栈空间的地址不可更改，并没有修饰堆空间的值不可修改。
  //常对象的属性 可以修改其值
  const d = {
      name:'allen'
  }
  d.name = 'zhaosi'
  console.log(d)
  ```

### 2.变量提升与暂时性死区

**变量提升**：预解析，即变量可以在声明之前使用，值为undefined。

var存在变量提升，let没有

```js
// var 的情况
console.log(foo);  // 输出 undefined
var foo = 2;

// let 的情况
console.log(bar); // 输出 ReferenceError
let bar = 2;
```

**暂时性死区**：ES6 明确规定，如果区块中存在` let 和 const 命令`，则这个区块对这些命令声明的变量`从一开始就形成封闭作用域`。只要在`声明之前使用这些变量`，就会`报错`。

- 这种语法称为“暂时性死区”（temporal dead zone，简称TDZ）

```js
var tmp = 123;
if (true) {
	// TDZ 开始
	tmp = 'abc';   // ReferenceError
	let tmp;   // TDZ 结束
}
```

**ES6 规定暂时性死区和 let、const 语句不出现变量提升，主要是为了减少运行时出现错误，防止在变量声明之前就使用这个变量，从而导致意料之外的行为。**

### 3.变量的解构赋值

> 1.数组的结构赋值
>
> 2.对象的解构赋值
>
> 3.字符串/数字/布尔型的解构赋值

- 数组的结构赋值

```js
// 基础类型解构
let [a, b, c] = [1, 2, 3]
console.log(a, b, c) // 1, 2, 3

// 对象数组解构
let [a, b, c] = [{name: '1'}, {name: '2'}, {name: '3'}]
console.log(a, b, c) // {name: '1'}, {name: '2'}, {name: '3'}

// ...解构
let [head, ...tail] = [1, 2, 3, 4]
console.log(head, tail) // 1, [2, 3, 4]

// 嵌套解构
let [a, [b], d] = [1, [2, 3], 4]  //只赋值了[2,3]中的2给b
console.log(a, b, d) // 1, 2, 4

// 解构不成功为undefined
let [a, b, c] = [1]
console.log(a, b, c) // 1, undefined, undefined

// 解构默认赋值  前面为默认  后面为解构 数组成员严格等于undefined，默认值才会生效
let [x = 1] = [undefined];// x=1;
let [x = 1] = [null];// x=null; // 数组成员严格等于undefined，默认值才会生效
let [x = 1, y = x] = [];     // x=1; y=1
let [x = 1, y = x] = [2];    // x=2; y=2
let [x = 1, y = x] = [1, 2]; // x=1; y=2
let [x = y, y = 1] = [];   // ReferenceError: y is not defined 因为x用y做默认值时，y还没有声明
```

- 对象解构赋值

```js
// 对象属性解构
let { f1, f2 } = { f1: 'test1', f2: 'test2' }
console.log(f1, f2) // test1, test2

// 可以不按照顺序，这是数组解构和对象解构的区别之一
let { f2, f1 } = { f1: 'test1', f2: 'test2' }
console.log(f1, f2) // test1, test2

// 解构对象重命名  f1重命名为rename
let { f1: rename, f2 } = { f1: 'test1', f2: 'test2' }
console.log(rename, f2) // test1, test2

// 嵌套解构
let { f1: {f11}} = { f1: { f11: 'test11', f12: 'test12' } }
console.log(f11) // test11

// 默认值
let { f1 = 'test1', f2: rename = 'test2' } = { f1: 'current1', f2: 'current2'}
console.log(f1, rename) // current1, current2
```

- 字符串/数字/布尔值

```js
//字符串解构赋值
const [a, b, c, d, e] = 'hello';
a // "h"
b // "e"
c // "l"
d // "l"
e // "o"

//length重命名为len   每个数组对象都有一个length属性，解构赋值给length
let {length : len} = 'hello';
console.log(len) // 5

//解构赋值时，如果等号右边是数值和布尔值，则会先转为对象
let {toString: s} = 123;
s === Number.prototype.toString // true   

let {toString: s} = true;
s === Boolean.prototype.toString // true
```

`解构赋值的规则是，只要等号右边的值不是对象或数组，就先将其转为对象。由于undefined和null无法转为对象，所以对它们进行解构赋值，都会报错。`

```js
let { prop: x } = undefined; // TypeError
let { prop: y } = null; // TypeError
```

- 解构赋值的使用

```js
//变量交换
let x = 1;
let y = 2;
[x, y] = [y, x];

//map数据结构的遍历
var map = new Map();
map.set('first', 'hello');
map.set('second', 'world');

for (let [key, value] of map) {  //解构赋值
    console.log(key + " is " + value);
}
```

### 4.箭头函数及其this问题

```js
//如果函数只有一个参数,可以不写()
const fun = function(e){}     //普通函数
const fun = e => {}           //箭头函数
```

```js
<div>我是div</div>
<script>
    const oDiv = document.querySelector('div');
    // 只有一个参数，可以不写(),直接定义一个参数
    oDiv.addEventListener('click' , e=>{
        console.log(e);
    });
</script>
```

```js
//如果执行体中只有一行代码,可以不写{}
const fun = e=>{console.log(e)}    //普通箭头函数
const fun = e=> console.log(e)     //不写{}箭头函数
```

```js
<div>我是div</div>
<script>
    // 只有一行代码,不写{}
    oDiv.addEventListener('click' , e=>console.log(e) )
</script>
```

- this 指向

>在箭头函数中，this指向要看父级程序的this指向
>如果父级程序有this指向，那么箭头函数指向的就是父级程序的this
>如果父级程序没有this指向，那么指向的就是window

```js
// 绑定的事件处理函数 --- 指向的是绑定事件处理函数的标签
//普通函数
const oDiv = document.querySelector('div');
oDiv.onclick = function(){
    console.log(this);
};
oDiv.addEventListener('click' , function(){
    console.log(this);
});

//箭头函数
const oDiv = document.querySelector('div');
// 当前的程序,，箭头函数的父级程序，没有
// this指向的就是window
oDiv.addEventListener('click' , ()=>{
    console.log(this);
});
```

```js
<div>1234</div>
<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
</ul>
<script>
    const oDiv = document.querySelector('div');
    // 当前的程序,，箭头函数的父级程序，没有
    // this指向的就是window
    oDiv.addEventListener('click' , ()=>{
        console.log(this);
    });
    
    // 对li进行操作
    const oLis = document.querySelectorAll('li');
    oLis.forEach(function(item,key){
        console.log(this);  // 输出的是forEach的函数的this指向
        // 箭头函数的this,是父级程序,forEach()的this,是window
        item.addEventListener('click' , ()=>{
            console.log(key,this);
        })
    });
    
    // forEach()中 函数的this指向,就是window
    const arr = [1,2,3,4,5,6];
    arr.forEach(function(){
        console.log(this);
    });
    
    const obj = {
        // 普通函数,this指向对象
        fun1 : function(){console.log(this)},
        // 箭头函数this指向是,父级程序
        // 父级程序是对象
        // 只有函数有this,obj对象没有this
        // 父级程序没有this,指向的是window
        fun2 : ()=>{console.log(this)},
    
        // fun3是一个普通函数,this指向的是obj对象
        fun3 : function(){
            // fun4,是一个箭头函数,this指向的是父级程序的this指向
            // 父级程序是fun3,fun3的this是对象,fun4箭头函数的this也是对象
            const fun4 = ()=>{console.log(this)};
            fun4();
        }
    };
    obj.fun1();
    obj.fun2();
    obj.fun3();
</script>
```

```js
1，普通的function函数
 		声明式 --- window
 		赋值式 --- window
 		forEach循环 --- window
 		定时器，延时器 --- window
 		对象中的函数 --- 对象本身
 		事件绑定事件处理函数 --- 绑定事件的标签

2，箭头函数的this指向
 		父级程序的this指向`(如果是对象调用箭头函数依旧指向window 具体实例看fun2)`
 		如果父级程序有this指向(父级程序也是函数)，this指向的就是父级程序的this指向
 		如果父级程序没有this指向(数组,对象....)，this指向的是window
 		
注意：`箭头函数，不能改变this指向，只有普通function函数，能改变this指向。`
```

### 5.Symbol概念及其作用

1.概念：Symbol是ES6中新增的一种数据类型, 被划分到了基本数据类型中  

- 基本数据类型: 字符串、数值、布尔、undefined、null、Symbol    引用数据类型: Object

2.Symbol的作用 ：用来表示一个独一无二的值

3.使用:

```js
let name = Symbol("name");
let say = Symbol("say");
let obj = {
    // 注意点: 如果想使用变量作为对象属性的名称, 那么必须加上[]
    [name]: "lnj",
    [say]: function () {
        console.log("say");
    }
}
// obj.name = "it666";
obj[Symbol("name")] = "it666";
console.log(obj);
```

### 6.Set和Map数据结构

### 7.Proxy



### 8. Reflect对象

### 9.Promise（API ，方法及手撕）

### 10.Iterator 和 for...of（Iterator遍历器的实现）

### 11.循环语法比较及使用场景（for/for..of/forEach/for..in）

### 12.Generator及其异步方面的应用

### 13.async函数

### 14.几种异步方式的比较（setTimeout/Promise/Generator/async）

### 15.class基本语法及继承

### 16.模块加载方案及比较（commonJS和ES6的Module）

### 17.ES6模块加载与CommonJS加载的原理