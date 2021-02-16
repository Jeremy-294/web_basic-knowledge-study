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

- **set：**Set是一个构造函数,用来生成Set数据结构,它类似于数组,但是`成员的值都是唯一的,没有重复的`
- 初始化Set可以接受一个`数组`或`类数组对象`

```js
let s3 = new Set([1, 2, 2, 3, 3, 4])
console.log(s3);//{1, 2, 3, 4}
```

- 属性方法：

  - size:返回成员个数 相当于数组的length,注意没有括号

  ```js
  let s3 = new Set([1, 2, 2, 3, 3, 4])
  console.log(s3.size);//去重之后,成员总数为4
  ```

  - add()：添加某个值,返回Set结构本身(可以链式调用)

  ```js
  let s3 = new Set([1, 2, 2, 3, 3, 4])
  console.log(s3.add([6,7]))//Set(5) {1, 2, 3, 4, Array(2)}
  console.log(s3.add(5));//Set(5) {1, 2, 3, 4, 5}
  ```

  - delete():删除某个值,返回一个bool值,表示是否删除成功

  ```js
  let s3 = new Set([1, 2, 2, 3, 3, 4])
  console.log(s3.delete(4));//true
  ```

  - has():返回一个bool值,表示该值是否为Set的成员

  ```js
  let s3 = new Set([1, 2, 2, 3, 3, 4])
  console.log(s3.has(5));//false
  ```

  - clear():清除所有成员,没有返回值

  ```js
  let s3 = new Set([1, 2, 2, 3, 3, 4])
  s3.clear();
  console.log(s3);//Set(0) {}
  ```

  - set 不可通过下标访问，修改和排序

  ```js
  var array = [1, 2, 'a', 3, 'b'];
  let s1 = new Set(array);
  console.log(s1[0]);//undefined
  console.log(s1['a']);//undefined
  ```

  - set的取值方法：

    1.使用扩展运算符转换为数组

  ```js
  var array = [1, 2, 'a', 3, 'b'];
  let s1 = new Set(array);
  var [a, b, c] = [...s1];
  console.log(a);//1
  console.log(b);//2
  console.log(c);//a
  
  var array=[...s1];
  console.log(array);//[1, 2, "a", 3, "b"]
  ```

  ​		2.for ..of 遍历

  ```js
  var array = [1, 2, 'a', 3, 'b'];
  let s1 = new Set(array);
  for (const val of s1) {
      console.log(val);
  }
  ```

  - keys() / values():由于 `Set` 结构没有键名，只有键值（或者说键名和键值是同一个值），所以 `keys` 方法和 `values` 方法的行为完全一致。
  - entries()：返回键值对

  ```js
  var array = [1, 2, 'a', 3, 'b'];
  let s1 = new Set(array);
  for (const val of s1.keys()) {   //s1.values()一样的效果
      console.log(val);
  }
  ```

  - set转换为数组:Array.from方法

  ```js
  const items = new Set([1, 2, 3, 4, 5])
  const array = Array.from(items)
  ```

- weakset：WeakSet结构与Set类似,也是不重复的值的集合，WeakSet的成员只能是对象(任何具有Iterable接口的对象),不能是其他类型的值，`WeakSet`不可迭代,因此不能被用在`for...of`等循环中，WeakSet`没有size`属性。
- Map:初始化 Map 需要一个二维数组，或者直接初始化一个空的 Map

```js
const m = new Map();
const m1 = new Map([["name", "liuqiao"], ["age", 27]]);
console.log(m);//Map(0) {}
console.log(m1);//Map(2) {"name" => "liuqiao", "age" => 27}
```

- size:返回成员个数

```js
const m1 = new Map([["name", "liuqiao"], ["age", 27]]);
console.log(m1.size);//2
```

- set(key,value):设置键值对

```js
const m1 = new Map([["name", "liuqiao"], ["age", 27]]);
m1.set("sex","男")
console.log(m1);//Map(3) {"name" => "liuqiao", "age" => 27, "sex" => "男"}
```

- get(key):获取键对应的值

```js
const m1 = new Map([["name", "liuqiao"], ["age", 27]]);
console.log(m1.get("name"));//liuqiao
```

- has(key):是否存在某个键

```js
const m1 = new Map([["name", "liuqiao"], ["age", 27]]);
console.log(m1.has("name"));//true
```

- delete(key):删除某个键值对,返回一个布尔值,表示是否删除成功

