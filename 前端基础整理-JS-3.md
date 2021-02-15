### 21.`addEventListener` 和` onClick()`的区别

**区别：**`onclick`和`addEventListener`事件区别是：`onclick`事件会被覆盖，而`addEventListener`可以先后运行不会被覆盖，`addEventListener`可以监听多个事件

**例子：**

```js
<ul id="color-list">
    <li id="addEvent">red</li>
    <li id="on_click">yellow</li>
</ul>

<script type="text/javascript">
    (function(){
    var addEvent = document.getElementById("addEvent");
    addEvent.addEventListener("click",function(){
        alert("我是addEvent1");
    },false);
    addEvent.addEventListener("click",function(){
        alert("我是addEvent2");
    },false);

    var addEvent = document.getElementById("on_click"); 

    on_click.onclick = function() {
        alert("我是click1");
    }
    on_click.onclick = function() {
        alert("我是click2");
    }
})(); //输出：我是click2  因为被覆盖了 只输出一次 而addEventListener不会，它允许给一个事件注册多个监听器
</script>
```

### 22.`new `和` Object.create()`的区别

- 创建对象的两种方式 `new` 和 `object.create()`

```js
var Base = function () {}
var o1 = new Base();
var o2 = Object.create(Base);
```

**1.创建空对象的不同**

```js
1.1 new Object(null),创建的是空对象，在该对象上`有`继承原型链的方法和属性；
1.2 Object.creat(null),创建的是空对象，在该对象上`没有`继承原型链的方法和属性；

eg:
//方式一：通过new方法来创建实例
var person1 = new Person(null);
//方式二：通过.creat方法来创建示例
var person2 = Object.create(null)
console.log(person1);
console.log(person2)
```

**2. 创建实例的区别**

```js
//以下两个是一样的
2.1 使用 new Person() 来创建`Person构造函数`的新实例
2.2 使用 Object.create(Person.prototype) 来创建`Person构造函数`的新实例
//Person构造函数
function Person(){
}
//示例
//方式一：通过new方法来创建实例
var person1 = new Person();
//方式二：通过.creat方法来创建示例
var person2 = Object.create(Person.prototype)   //Person.prototype
console.log(person1);
console.log(person2);
```

若采用`new Object(Person) `和 `new Object(Person.prototype)`的区别 

```js
var person2 = Object.create(Person)
var person3 = Object.create(Person.prototype)
console.log('person2 instanceof Person:',person2 instanceof Person); //false
console.log('person3 instanceof Person:',person3 instanceof Person); //true
//结果显示 Object.create(Person)创建的对象不属于Person原型链，person2的__proto__不会指向person的prototype
//而通过Object.create(Person.prototype）创建的对象属于Person原型链
```

- `instanceof`和`isPrototypeOf`

```js
A.isPrototypeOf(B) ：A对象是否存在于B对象的原型链之中
A instanceof B ：B.prototype是否存在与A的原型链之中
```

### 23.`DOM`的`location`对象

```html
<html>
	<head>
	</head>

	<body>
		<script type="text/javascript">
		//演示window对象中的location对象
		function windowObjDemo_2(){
			var pro = location.protocol;//获取URL的协议部分，即浏览器地址栏中的当前地址的协议
			var text = location.href;//获取整个URL为字符串，即浏览器地址栏中的当前地址
			//给location的href属性设置一个值，改变了地址栏的值，并对其进行了解析如果是http，还进行连接访问
			location.href = "http://www.sina.com.cn";
			//鼠标一单击button，就访问新浪主页且浏览器地址栏变更为新浪主页地址
		}
		</script>
		<!--定义事件源，注册事件关联的动作-->
		<input type="button" value="演示window中的location" οnclick="windowObjDemo_2()" />
	</body>
</html>
```

- 常见属性 / 方法补充：

