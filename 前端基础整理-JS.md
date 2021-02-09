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

