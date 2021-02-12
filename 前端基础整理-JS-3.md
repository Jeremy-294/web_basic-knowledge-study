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



### 26.浏览器的回流（Reflow）和重绘（Repaints）

### 27.JavaScript中的arguments

### 28.EventLoop事件循环

### 29.宏任务与微任务

### 30.BOM属性对象方法

### 31.函数柯里化及其通用封装

### 32.JS的map()和reduce()方法

### 33.“==”和“===”的区别

### 34.setTimeout用作倒计时为何会产生误差？