```js
const m1 = new Map([["name", "liuqiao"], ["age", 27]]);
console.log(m1.delete("age"));//true
console.log(m1);// {"name" => "liuqiao"}
```

- clear():将Map中的所有元素全部删除

```js
const m1 = new Map([["name", "liuqiao"], ["age", 27]]);
m1.clear();
console.log(m1);//Map(0) {}
```

- keys() / values() /entries / forEach / for ...of和set差不多，不写了

- Map转换为数组：

  ```js
  const map=new Map();
  const arr=[...map];
  ```

- 数组转换为Map：

  ```js
  let arr=[];
  let map=new Map(arr);
  ```

- WeakMap结构与Map结构类似,也是用于生成键值对的集合,只接受对象作为键名。

### 7.Proxy

定义：代理对象是用于定义基本操作的自定义行为，换句话说，我们可以说代理对象是我们的目标对象的包装器，我们可以在其中操纵其属性并阻止对它的直接访问。（如果感到难以理解，可以直接看代码）

- handler：限制操作的函数
- target：（代理虚拟化的对象）即我们操作的对象(可以是数组，对象，函数等等)

- 语法：

```js
let p = new Proxy(target, handler); //新建一个代理对象p 传入操作对象target，和处理函数handler
```

- 以下一个例子讲解有无代理的区别：`对一个对象操作，如果属性存在，我们想要打印用户信息，如果不存在，则抛出异常`

```js
let user = {
    name: 'John',
    surname: 'Doe'
};
 
//在输出函数中相应的进行判断，这是没有代理的情况
let printUser = (property) => {
    let value = user[property];
    if (!value) {
        throw new Error(`The property [${property}] does not exist`);
    } else {
        console.log(`The user ${property} is ${value}`);
    }
}
 
printUser('name'); // 输出: 'The user name is John'
printUser('email'); // 抛出错误: The property [email] does not exist
```

- 用代理的`get`方法

```js
let user = {
    name: 'John',
    surname: 'Doe'
};
 
//用代理对象来处理我们的限制操作
let proxy = new Proxy(user, {  //target为user， handler为get函数
    get(target, property) {   //get方法
        let value = target[property];
        if (!value) {
            throw new Error(`The property [${property}] does not exist`);
        }
        return value;
    }
});
 
let printUser = (property) => {
    console.log(`The user ${property} is ${proxy[property]}`);
};
 
printUser('name'); // 输出： 'The user name is John'
printUser('email'); // 抛出错误: The property [email] does not exist
//通过查看上面的代码，你会发现：将条件和异常移到其他地方，而printUser中仅关注显示用户信息的实际逻辑会更好。
```

- 代理的`set`函数

```js
//代理可能有用的另一个例子是属性值验证,我们需要使用 set 方法，并在其中进行验证
let user = new Proxy({}, {
    set(target, property, value) {
        if (property === 'name' && Object.prototype.toString.call(value) !== '[object String]') { // 确保是 string 类型
            throw new Error(`The value for [${property}] must be a string`);
        };
        target[property] = value;
    }
});
 
user.name = 1; // 抛出错误: The value for [name] must be a string
//对属性值验证，如果赋值的不是string类型，就会报错
```

- `proxy`还有以下功能:

  > - 格式化
  > - 价值和类型修正
  > - 数据绑定
  > - 调试
  >
  > 你可以根据受控规则扩展或拒绝对原始数据的访问，从而监视对象并确保正确行为

### 8. Reflect对象