```js
/*
	<----location对象----->
	1.此对象包含当前的URL信息，属性有以下几个：
		1> href			设置或获取整个URL为一个字串
		2> protocol		设置或获取URL对应的协议部分
		3> pathname		设置或获取location或者URL对应的主机名称
		4> port			设置或获取与URL关联的端口号
		5> host			设置或获取location或URL对应的主机名和端口号
		6> hostname		设置或获取location或URL对应的主机名
		7> search		设置或获取属性href中问号后的内容
	2. 对象包含的方法：
	1> assign		装入新的html文档
	2> reload		重新载入当前页面
	3> replace		装入指定URL的文档来替换当前文档

	通过location对象的属性进行获取/设置属性
	*/
/*属性的获取*/
```

### 24.浏览器从输入URL到页面渲染的整个流程（涉及到计算机网络数据传输过程、浏览器解析渲染过程）

```js
一、获取IP地址
二、TCP/IP连接
三、浏览器向web服务器发送http请求
四、浏览器渲染
五、四次挥手
```

[详细文章](https://blog.csdn.net/weixin_43648017/article/details/113795121)

### 25.跨域、同源策略及跨域实现方式和原理

- **同源策略** (Same Origin Policy，**SOP**)是一种约定，它是浏览器最核心也最基本的**安全功能**，如果缺少了同源策略，则**浏览器的正常功能可能都会受到影响**。可以说Web是构建在同源策略基础之上的，浏览器只是针对同源策略的一种实现。

- 所谓同源是指**域名，协议，端口**相同

```js
例子：
当一个浏览器的两个tab页中分别打开百度和谷歌的页面时，`当浏览器的百度tab页执行一个脚本的时候会检查这个脚本是属于哪个页面的，即检查是否同源，只有和百度同源的脚本才会被执行`。而如果非同源，那么在请求数据时，浏览器会在控制台中报一个异常，提示拒绝访问。不同源的客户端脚本在没有明确授权的情况下，不能读写对方资源。所以google.com下的js脚本采用ajax读取baidu.com里面的文件数据是会报错的。
```

```js
不受同源策略的限制：
- 页面中的链接，重定向以及表单提交是不会受到同源策略限制的。
- 跨域资源的引入是可以的，但是js不能读写加载的内容。如嵌入到页面中的<script src="..."></script>，<img>，<link>，<iframe>等
```

- **跨域资源共享(Cross Origin Resourse-Sharing)**：受前面所讲的浏览器同源策略的影响，不是同源的脚本不能操作其他源下面的对象。_想要操作另一个源下的对象是就需要跨域。_

- 跨域的实现方式：

  - **1.降域 document.domain**

  ```js
  同源策略认为域和子域属于不同的域，如：
  child1.a.com 与 a.com，
  child1.a.com 与 child2.a.com，
  abc.child1.a.com 与 child1.a.com
  两两不同源，但是可以通过设置 document.domain='a.com'，浏览器就会认为它们都是同一个源。想要实现以上任意两个页面之间的通信，两个页面必须都设置documen.domain='a.com'。
  
  特点：
  - 只能在父域名与子域名之间使用，且将 xxx.child1.a.com域名设置为a.com后，不能再设置成child1.a.com
  - 存在安全性问题，当一个站点被攻击后，另一个站点会引起安全漏洞
  - 这种方法只适用于 Cookie 和 iframe 窗口
  ```

  - **2.JSONP跨域**

  ```js
  - JSONP 是 JSON With Padding（填充式 JSON 或参数式 JSON）的简写。
  
  JSONP实现跨域请求的原理：简单的说，就是动态创建<script>标签，然后利用<script>的 src 属性不受同源策略约束来跨域获取数据。
  JSONP 由两部分组成：`回调函数` 和 `数据`。回调函数是用来处理服务器端返回的数据，回调函数的名字一般是在请求中指定的。而数据就是我们需要获取的数据，也就是服务器端的数据。
  ```

### 26.浏览器的回流（Reflow）和重绘（Repaints）

前言：浏览器对页面的呈现的处理流程

```js
1. 文档初次加载时，浏览器引擎会解析HTML文档来构建DOM树
2. 之后根据DOM元素的几何属性构建一棵用于渲染的树。
3. 渲染树的每个节点都有大小和边距等属性，类似于盒子模型。
4. 当渲染树构建完成后，浏览器 就可以将元素放置到正确的位置了，再根据渲染树节点的样式属性绘制出页面。
```

![img](https://img-blog.csdn.net/2018062010031226)



- 回流：渲染树(render tree)中的一部分（或全部）因为元素的规模尺寸、布局、隐藏等改变而需要重新构建。

  (每个页面都至少会有一次回流，即在页面首次加载的时候)

  >1.页面首次渲染
  >2.浏览器窗口大小发生改变
  >3.元素尺寸或位置发生改变
  >4.元素内容变化（文字数量或图片大小等等）
  >5.元素字体大小变化
  >6.添加或者删除可见的DOM元素
  >7.激活CSS伪类（例如：:hover）
  >8.查询某些属性或调用某些方法

- 重绘：当渲染树中的一些元素需要更新属性，而这些属性只是影响元素的外观、风格，而不会影响布局，如 background-color。（常见的重绘有：单纯改变字体颜色，背景）
- 回流必然会引起重绘，重绘不一定会引起回流

##### 浏览器优化：(维护一个1个队列)

如果每一句带啊吗都有重绘回流的话，那么浏览器的开销会很大，因此，`浏览器会维护一个队列，把所有引起回流重绘的操作放入这个队列，当队列中的操作到达一定数量或者一定时间间隔后，浏览器就会flush队列，进行批处理。`

##### 如何减少回流、重绘？（在CSS中）

```js
尽可能在 DOM 树的最末端改变class
避免设置多项内联样式
动画效果应用到 position 属性或 fixed 的元素上
牺牲平滑度换取速度
避免使用 table 布局
避免使用 CSS 的 JavaScript 表达式（仅IE浏览器）
```

### 27.JavaScript中的arguments

在函数调用的时候，浏览器每次都会传递进两个隐式参数：

- 函数的上下文对象this

-  封装实参的对象arguments

```js
arguments类型是类数组对象

1.可以使用arguments[0]...等下标来使用传入的参数
2.该对象有一个length属性，表示传入的参数的个数
3.callee函数，指向当前函数，打印出来的话，就是我们的函数代码
`eg1:`
function add() {
	if( arguments.length == 2 ){
		return arguments[0] + arguments[1];
	}else{
		return '传入参数不合法';
	}
}

console.log( add(2,3) ); //5
console.log( add(1,2,3) ); //传入参数不合法

`eg2:`
function showcallee() {
    var a = '这里是代码';
    var b = '这是另一段代码';
    var c = a + b;
    
    console.log(arguments.callee);
    
    return c;
} 
showcallee(); 
//输出：
`function showcallee() {
    var a = '这里是代码';
    var b = '这是另一段代码';
    var c = a + b;
    
    console.log(arguments.callee);
    
    return c;
} `
```

- argument作用：

  - 实现方法的重载

  ```js
  //传入多少参数都可以
  function add() {
      var len = arguments.length,
          sum = 0;
      while(len--){
          sum += arguments[len];
      }
      return sum;
  }
  
  console.log( add(1,2,3) );   //6
  console.log( add(1,3) );     //4
  console.log( add(1,2,3,5,6,2,7) );   //26
  ```

  - 实现递归(当这个函数使匿名函数时才有意义啊，不然直接调用本函数名实现递归不就好了)

  ```js
  //计算阶乘  最好时匿名函数才有用把
  function factorial(num) { 
      if(num<=1) { 
      	return 1; 
      }else { 
      	return num * arguments.callee(num-1);  //return num * factorial(num-1); 
      } 
  } 
  ```

### 28.EventLoop事件循环

- 首先清楚javaScript这门语言是一门单线程非阻塞的脚本语言

>**单线程**是指在执行javascript代码的时候，只有一个主线程按照一定的顺序执行任务。
>**非阻塞**是指当代码需要进行一项异步任务的时候，主线程会挂起这个任务，然后在异步结果返回时根据一定规则执行相应的回调。
>
>由于JS是应用在浏览器操作和DOM操作上的脚本语言，所以只能是单线程，否则会有冲突
>
>eg：`如果多线程同时操作同一个dom元素的话，比如子线程修改dom元素，子线程B删除这个Dom元素，如果子线程B先执行完毕，那么子线程A修改dom元素就不成立，是个悖论。`

- 事件循环详细描述：

>javascript擎遇到一个异步事件后并不会一直等待其返回结果，而是会将这个事件挂起，继续执行执行栈中的其他任务。当一个异步事件返回结果后，js会将这个事件加入与当前执行栈不同的另一个队列，我们称之为事件队列。被放入事件队列不会立刻执行其回调，而是等待当前执行栈中的所有任务都执行完毕， 主线程处于闲置状态时，主线程会去查找事件队列是否有任务。如果有，那么主线程会从中取出排在第一位的事件，并把这个事件对应的回调放入执行栈中，然后执行其中的同步代码…，如此反复，这样就形成了一个无限的循环。这就是这个过程被称为“事件循环（Event Loop）

[https://blog.csdn.net/weixin_45111619/article/details/112019418?ops_request_misc=%25257B%252522request%25255Fid%252522%25253A%252522161336675916780261972472%252522%25252C%252522scm%252522%25253A%25252220140713.130102334..%252522%25257D&request_id=161336675916780261972472&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-112019418.pc_search_result_no_baidu_js&utm_term=%25E4%25BA%258B%25E4%25BB%25B6%25E5%25BE%25AA%25E7%258E%25AF%25E6%259C%25BA%25E5%2588%25B6eventloop](https://blog.csdn.net/weixin_45111619/article/details/112019418?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522161336675916780261972472%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=161336675916780261972472&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-112019418.pc_search_result_no_baidu_js&utm_term=%E4%BA%8B%E4%BB%B6%E5%BE%AA%E7%8E%AF%E6%9C%BA%E5%88%B6eventloop)

### 29.宏任务与微任务	

- 异步任务有两种类型：微任务（micorotask）和宏任务（macrotack），不同的类型的任务会放在不同的任务队列中（宏任务队列和微任务队列）

[https://blog.csdn.net/weixin_45111619/article/details/112019418?ops_request_misc=%25257B%252522request%25255Fid%252522%25253A%252522161336675916780261972472%252522%25252C%252522scm%252522%25253A%25252220140713.130102334..%252522%25257D&request_id=161336675916780261972472&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-112019418.pc_search_result_no_baidu_js&utm_term=%25E4%25BA%258B%25E4%25BB%25B6%25E5%25BE%25AA%25E7%258E%25AF%25E6%259C%25BA%25E5%2588%25B6eventloop](https://blog.csdn.net/weixin_45111619/article/details/112019418?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522161336675916780261972472%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=161336675916780261972472&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-112019418.pc_search_result_no_baidu_js&utm_term=%E4%BA%8B%E4%BB%B6%E5%BE%AA%E7%8E%AF%E6%9C%BA%E5%88%B6eventloop)

### 30.BOM属性对象方法

- BOM浏览器对象模型：

  ```js
  window 对象 （浏览器对象，全局对象）
  location 对象 （window.location 和document.location指向的是同一个对象）
  navigator 对象
  screen 对象
  history 对象
  ```

  - window 对象

  ```js
  1、窗口位置：
  　　　　screenTop，screenLeft（screenX,screenY）：窗口相对于屏幕左边和上边的位置
  　　　　moveTo(x,y):将窗口移动到特定位置
  　　　　moveby(xpx,ypx):移动的像素数
  2、获取窗口大小
  　　　　页面视图区大小：innerHeight,innerWidth
  　　　　浏览器窗口大小：outerHeight，outerWidth　　
  3、调整窗口
  　　　　resizeTo(新宽度，新高度)；//window.resizeTo(100,100);//将浏览器窗口调整为100x100，outerWidth和outerHeight访问的值
  　　　　resizeBy(宽差，高差)；//window.resizeBy(100,50),//又将窗口调整为200x150，在原窗口宽度的基础上增加了长度
  4.打开新窗口
  　　　　window.open();
  　　　　点击打开一个宽高各100的新窗口
         window.close();关闭窗口
  　　　　window.opener = null;切断与原窗口的链接
  5、超时调用
  　　　　setTimeout(functionName,1000);
  　　　　取消超时调用：clearTimeout();
  6、间歇调用
  　　　　setInterval();
  　　　　clearIntval();
  7、系统对话框：alert();confirm();prompt();
  ```

  - location 对象

  ```js
  location.href = "";：在原页面上重新加载一个网页
  window.open();：打开一个新窗口
  ```

  - history 对象

  ```js
  　href="Javascript:history.back();"
  1、go()
  history.go(-1)//后退一页
  history.go(1)//前进一页
  history.go(2)//前进两页
  2、
  history.back();//后退一页
  history.forward();//前进一页
  3、length属性
  if(history.length == 0)//判断是否是新打开的页面
  ```

  [BOM主要对象属性方法总结](https://www.cnblogs.com/Grace-zyy/p/8361805.html)

### 31.函数柯里化及其通用封装

- 增加函数通用性

> 柯里化（Currying）是把接受多个参数的函数变换成接受一个单一参数(最初函数的第一个参数)的函数，并且返回接受余下的参数且返回结果的新函数的技术。

```js
//判断参数类型的柯里化
function typeIs(type,arg){
    return  Object.prototype.toString.call(arg) === `[object ${type}]`
}
typeIs('Number',123) //output: true
typeIs('String',123) //output: false
typeIs('Object',{})  //output: true

//currying
function typeIs(type){
    return function(arg){
        return Object.prototype.toString.call(arg) === `[object ${type}]`
    }
}
let isNumber = typeIs('Number')
isNumber(1234)		// true
isNumber('1234')	// false

let isArray = typeIs('Array')
isArray([])	       // true
isArray({})		   //false
```

- 柯里化的通用封装

```js
//currying的通用封装
function currying(fn){  //fn是一个函数在上述例子中可以理解为判断类型的函数，该函数接受多个参数
    //***一个函数的长度就是它参数的个数***
    let fnLen = fn.length;

    //返回一个收集参数函数
    return function collectArgs(){
        //将收集的参数保存在args中
        let args = [...arguments];
        //参数收集完毕，将所有的参数传入fn中执行并返回执行结果
        if(args.length >= fnLen){
            return fn(...args);
        }
        //参数还没有收集完毕，返回一个函数，并于其中递归调用收集参数函数
        return function(){
            return collectArgs(...args,...arguments);
        }
    }
}

//直接拿函数typeIs(type,arg){}来实验：
function typeIs(type,arg){
	return  Object.prototype.toString.call(arg) === `[object ${type}]`
}

const whatType = currying(typeIs) //调用currying

const isString = whatType('String')
const isArray = whatType('Array')
console.log(isArray([]))       // true
console.log(isString(23))      // false
```

### 32.JS的map()和reduce()方法

- map() 参数为一个回调函数，输出结果为一个执行回调之后的新的数组

```js
//map()  计算数组中各元素的平方
function pow(x) {
    return x * x;
}
var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
var results = arr.map(pow); 
return result; // [1, 4, 9, 16, 25, 36, 49, 64, 81];  //返回一个新数组

//把Array的所有数字转为字符串,只需要一行代码。
var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
arr.map(String); // ['1', '2', '3', '4', '5', '6', '7', '8', '9']
```

- reduce()方法：Array的reduce()把一个函数作用在这个Array的[x1, x2, x3…]上，这个`回调函数`必须接收两个参数，reduce()把结果继续和序列的下一个元素做累积计算，其效果就是：

```js
[x1, x2, x3, x4].reduce(f) = f(f(f(x1, x2), x3), x4);
```

- reduce()实现累加器：

```js
1.使用reduce()对一个数组求和：
var arr = [1, 3, 5, 7, 9];
arr.reduce(function (x, y) {
    return x + y;
}); // 25

2.数组就乘积
var  arr = [1, 2, 3, 4];
var mul = arr.reduce((x,y)=>x*y)
console.log( mul ); //求乘积，24
```

- reduce()高级用法：对象里元素求和

```js
var result = [
    {
        subject: 'math',
        score: 10
    },
    {
        subject: 'chinese',
        score: 20
    },
    {
        subject: 'english',
        score: 30
    }
];

var sum = result.reduce(function(prev, cur) {
    return cur.score + prev;
}, 0);
console.log(sum) //60
```

[完整reduce()高级用法](https://blog.csdn.net/qq_24147051/article/details/102834109?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522161336980316780269869435%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=161336980316780269869435&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_v2~rank_v29-3-102834109.pc_search_result_no_baidu_js&utm_term=jsreduce%28%29%E6%96%B9%E6%B3%95)

### 33.“==”和“===”的区别

**1、对于string,number等基础类型，==和===是有区别的**

1）不同类型间比较，==之比较“转化成同一类型后的值”看“值”是否相等，===如果类型不同，其结果就是不等，

==可以进行类型转换

2）同类型比较，直接进行“值”比较，两者结果一样

**2、对于Array,Object等高级类型，==和===是没有区别的**

进行“指针地址”比较

**3、基础类型与高级类型，==和===是有区别的**

1）对于==，将高级转化为基础类型，进行“值”比较

2）因为类型不同，===结果为false

### 34.setTimeout用作倒计时为何会产生误差？

定时器是属于 **宏任务(macrotask)** 。如果当前 **执行栈** 所花费的时间大于 **定时器** 时间，那么定时器的回调在 **宏任务(macrotask)** 里，来不及去调用，所有这个时间会有误差。

- 情况一

```js
setTimeout(function () {
	console.log('biubiu');
}, 1000);

某个执行时间很长的函数();
```

如果定时器下面的函数执行要 5秒钟，那么定时器里的log 则需要 5秒之后再执行，函数占用了当前 **执行栈** ，要等执行栈执行完毕后再去读取 **微任务(microtask)**，等 **微任务(microtask)** 完成，这个时候才会去读取 **宏任务(macrotask)** 里面的 **setTimeout** 回调函数执行。**setInterval** 同理，例如每3秒放入宏任务，也要等到执行栈的完成。

```js
浏览器端执行顺序：
1.执行同步代码，这是宏任务
2.执行栈为空，查询是否有微任务要执行
3.必要时渲染UI
4.进行下一轮的 EventLoop ，执行宏任务中的异步代码
```

- 情况二

```js
setTimeout(function() {
    setTimeout(function() {
        setTimeout(function() {
            setTimeout(function() {
                setTimeout(function() {
                    setTimeout(function() {
                        console.log('嘤嘤嘤');
                    }, 0);
                }, 0);
            }, 0);
        }, 0);
    }, 0);
}, 0);

//如果timeout嵌套大于 5层，而时间间隔小于4ms，则时间间隔增加到4ms。
```

[详细内容](https://juejin.cn/post/6844903861925199886?utm_medium=hao.caibaojian.com&utm_source=hao.caibaojian.com%3Futm_medium%3Dhao.caibaojian.com&utm_source=hao.caibaojian.com)