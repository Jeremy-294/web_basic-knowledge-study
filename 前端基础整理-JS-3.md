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

[https://blog.csdn.net/hz___zh/article/details/110958662?ops_request_misc=&request_id=&biz_id=102&utm_term=EventLoop%25E4%25BA%258B%25E4%25BB%25B6%25E5%25BE%25AA%25E7%258E%25AF&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-1-110958662.pc_search_result_no_baidu_js](https://blog.csdn.net/hz___zh/article/details/110958662?ops_request_misc=&request_id=&biz_id=102&utm_term=EventLoop%E4%BA%8B%E4%BB%B6%E5%BE%AA%E7%8E%AF&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-1-110958662.pc_search_result_no_baidu_js)

### 29.宏任务与微任务	



### 30.BOM属性对象方法

### 31.函数柯里化及其通用封装

### 32.JS的map()和reduce()方法

### 33.“==”和“===”的区别

### 34.setTimeout用作倒计时为何会产生误差？