[建议看此链接](https://blog.csdn.net/qq_35036255/article/details/80942572?ops_request_misc=&request_id=&biz_id=102&utm_term=Reflect%E5%AF%B9%E8%B1%A1&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-2-80942572.pc_search_result_no_baidu_js)

### 9.Promise（API ，方法及手撕）

1. 概述：Promise是异步编程的一种解决方案，从语法上讲，Promise是一个对象，可以获取异步操作的消息
2. 目的： （1）避免回调地狱的问题（2）Promise对象提供了简洁的API，使得控制异步操作更加容易
3. Promise有三种状态：pendding //正在请求，rejected //失败，resolved //成功
4. 基础用法：new Promise(function(resolve,reject){ })
5. resolved,rejected函数：在异步事件状态pendding->resolved回调成功时，通过调用resolved函数返回结果；当异步操作失败时，回调用rejected函数显示错误信息
6. then的用法：then中传了两个参数，第一个对应resolve的回调，第二个对应reject的回调
7. catch方法：捕捉promise错误函数，和then函数参数中rejected作用一样，处理错误，由于Promise抛出错误具有冒泡性质，能够不断传递，会传到catch中，所以一般来说所有错误处理放在catch中，then中只处理成功的，同时catch还会捕捉resolved中抛出的异常

```js
p.then((data) => {
    console.log('resolved',data);
})
.catch((err) => {
    console.log('rejected',err);
});
```

8.all方法：Promise.all([promise1,promise2])——参数是对象数组。以慢为准，等数组中所有的promise对象状态为resolved时，该对象就为resolved；只要数组中有任意一个promise对象状态为rejected，该对象就为rejected

```js
let p = Promise.all([Promise1, Promise2])

p.then((data) => {
    //都成功才表示成功
})
.catch((err) => {
    //有一个失败，则都失败
});
```

9.race方法：Promise.race([promise1,promise2])——参数是对象数组。以快为准，数组中所有的promise对象，有一个先执行了何种状态，该对象就为何种状态，并执行相应函数

> 手撕代码请查看手撕代码的文件，这里不展示手撕代码噢

### 10.Iterator 和 for...of（Iterator遍历器的实现）

- for...of 描述

```js
- for…of 语句在可迭代对象上创建一个迭代循环，调用自定义迭代钩子，并为每个不同属性的值执行语句——MDN
- 一个数据结构只要部署了Symbol.iterator属性，就被视为具有 iterator 接口，就可以用for...of循环遍历它的成员
- 你会发现for...of和迭代器总是在一起， for...of循环内部调用的是数据结构的Symbol.iterator方法
```

- JavaScript 原有的 for…in 循环，只能获得对象的键名，不能直接获取键值。ES6 提供 for…of 循环，允许遍历获得键值

```js
var arr = ["a", "b", "c", "d"];

for (let a in arr) {
  console.log(a); // 0 1 2 3
}

for (let a of arr) {
  console.log(a); // a b c d
}

//for…in 循环读取键名
//for…of 循环读取键值
```

- 原生具备 Iterator 接口的数据结构如下。
  - Array
  - Map
  - Set
  - String
  - TypedArray
  - 函数的 arguments 对象
  - NodeList 对象

- Iterator遍历器的实现

```js
//实现
function makeIterator(array) {
  let index = 0;
  const iterator = {}; //iterator对象
  iterator.next = function() {
    if (index < array.length) return { value: array[index++], done: false };
    return { value: undefined, done: true };
  };
  return iterator;
}

//实例
var it = makeIterator(["a", "b"]);
it.next(); // { value: "a", done: false }
it.next(); // { value: "b", done: false }
it.next(); // { value: undefined, done: true }
```

- iterator遍历器使用的场景
  - 解构赋值
  - 扩展运算符...
  - 任何接受数组作为参数的场合，其实都调用了遍历器接口

### 11.循环语法比较及使用场景（for/for..of/forEach/for..in）

- for ：缺点是麻烦

- forEach：不可以使用continue，break，return等跳出循环
- for.. in：用于遍历所有可枚举的属性，但是遍历不到constructor，length这样的不可枚举属性

> -  数组的键名为数字，但是for…in循环是以字符串作为键名"0",“1”,“2”
> - for…in循环主要是为遍历对象而设计的，不适用于遍历数组。 

- for…of：所有实现了[Symbol.iterator]接口的对象都可以被遍历。for…of循环可以使用的范围包括数组、Set 和 Map 结构、某些类似数组的对象（比如arguments对象、DOM NodeList 对象）、Generator 对象，以及字符串

> - 用法简洁
> - 可以和break,continue,return配合使用
> - 提供了遍历所有数据结构的统一操作接口

```js
// demo
let arr = ['a', 'b', 'c'];
for (let pair of arr.entries()) {
  console.log(pair);
}
// [0, 'a']
// [1, 'b']
// [2, 'c']
```

```js
let arrayLike = { length: 2, 0: 'a', 1: 'b' };

// 报错
for (let x of arrayLike) {
  console.log(x);
}

// 正确
for (let x of Array.from(arrayLike)) {
  console.log(x);  // 'a' // 'b'
}
```

### 12.Generator及其异步方面的应用

Generator函数是ES6提供的一种异步编程解决方案，语法行为与传统函数完全不同。

>- 一是，function关键字与函数名之间有一个星号；
>
>- 二是，函数体内部使用yield表达式，定义不同的内部状态

```js
function*countAppleSales(){
    var arr = [1,2,3];
    for (var i=0;i<arr.length;i++){
        yield arr[i];
    }
}
//一旦生成器函数已定义，可以通过构造一个迭代器来使用它。
var appleStore = countAppleSales();//Generator{}
console.log(appleStore.next());//{value:1,done:false}
console.log(appleStore.next());//{value:2,done:false}
console.log(appleStore.next());//{value:3,done:false}
console.log(appleStore.next());//{value:undefined,done:true}
```

- 异步方面的应用

[请参考这里](https://blog.csdn.net/wjyyhhxit/article/details/103941789?ops_request_misc=&request_id=&biz_id=102&utm_term=Generator%E5%8F%8A%E5%85%B6%E5%BC%82%E6%AD%A5%E6%96%B9%E9%9D%A2%E7%9A%84%E5%BA%94%E7%94%A8&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-3-103941789.pc_search_result_no_baidu_js)

- 与Iterator的关系

> 任意一个对象的symbol.iterator方法，等于该对象的遍历器生成函数，调用该函数会返回一个遍历器对象。由于Generator函数就是遍历器生成函数，因此可以把`Generator赋值给对象的symbol.iterator属性`，从而`使得该对象具有Iterator接口`。

```js
var myIterator = {};
myIterator[Symbol.iterator] = function*(){
    yield 1;
    yield 2;
    yield 3;
}
[...myIterator]//[1,2,3]  //从而使得myIterable对象具有了Iterator接口，可以被...运算遍历了
```

### 13.async函数

async 函数对 Generator 函数的改进，体现在以下三点。

> - **内置执行器**
>
> ```js
> var result = async ReadFile(); //async的执行
> ```
>
> - **更好的语义**
>   - async 和 await，比起星号和 yield，语义更清楚了。async 表示函数里有异步操作，await 表示紧跟在后面的表达式需要等待结果。
>
> - **更广的适用性**
>   - co 函数库约定，yield 命令后面只能是 Thunk 函数或 Promise 对象，而 async 函数的 await 命令后面，可以跟 Promise 对象和原始类型的值（数值、字符串和布尔值，但这时等同于同步操作）



- 同 Generator 函数一样，async 函数返回一个 Promise 对象，可以使用 then 方法添加回调函数。当函数执行的时候，一旦遇到 await 就会先暂停当前函数的执行，等到触发的异步操作完成，再接着执行函数体内后面的语句。(***)

```js
async function getStockPriceByName(name) {
  var symbol = await getStockSymbol(name);
  var stockPrice = await getStockPrice(symbol);
  return stockPrice;
}

//async函数返回一个promise ，我们可以用then方法直接添加其回调函数
getStockPriceByName('goog').then(function (result){
  console.log(result);
});
```

上面代码是一个获取股票报价的函数，函数前面的async关键字，表明该函数内部有异步操作。调用该函数时，会立即返回一个Promise对象。

下面的例子，指定多少毫秒后输出一个值

```js
function timeout(ms) {
  return new Promise((resolve) => {
    setTimeout(resolve, ms);
  });
}

async function asyncPrint(value, ms) {
  await timeout(ms);//定时器
  console.log(value)
}

asyncPrint('hello world', 50); //50ms后输出hello world
```

##### 注意点

await 命令后面的 Promise 对象，运行结果可能是 rejected，所以最好把` await `命令放在` try…catch `代码块中

```js
async function myFunction() {
  try {
    await somethingThatReturnsAPromise();
  } catch (err) {
    console.log(err);
  }
}

// 另一种写法
async function myFunction() {
  await somethingThatReturnsAPromise().catch(function (err){
    console.log(err);
  });
}
```

- await 命令只能用在 async 函数之中，如果用在普通函数，就会报错。

### 14.几种异步方式的比较（setTimeout/Promise/Generator/async）

[请参考这里](https://blog.csdn.net/howgod/article/details/93978297?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522161346811316780261936345%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=161346811316780261936345&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_v2~rank_v29-6-93978297.pc_search_result_no_baidu_js&utm_term=%E5%87%A0%E7%A7%8D%E5%BC%82%E6%AD%A5%E6%96%B9%E5%BC%8F%E7%9A%84%E6%AF%94%E8%BE%83)

[具体demo请参考这里codeopen.io](https://codepen.io/newming/pen/wmPejP)

### 15.class基本语法及继承







### 16.模块加载方案及比较（commonJS和ES6的Module）

### 17.ES6模块加载与CommonJS加载的原理