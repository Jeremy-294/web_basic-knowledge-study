# 目录

## **JS** 

1.  模拟实现函数的call、apply、bind方法  ★★ 
2.  模拟实现函数节流、防抖方法    ★★★ 
3.  模拟实现对象的深拷贝    ★★ 
4.  嵌套数组指定层次展开 flat扁平化（多种方法，至少掌握两种）★★★ 
5.  模拟实现 reduce 数组方法  ★ 
6.  模拟实现数组map方法  ★★ 
7.  模拟实现Array.fill()、Array.filter()  ★★ 
8.  模拟实现Array.find()、Array.findIndex()★★ 
9.  模拟实现Promise.all方法(Promise.race也需要了解)★★ 
10.  使用原生的[JavaScript]()实现ajax请求（也可能让你说ajax实现原理，答案一样）★★★ 
11.  模拟实现构造函数new的过程（也可能让你口述new的过程，答案一样）★★★ 
12.  模拟实现Object.create方法  ★★ 
13.  模拟实现instanceof的功能    ★★★ 
14.  使用setTimeout实现setInterval方法 ★★ 
15.  实现jsonp★★★ 
16.  promise 实现sleep函数★★ 
17.  实现promise retry重试★ 
18.  js实现观察者模式★ 



##  **数据结构和[算法]()** 

1.  [排序]()[算法]()（要会分析时间空间复杂度）：冒泡、选择、插入、快排  ★★★ 归并   ★ 

2.  [二分查找]()（非递归递归）★★★ 

3.  字符串逆序（翻转整数字符串）★★★ 

4.  数组乱序（打乱数组，至少掌握两种方法）★★★ 

5.  数组去重（至少掌握两种方法）★★★ 

6.  两个栈来实现一个队列（两个队列实现栈可以了解一下）★ 

7.  [链表]()相关 

   ​      入门：★★★      

   -  反转单[链表]() 
   -  未[排序]()[链表]()去重O（n2） 
   -  [排序]()[链表]()去重O（n） 
   -  单[链表]()删除节点 
   -  [链表]()partition 
   -  寻找[链表]()倒数第K个节点 
   -  删除[链表]()倒数第N个节点 
   -  判断[链表]()是否为回文[链表]() 
   -  判断[链表]()是否有环 
   -  环形[链表]()第一个入环节点 
   -  两个[链表]()的第一个公共节点 

   ​       复杂：       

   -  合并两个[排序]()的[链表]()★★ 
   -  合并K个[排序]()的[链表]()★★ 
   -  奇偶[链表]()★ 
   -  复制带随机指针的单向[链表]()★ 
   -  有序[链表]()转换二叉搜索树（BST）★ 
   -  [二叉树]()展开为[链表]()★ 
   -  K 个一组翻转[链表]()★ 

8.  [二叉树]()各种遍历（前中后序遍历，递归非递归，DFS,BFS）★★★ 

9.  [二叉树]()遍历涉及到的一些[算法题]() 

    （好多题其实就是[二叉树]()深度或者广度非递归遍历稍微改一下即可） 

   -  前序和中序[重建二叉树]()★★ 
   -  BST第K大的数和第K小的数★ 
   -  [二叉树]()按层求和（层序遍历改进）★ 
   -  Z字型（之字形）遍历[二叉树]()★ 

10.  [二叉树]()深度相关 

    -  [二叉树]()深度★★★ 
    -  [二叉树]()最小深度★★ 
    -  树找两(叶子)节点最长距离（相隔的最长路径）★ 
    -  判断[二叉树]()是否为[平衡二叉树]()★★ 
    -  [二叉树]()右视图（左）★ 

11.  [二叉树]()路径相关 

    -  路径总和1★★★ 
    -  路径总和2（回溯法）★ 

12.  DP     

    -  [斐波那契数列]()★★★ 
    -  [最长公共子序列]() LCS ★★ 
    -  最长上升子序列 ★★ 
    -  连续子数组（子串）的最大和★★ 
    -  硬币找零（最少硬币个数）★★ 
    -  0-1背包问题★ 
    -  完全背包问题（了解即可）★ 

13.  全排列（回溯法）见LeetCode 全排列1、全排列2 ★ 

##  **CSS** 

1.  CSS画各种图形（等腰三角形、等腰梯形、扇形、圆、半圆）  ★ 
2.  三列布局（至少掌握三种方法）  ★★★ 
3.  垂直水平居中（至少掌握三种方法）  ★★★ 



# 代码区

## **JS**

1.模拟实现函数的call、apply、bind方法  ★★

```js
/* 先将传入的指定执行环境的对象 context 取到
将需要执行的方法(调用call的对象) 作为 context 的一个属性方法fn
处理传入的参数， 并执行 context的属性方法fn， 传入处理好的参数
删除私自定义的 fn 属性
返回执行的结果 */
// 模拟 call 方法
Function.prototype.defineCall = function (context) {
    context = context || window;
    context.fn = this; //this指向 sayName函数实例
    let args = [];
    for (let i = 1; i < arguments.length; i++) { //i从1开始 
        args.push(arguments[i]);
    } //或者args = [...arguments].slice(1);
    let result = context.fn(...args));     //args.join(',')好像不对 ，bs
    delete context.fn;
    return result;
}
let sayName = function (age) {
    console.log('current name: ' + this.name, "current age: " + age);
}
let obj1 = {
    name: "obj's name"
}
sayName.defineCall(obj1, 22); //this指向 sayName函数实例
// current name: obj's name current age: 22


// 模拟 apply 方法
Function.prototype.defineApply = function (context, arr) {
    context = context || window;
    context.fn = this;
    let result;
    if (!arr) { // 第二个参数不传
        result = context.fn();
    } else { // 第二个参数是数组类型
        let args = [];
        for (let i = 0; i < arr.length; i++) { //i从0开始
            args.push(arr[i]);
        }
        result = context.fn(args.join(','));
    }
    delete context.fn;
    return result;
}
let obj2 = {
    name: ['Tom', 'Johy', 'Joe', 'David']
}
sayName.defineApply(obj2, [3, 4, 5, 6, 7]);
// current name: Tom,Johy,Joe,David current age: 3,4,5,6,7


//用call、apply模拟实现bind
Function.prototype.bind = function (context) {
    let self = this; // 保存函数的引用
    return function () { // 返回一个新的函数
        // console.log(arguments);
        // return self.apply(context, arguments);
        return self.call(context, arguments);
    }
};

let obj = {
    name: 'seven'
}

let func = function () {
    console.log(this.name)
}.bind(obj);
func('zhangsan', 20);
```

2.模拟实现函数节流、防抖方法    ★★★

```js
function debounce(fn, delay) {//防抖
  // 维护一个 timer，用来记录当前执行函数状态
  let timer = null;

  return function() {
    // 通过 ‘this’ 和 ‘arguments’ 获取函数的作用域和变量
    let context = this;
    let args = arguments;
    // 清理掉正在执行的函数，并重新执行
    clearTimeout(timer);
    timer = setTimeout(function() {
      fn.apply(context, args);
    }, delay);
  }
}
let flag = 0; // 记录当前函数调用次数
// 当用户滚动时被调用的函数
function foo() {
  flag++;
  console.log('Number of calls: %d', flag);
}

// 在 debounce 中包装我们的函数，过 2 秒触发一次
document.body.addEventListener('scroll', debounce(foo, 2000));

function throttle(func, delay){//节流
  let timer = null;

  return function(){
    let context = this;
    let args    = arguments;
    if(!timer){
      timer = setTimeout(function(){
        func.apply(context, args);
        timer = null;
      }, delay);
    }
  }
}
```

3.模拟实现对象的深拷贝    ★★

```js
//第一种：JSON.parse(JSON.stringify())方法实现深拷贝
var obj={a:"hello",b:1,c:true,d:[1,2],e:{x:1,y:2},f:function(){console.log("copytest");},g:null,h:undefined};
var copyobj=JSON.parse(JSON.stringify(obj));
console.log(copyobj);
copyobj.a="change";
copyobj.d[0]=9;
copyobj.e.x=8;
console.log(obj);


//第二种：递归的方法实现深拷贝
function deepclone(obj,copyobj){
    var copyobj=copyobj||{};
    for (var keys in obj) { //使用for..in进行遍历   数组的话keys为0,1..... keys是string类型
        if(obj.hasOwnProperty(keys)){//剥离原型链的数据
            if((typeof(obj[keys])) ==='object' && obj[keys]!==null){//判断是否为引用数据类型
                if (Object.prototype.toString.call(obj[keys]) === '[object Array]') { //Object原型方法得到类型
                    copyobj[keys]=[];
                }else{
                    copyobj[keys]={};
                }
                deepclone(obj[keys],copyobj[keys]);
            }else{
                copyobj[keys]=obj[keys];
            }
        }
    }
    return copyobj;
}
```

4.嵌套数组指定层次展开 flat扁平化（多种方法，至少掌握两种）★★★

```js
// 嵌套数组指定层次展开   flat扁平化
// 1. 普通方法 递归
function flattenMd() {
    let result = []
    return function flatten(arr) {//闭包
        arr.forEach(item => {
            if (Array.isArray(item)) {
                flatten(item)
            } else {
                result.push(item)
            }
        })
        return result
    }
}
// var ary = [1, [2, [3, [4, 5]]], 6]
// flattenMd()(ary);  函数柯里化  部分求值

//2.concat  与方法1类似 没用闭包
function flattenMd(arr) {
    let result = [] // 利用函数作用域保存result var result = []也可
    arr.forEach(item => {
        if (Array.isArray(item)) {
            result = result.concat(flattenMd(item))
        } else {
            result.push(item)
        }
    })
    return result
}

//3. reduce 
function flattenMd(arr) { //.concat([3,4])和.concat(3,4)均可
    return arr.reduce((prev, item) => prev.concat(Array.isArray(item) ? flattenMd(item) : item), [])
}

// 4. 展开运算符
function flattenMd(arr) {
    let flatten = arr => [].concat(...arr)//可去掉一层[]
    return flatten(arr.map(item => Array.isArray(item) ? flattenMd(item) : item))
}

// 5. join和split组合（ 只适用字符串数组， 最简单粗暴）
//[ '1', '2', '3', '4', '5', '6' ] 得到的是字符串数组  再转换一下才行
function flattenMd(arr) {
    return arr.join().split(',')
    //join() 方法用于把数组中的所有元素放入一个字符串 默认用,隔开
}
```

5.模拟实现 reduce 数组方法  ★

```js
//reduce（）函数接受两个参数，一个函数一个累积变量的初始值。
//函数有四个参数：累计变量初值（默认第一个成员），当前变量值（默认第二个成员），当前位置，数组自身。
//arr.reduce(function(prev, cur, index, arr){}, initialValue)
Array.prototype.myReduce=function(fn,base){
    if(typeof fn !== 'function'){
        throw new TypeError("arguments[0] is not a function");//TypeError是错误的类型之一：类型错误
    }

    var initialArr=this;//调用myReduce()函数的当前对象
    var arr=initialArr.concat();//目的是返回一个等于初始数组的新数组，后面的操作都基于arr，这样初始数组不会发生改动
    var index,newValue;
    
    if(arguments.length==2){
        arr.unshift(base);
        index = 0; //！！当前位置 指的是当前变量（第二个参数）针对调用该方法的数组位置即initialArr
    }else{
        index=1;
    }

    if(arr.length===1){//长度为1 直接返回
        newValue=arr[0];
    }

    while(arr.length>1){
        newValue=fn.call(null,arr[0],arr[1],index,initialArr);
        index++;
        arr.splice(0,2,newValue);//删除前两位 然后把累加值添加到第一位
    }
    return newValue;
};
```

6.模拟实现数组map方法  ★★

```js
// 模拟实现map
// arr.map(function (currentValue, index, arr) {
// })
// currentValue 必须。 当前元素的值
// index 可选。 当期元素的索引值
// arr 可选。 当期元素属于的数组对象

Array.prototype.newMap = function (fn) { //写法一
    var newArr = [];
    for (var i = 0; i < this.length; i++) {
        newArr.push(fn(this[i], i, this)) //this指向调用newMap方法的数组
    }
    return newArr;
}

// arr.reduce((previousValue, currentValue, currentIndex, array) => {}, initialValue？)
// reduce若不指定初始值， 那么第一次迭代的 previousValue 为 ar[[0], currentValue 为 arr[1], currentIndex 为 1，
// 若指定初始值， 那么第一次迭代的 previousValue 为 initialValue, currentValue为 arr[0], currentIndex 为0.

Array.prototype.newMap = function (fn, Arg) { ////写法二：用数组的reduce方法实现数组的map
    var res = [];
    this.reduce((prev, curr, index, array) => {
        res.push(fn.call(Arg, curr, index, array));
    }, 0) //指定初始值initialValue=0，所以从currentIndex=0开始，即第一个开始  不这样会缺第一项，结果为[3,4]
    return res;
}


let arr = [1, 2, 3];
let res = arr.newMap((a) => a + 1);
console.log(res); //[2,3,4]
```

7.模拟实现Array.fill()、Array.filter()  ★★        

​         array.fill(value, start, end)        

​         value 必需。填充的值。start 可选。开始填充位置。end 可选。停止填充位置 (默认为 array.length)

```js
Array.prototype.myFill = function (value, start = 0, end = this.length) {
    for (let i = start; i < end; i++) {
        this[i] = value;
    }
}
```

Array.filter()

```js
Array.prototype.myFilter = function myFilter(fn, context) {
    if (typeof fn !== "function") {
        throw new TypeError(`${fn} is not a function`);
    }
    let arr = this;
    let temp = [];
    for (let i = 0; i < arr.length; i++) {
        let result = fn.call(context, arr[i], i, arr);
        if (result) temp.push(arr[i]);
    }
    return temp;
};
```

8.模拟实现Array.find()、Array.findIndex()★★        

​         Array.find()        

​         用于找出第一个符合条件的数组成员，参数为一个回调函数        

​         [1, 4, -5, 10].find((n) => n < 0) // -5

```js
Array.prototype.myFind = function (fn, start = 0, end = this.length) {
    for (let i = start; i < end; i++) {
        if (fn.call(this, this[i], i, this)) {
            return this[i]
        }
    }
}
```

Array.findIndex()

```js
Array.prototype.myFindIndex = function (fn, start = 0, end = this.length) {
    for (let i = start; i < end; i++) {
        if (fn.call(this, this[i], i, this)) {
            return i
        }
    }
    return -1
}
```

9.模拟实现Promise.all方法(Promise.race也需要了解)★★

```js
 //promise.all()
  function myall(proArr) {
    return new Promise((resolve, reject) => {
      let ret = []
      let count = 0
      let done = (i, data) => {
        ret[i] = data
        if(++count === proArr.length) resolve(ret)
      }
      for (let i = 0; i < proArr.length; i++) {
        proArr[i].then(data => done(i,data) , reject)
      }
    })
  }
  
  
//promise.race();这么简单得益于promise的状态只能改变一次，即resolve和reject都只被能执行一次
 function myrace(proArr) {
    return new Promise(function (resolve, reject) {
      for(let i=0;i<proArr.length;i++){
        proArr[i].then(resolve,reject);
      }
    })
  }
```

10.使用原生的[JavaScript](https://www.nowcoder.com/jump/super-jump/word?word=JavaScript)实现ajax请求（也可能让你说ajax实现原理，答案一样）★★★

```js
//手写3遍以上
var xhr = new XMLHttpRequest();// 创建XMLHttqRequest
var url = 'https://bbin.com';
xhr.onreadystatechange=function(){// 监听状态码的变化，每次变化 均执行
    if(xhr.readyState===4){

        if (xhr.status === 200) { // 服务端 状态码
            console.log(xhr.responseText); //服务器返回的响应文本
        }else{
            console.error(xhr.statusText); //状态码的文本描述，如200的statusText是ok
        }
    
    }
}

xhr.open('GET', url, true); // 初始化请求参数，还没发送请求   true表示异步
xhr.send(null); // 向服务器发送请求,但是不带有数据发送过去,一般在get方式发送时候多使用这个方式
```

11.模拟实现构造函数new的过程（也可能让你口述new的过程，答案一样）★★★

```js
function myNew(constructor,params){
  var args = [].slice.call(arguments);
  var constructor = args.shift();
  var obj = new Object();
  obj.__proto__ = constructor.prototype;
  var res = constructor.apply(obj,args);
  return (typeof res === 'object' && typeof res !== null) ? res : obj;
}
```

12.模拟实现Object.create方法  ★★

```js
// 用于创建一个新对象,被创建的对象继承另一个对象(o)的原型
function createObj(o) {//传入的参数o为返回实例的__porto__,也就是实例构造函数的显示原型
    function F() {}//构造函数
    F.prototype = o;
    return new F();//返回实例
}
```

13.模拟实现instanceof的功能    ★★★

```js
function instanceofObj(a, b) {
    // 模拟 a instanceof b
    let prototypeB = b.prototype;
    let protoA = a.__proto__;
    let state = false;
    while (true) {
        if (protoA == null) { // 可能是 undefined 
            state = false;
            break;
        }
        if (prototypeB === protoA) {
            state = true;
            break;
        }
        protoA = protoA.__proto__;
    }
    return state;
}
console.log(instanceofObj([], Array));
instanceofObj([], Array); //true
```

14.使用setTimeout实现setInterval方法 ★★

```js
function mysetinterval(fn,time){
    console.log("利用steTimeout实现setInterval");
    function interval(){//执行该函数，异步被挂起time时间后在执行，一上来就执行fn
        setTimeout(interval,time);//异步
        //好，time时间过去，这个异步被执行，而内部执行的函数正是interval，就相当于进了一个循环，递归
        fn();//同步
    }
    setTimeout(interval,time);//interval被延迟time时间执行
}
```

15.实现jsonp★★★

```js
var newscript=document.createElement('script');
newscript.src='https://sp0.baidu.com/su?wd=Java&cb=foo';
document.body.appendChild(newscript);
function foo(data) { //callback函数要绑定在window对象上
   console.log(data);
}
```

16.promise 实现sleep函数★★

```js
async function test() {
    console.log('开始');
    await sleep(4000);
    console.log('结束');
}
 
function sleep(ms) {
    return new Promise(resolve => {
        setTimeout(resolve, ms);
    })
}
test();
```

17.实现promise retry重试★

实现函数myGetData，也返回一个promise，但是有失败重试功能

```js
function myGetData(getData, times, delay) {//retry函数
return new Promise(function (resolve, reject) {
    function attempt() {
        getData().then(resolve).catch(function (erro) {
            console.log(`还有 ${times} 次尝试`)
            if (0 == times) {
                reject(erro)
            } else {
                times--
                setTimeout(attempt(), delay)
            }
        })
    }
    attempt()
})
}
```

18.js实现观察者模式★

```js
//ES6 实现观察者模式代码：（观察订阅模式）
const queuedObservers = new Set();
const observe = fn => queuedObservers.add(fn); // 依赖收集  Vue中的dep
const observable = obj => new Proxy(obj, {set});// Proxy和Reflect一一对应
function set(target, key, value, receiver){
    const result = Reflect.set(target, key, value, receiver); // 执行set
    queuedObservers.forEach(observe => observe()); // 派发更新
    return result;
}

// 使用 观察者模式实例
const person = observable({
    name: 'Sun',
    age: 30
});

function print(){
    console.log(`name: ${person.name}, age: ${person.age}`);
}
observe(print);

person.age = 31;
// name: Sun, age: 31
```

## **数据结构和[算法](https://www.nowcoder.com/jump/super-jump/word?word=算法)**

1. [排序](https://www.nowcoder.com/jump/super-jump/word?word=排序)[算法](https://www.nowcoder.com/jump/super-jump/word?word=算法)（要会分析时间空间复杂度）：冒泡、选择、插入、快排  ★★★ 归并   ★
2. [二分查找](https://www.nowcoder.com/jump/super-jump/word?word=二分查找)（非递归递归）★★★

```js
//二分查找 arr为有序数组
function binary_search(nums, target) { //非递归
var left = 0;
var right = nums.length - 1;
while (left <= right) {
    mid = left + parseInt((right - left) / 2);
    if (target == nums[mid]) return mid;
    if (target < nums[mid]) {
        right = mid - 1;
    } else {
        left = mid + 1;
    }
}
return -1;
};

// 递归算法
function binary_search(arr, low, high, key) {
    if (low > high) {
        return -1;
    }
    var mid = parseInt((high + low) / 2);
    if (arr[mid] == key) {
        return mid;
    } else if (arr[mid] > key) {
        high = mid - 1;
        return binary_search(arr, low, high, key);
    } else if (arr[mid] < key) {
        low = mid + 1;
        return binary_search(arr, low, high, key);
    }
};
```

3.字符串逆序（翻转整数字符串）★★★

```js
//JavaScript 中可以使用 split 结合 数组自带的 reverse，先转成数组，再将数组拼接成字符串。
var str = 'abc';
var reverseStr = str.split('').reverse().join('');

//经过此操作 str 还是不变
//方法二：不使用API
var x = 501120;
var digit;
var ret = 0;
var count = [];
while (x > 0) {
    digit = x % 10;
    count.push(digit);
    ret = ret*10+digit;
    x = parseInt(x / 10);
} 
console.log(count.join(""));//'021105' 字符串类型
console.log(ret);//21105 Number 类型
```

4.数组乱序（打乱数组，至少掌握两种方法）★★★

```js
// 方法1：arr.sort((a,b) => Math.random() - 0.5);
arr.sort(() => Math.random() - 0.5);// 使用sort

// 方法2：时间复杂度 O(n^2)
function randomSortArray(arr) {
    let backArr = [];
    while (arr.length) {
        let index = parseInt(Math.random() * arr.length);
        backArr.push(arr[index]);
        arr.splice(index, 1);
    }
    return backArr;
}

// 方法3：时间复杂度 O(n)
function randomSortArray2(arr) {
    let lenNum = arr.length - 1;
    let tempData;
    for (let i = 0; i < lenNum; i++) {
        let index = parseInt(Math.random() * (lenNum + 1 - i));
        tempData = a[index];
        a[index] = a[lenNum - i]// 随机选一个放在最后
        a[lenNum - i] = tempData;
    }
    return arr;
}
```

5.数组去重（至少掌握两种方法）★★★

```js
//方法1：普通版  
function arrayUnique(arr){
  let res = [];
  for(let i=0,len=arr.length;i< len;i++){
      let item = arr[i];
      (res.indexOf(item) === -1) && res.push(item);// res中没有该数才push 去重
  }
  return res;
}
// 方法2：hashMap
function arrayUnique2(arr) {
  let res = [];
  let hash = {};
  for (let i = 0; i < arr.length; i++) {
    let item = arr[i];
    let key = typeof(item) + item;
    if (hash[key] !== 1) {
      res.push(item);
      hash[key] = 1;
    }
  }
  return res;
}

//方法3：使用 Set 进行数组去重
// 使用 Set 完成 数组去重,只能去除字符串和数字的重复，不能去除对象的重复  
function uniqueSet(array) {
  return Array.from(new Set(array));
}:
function uniqueSet2(array) {
  return [...new Set(array)];
}

// 方法4：使用 filter 的版本
function unique(a) {
  var res = a.filter(function(item, index, array) {
    return array.indexOf(item) === index;// 每次都是从左边开始做找
  });
  return res;
}

// 方法5：原型方法版本
Array.prototype.unique = function () {
  const newArray = [];
  this.forEach(item => {
    if (newArray.indexOf(item) === -1) {
      newArray.push(item);
    }
  });
  return newArray;
}

// 方法6：精简版 使用 filter 和 indexOf 结合实现
Array.prototype.unique = function () {
  return this.filter((item, index) => {
    return this.indexOf(item) === index;
  })
}
```

6.两个栈来实现一个队列（两个队列实现栈可以了解一下）★

```js
//用两个栈来实现一个队列，完成队列的Push和Pop操作。
var stackPush = [];
var stackPop = [];

function push(node) {
    // write code here
    stackPush.push(node);
}

function pop() {
    if (stackPop.length==0 && stackPush.length==0) {
        console.log("Queue is empty!");
    } else if (stackPop.length==0) {
        while (!stackPush.length==0) {
            stackPop.push(stackPush.pop());
        }
    }
    return stackPop.pop();
}
```

7.[链表](https://www.nowcoder.com/jump/super-jump/word?word=链表)相关

入门：★★★

- 反转单[链表](https://www.nowcoder.com/jump/super-jump/word?word=链表)

```js
var reverseList = function(head) {
    //let [prev, curr] = [null, head];
    let prev=null;
    let curr=head;
    while (curr) {
        let tmp = curr.next;    // 1. 临时存储当前指针后续内容
        curr.next = prev;       // 2. 反转链表
        prev = curr;            // 3. 接收反转结果
        curr = tmp;             // 4. 接回临时存储的后续内容
    }
    return prev;
};
```

- 未[排序](https://www.nowcoder.com/jump/super-jump/word?word=排序)[链表](https://www.nowcoder.com/jump/super-jump/word?word=链表)去重O（n2）

```js
//移除未排序链表中的重复节点。保留最开始出现的节点。（两层循环）
 //输入：[1, 2, 3, 3, 2, 1]
 //输出：[1, 2, 3]
var removeDuplicateNodes = function (head) {
    var p = head;
    while (p) {
        var q = p;
        while (q.next) {
            if (q.next.val == p.val) {
                q.next = q.next.next;
            } else {
                q = q.next;
            }
        }
        p = p.next;
    }
    return head;
};
```

- [排序](https://www.nowcoder.com/jump/super-jump/word?word=排序)[链表](https://www.nowcoder.com/jump/super-jump/word?word=链表)去重O（n）

```js
var deleteDuplicates = function(head) {
    let current = head //把首节点指针赋值给current
    while(current && current.next) { //当前节点以及下一节点不为空时
        if(current.val === current.next.val) { //值相等
            current.next = current.next.next
        }else {
            current = current.next //值不相等
        }
    }
    return head //返回首节点
}
```

- 单[链表](https://www.nowcoder.com/jump/super-jump/word?word=链表)删除节点

```js
//给定单向链表的头指针和一个要删除的节点的值，定义一个函数删除该节点。
//返回删除后的链表的头节点。
var deleteNode = function(head, val) {
    let pre = new ListNode(0); // 额外添加头节点 哨兵节点 考虑要删除的节点在第一个
    pre.next = head;

    let node = pre;
    while (node.next) {
        if (node.next.val === val) {
            node.next = node.next.next;
            break;
        }
        node = node.next;
    }
    return pre.next;
};
```

- [链表](https://www.nowcoder.com/jump/super-jump/word?word=链表)partition

```js
//思路：一拆为二然后合并
function partition(head, x) {
var dummy1 = new ListNode(-1); //辅助节点
var dummy2 = new ListNode(-1); //辅助节点
var p1 = dummy1;
var p2 = dummy2;
var p = head;
while (p != null) {
    if (p.val < x) {
        p1.next = p;
        p1 = p1.next;
    } else {
        p2.next = p;
        p2 = p2.next;
    }
    p = p.next;
}
if (dummy1.next == null) {
    return head;
} else {
    p1.next = dummy2.next;
    p2.next = null; //以null结尾
    return dummy1.next;
}
}
```

- 找[链表](https://www.nowcoder.com/jump/super-jump/word?word=链表)倒数第K个节点

```js
//给定一个链表: 1->2->3->4->5, 和 k = 2.
//返回链表 4->5.
var getKthFromEnd = function(head, k) {
//用两个指针来跑，两个指针中间相距k-1个节点,第一个指针先跑，跑到了第k个节点时，第二个指针则是第一个节点。
//这时候两个一起跑。当第一个跑到了最后一个节点时，这时候第一个指针则是倒数第k个节点。
  if (head === null || k <= 0) return null;
  let pNode1 = head,pNode2 = head;
  while (--k) {
    if (pNode2.next !== null) {
      pNode2 = pNode2.next;
    } else {
      return null;
    }
  }
  while (pNode2.next !== null) {
    pNode1 = pNode1.next;
    pNode2 = pNode2.next;
  }
  return pNode1;
};
```

- 删除[链表](https://www.nowcoder.com/jump/super-jump/word?word=链表)倒数第N个节点

```js
//给定一个链表: 1->2->3->4->5, 和 n = 2.
//当删除了倒数第二个节点后，链表变为 1->2->3->5.

var removeNthFromEnd = function(head, n) {
    let preHead = new ListNode(0)
    preHead.next = head
    let fast = preHead, slow = preHead
    // 快先走 n+1 步
    while(n--) {
        fast = fast.next
    }
    // fast、slow 一起前进
    while(fast && fast.next) {
        fast = fast.next
        slow = slow.next
    }
    slow.next = slow.next.next
    return preHead.next
};
```

- 判断[链表](https://www.nowcoder.com/jump/super-jump/word?word=链表)是否为回文[链表](https://www.nowcoder.com/jump/super-jump/word?word=链表)

```js
//方法1：
var isPalindrome = function(head) {
  let nums = [];
  while (head){
    nums.push(head.val);
    head = head.next;
  }
  while(nums.length > 1){//注意是大于1  考虑基数个
    if(nums.pop() != nums.shift()) return false;
  }
  return true;
};
//方法2：双指针法/////////////////
var isPalindrome = function(head) {
    let slow=head,fast=head;
    while(fast && fast.next){
        slow = slow.next;
        fast = fast.next.next;
    }
    //反转慢指针 翻转后半段
    let resverse = null;
    while(slow !== null){
        let next = slow.next;
        slow.next = resverse;
        resverse = slow;
        slow = next;
    }
    while(resverse !== null){
        if(resverse.val !== head.val) return false;
        resverse = resverse.next;
        head = head.next;
    } 
    return true;
};
```

- 判断[链表](https://www.nowcoder.com/jump/super-jump/word?word=链表)是否有环

```js
//给定一个链表，判断链表中是否有环。
var hasCycle = function(head) {
    if(!head || !head.next) return false
    let slow = head
    let fast = head.next
    while(slow != fast){
        if(!fast || !fast.next) return false
        fast = fast.next.next
        slow = slow.next
    }
    return true
};
```

- 环形[链表](https://www.nowcoder.com/jump/super-jump/word?word=链表)第一个入环节点

```js
//环形链表第一个入环节点
//给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。
var detectCycle = function(head) {
    if(!head || !head.next) return null
    let slow = head;
    let fast = head;
    while(fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if(slow == fast) {
            fast = head;
            while(slow != fast) {
                slow = slow.next;
                fast = fast.next;
            }
            return slow
        }
    }
    return null
};
```

- 两个[链表]()的第一个公共节点 

​       我们使用两个指针 node1，node2 分别指向两个[链表]() headA，headB 的头结点，然后同时分别逐结点遍历，当 node1 到达[链表]() headA 的末尾时，重新定位到[链表]() headB 的头结点；当 node2 到达[链表]() headB 的末尾时，重新定位到[链表]() headA 的头结点。              

​       这样，当它们相遇时，所指向的结点就是第一个公共结点。

```js
方法3：求出两链表长度，长的先走  差值数量  步，再一起走
方法1：
var getIntersectionNode = function(headA, headB) {//双指针
    let node1 = headA,node2 = headB;
    while(node1!=node2) {
        node1= node1 !== null ? node1.next : headB;
        node2 = node2 !== null ? node2.next : headA;
    }
    return node1;
};
方法二：
var getIntersectionNode = function(headA, headB) {//hashmap法
    let map = new Map();
    while(headA) {
        map.set(headA,true);
        headA = headA.next;
    }
    while(headB) {
        if(map.has(headB)) return headB;
        headB = headB.next;
    }
    return null;
};
```

**复杂：**

- 合并两个[排序](https://www.nowcoder.com/jump/super-jump/word?word=排序)的[链表](https://www.nowcoder.com/jump/super-jump/word?word=链表)★★

```js
function Merge(pHead1, pHead2) {//方法1：递归法  
    let pMergeHead = null;
    if (pHead1 === null) return pHead2;
    if (pHead2 === null) return pHead1;
    if (pHead1.val < pHead2.val) {
        pMergeHead = pHead1;
        pMergeHead.next = Merge(pHead1.next, pHead2);
    } else {
        pMergeHead = pHead2;
        pMergeHead.next = Merge(pHead1, pHead2.next);
    }
    return pMergeHead;
}
////////////////////////////////////////////////////////////////////////////////////////////
var mergeTwoLists = function (pHead1, pHead2) { //方法2：迭代法    
    if (!pHead1) {
        return pHead2;
    } else if (!pHead2) {
        return pHead1;
    }

    let preHead = new ListNode(-1);
    let node = preHead;

    while (pHead1 && pHead2) {
        if (pHead1.val <= pHead2.val) {
            node.next = pHead1;
            pHead1 = pHead1.next;
        } else {
            node.next = pHead2;
            pHead2 = pHead2.next;
        }
        node = node.next;
    }

    if (pHead1) {
        node.next = pHead1;
    } else if (pHead2) {
        node.next = pHead2;
    }

    return preHead.next;
};
```

- 合并K个[排序]()的[链表]()★★ 

  ​                给你一个[链表]()数组，每个[链表]()都已经按升序排列。               

  ​                请你将所有[链表]()合并到一个升序[链表]()中，返回合并后的[链表]()。               

  ​                示例 1：               

  ​                输入：lists = [[1,4,5],[1,3,4],[2,6]]               

  ​                输出：[1,1,2,3,4,4,5,6]               

  ​                解释：[链表]()数组如下：               

  ​                [               

  ​                1->4->5,               

  ​                1->3->4,               

  ​                2->6               

  ​                ]               

  ​                将它们合并到一个有序[链表]()中得到。               

  ​                1->1->2->3->4->4->5->6

```js
方法1：两两合并 逐一合并两条链表
var mergeKLists = function (lists) {
    let mergeTwoLists = (list1, list2) => {
        let preHead = new ListNode(-1)
        let preNode = preHead
        while (list1 && list2) {
            if (list1.val <= list2.val) {
                preNode.next = list1
                list1 = list1.next
            } else {
                preNode.next = list2
                list2 = list2.next
            }
            preNode = preNode.next
        }
        preNode.next = list1 ? list1 : list2
        return preHead.next
    }
    let n = lists.length
    if (n == 0) return null
    let res = lists[0]
    for (let i = 1; i < n; i++) {
        if (lists[i]) {
            res = mergeTwoLists(res, lists[i])
        }
    }
    return res
};

方法2：
思路
    遍历所有链表元素，将元素值放到一个数组中
    排序数组，相当于按优先级排序优先级队列
    遍历排好序的数组，重新构造一个新的有序链表即为所求
var mergeKLists = function(lists) {
if(!lists || lists.length == 0) return null;
let arr = [];
let res = new ListNode(0);
lists.forEach(list => {
    let cur = list;
    while(cur){
        arr.push(cur.val);
        cur = cur.next;
    }
})
let cur = res;
arr.sort((a,b) => a-b).forEach(val => {
    let node = new ListNode(val);
    cur.next = node;
    cur = cur.next;
})
return res.next;
};
```

- 奇偶[链表]()★ 

  ​                给定一个单[链表]()，把所有的奇数节点和偶数节点分别排在一起。请注意，这里的奇数节点和偶数节点指的是节点编号的奇偶性，而不是节点的值的奇偶性。               

  ​                示例 1:               

  ​                输入: 1->2->3->4->5->NULL               

  ​                输出: 1->3->5->2->4->NULL               

  ​                **解题思路**               

  ​                有[链表]()1-2-3-4-5               

  ​                对于奇数，我们其实只要改变奇数的下一个指向，指到下下个，如1改为指向3即可，也就是 node.next = node.next.next;               

  ​                对于偶数，也是一样的node.next = node.next.next               

  ​                遍历完成后，我们就得到两条[链表]()，一条是全奇数节点，一条是全偶数节点。               

  ​                然后链接两段[链表]()，               奇数链.next = 偶数链头即可               

  ​                最后返回奇数链头

```js
var oddEvenList = function(head) {
    if(!head){return head;}
    
    //储存偶数头
    let doubleHead = head.next;
    
    //偶数链
    let double = head.next;
    //奇数链
    let single = head;
    while(single.next&&single.next.next){
        single.next = single.next.next;
        double.next = double.next.next;
        //赋值需要放在后面一起赋值，不然double.next.next会被更改
        single = single.next;
        double = double.next;
    }
    //链接两条链表
    single.next = doubleHead;
    return head;
};
```

- 复制带随机指针的单向[链表](https://www.nowcoder.com/jump/super-jump/word?word=链表)★

```js
方法1：
用一个哈希表表示映射关系：键是原节点，值是复制的节点。

整体算法流程是：
    第一次遍历，复制每个节点和 next 指针，并且保存“原节点-复制节点”的映射关系
    第二次遍历，通过哈希表获得节点对应的复制节点，更新 random 指针

代码实现
使用 ES6 的Map，可以直接将对象作为键值。
var copyRandomList = function(head) {
    if (!head) {
        return null;
    }
    const map = new Map();
    let node = head; // 当前节点
    const newHead = new Node(node.val);
    let newNode = newHead; // 当前节点的copy
    map.set(node, newNode);

    while (node.next) {
        newNode.next = new Node(node.next.val);
        node = node.next;
        newNode = newNode.next;
        map.set(node, newNode);
    }

    newNode = newHead;
    node = head;
    while (newNode) {
        newNode.random = map.get(node.random);
        newNode = newNode.next;
        node = node.next;
    }

    return newHead;
};

方法二：
解题思路
原地复制，再拆分链表。
var copyRandomList = function(head) {
if (head == null) {
        return head;
    }
    //将拷贝节点放到原节点后面，例如1->2->3这样的链表就变成了这样1->1'->2'->3->3'
    for (let node = head, copy = null; node != null; node = node.next.next) {
        copy = new Node(node.val);
        copy.next = node.next;
        node.next = copy;
    }
    //把拷贝节点的random指针安排上
    for (let node = head; node != null; node = node.next.next) {
        if (node.random != null) {
            node.next.random = node.random.next;
        }
    }
    //分离拷贝节点和原节点，变成1->2->3和1'->2'->3'两个链表，后者就是答案
    let newHead = head.next;
    for (let node = head, temp = null; node != null && node.next != null;) {
        temp = node.next;
        node.next = temp.next;
        node = temp;
    }

    return newHead;
};
```

- 有序[链表](https://www.nowcoder.com/jump/super-jump/word?word=链表)转换二叉搜索树（BST）★

```js
//自上而下构建树，先找链表的中点
//使用数组，注意转换时的细节
var sortedListToBST = function(head) {
    if(head === null) {
        return null
    }
    const nodes = []
    while(head.next) {
        nodes.push(head.val)
        head=head.next
    }
    nodes.push(head.val)

    const buildBST = (start, end)=>{
        if(start > end) {
            return null
        }
        const mid = Math.ceil(start + (end - start) / 2) 
        //向上取整
        const root = new TreeNode(nodes[mid])
        root.left = buildBST(start, mid-1)
        root.right = buildBST(mid+1, end)
        return root
    }

    return buildBST(0, nodes.length-1)
};
```

- [二叉树](https://www.nowcoder.com/jump/super-jump/word?word=二叉树)展开为[链表](https://www.nowcoder.com/jump/super-jump/word?word=链表)★

```js
思路：先序遍历递归，再转换
例如，给定二叉树
    1
   / \
  2   5
 / \   \
3   4   6

将其展开为：
1
 \
  2
   \
    3
     \
      4
       \
        5
         \
          6
var flatten = function (root) {
    const helper = (root) => {//先序遍历
        if (!root) {
            return
        }
        res.push(root)
        helper(root.left)
        helper(root.right)
    }
    let res = []
    helper(root)
    for (let i = 0; i < res.length - 1; i++) {
        res[i].left = null//很关键，左子树均为空，类似链表
        res[i].right = res[i + 1]
    }
    return res;
};
```

- K 个一组翻转[链表](https://www.nowcoder.com/jump/super-jump/word?word=链表)★

```js
给你一个链表，每 k 个节点一组进行翻转，请你返回翻转后的链表。
k 是一个正整数，它的值小于或等于链表的长度。
如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。
示例：
给你这个链表：1->2->3->4->5
当 k = 2 时，应当返回: 2->1->4->3->5
当 k = 3 时，应当返回: 3->2->1->4->5
var reverseKGroup = function(head, k) {
  // 标兵
  let dummy = new ListNode()
  dummy.next = head
  let [start, end] = [dummy, dummy.next]
  let count = 0
  while(end) {
    count++
    if (count % k === 0) {
      start = reverseList(start, end.next)
      end = start.next
    } else {
      end = end.next
    }
  }
  return dummy.next

  // 翻转stat -> end的链表
  function reverseList(start, end) {
    let [pre, cur] = [start, start.next]
    const first = cur
    while(cur !== end) {
      let next = cur.next
      cur.next = pre
      pre = cur
      cur = next
    }
    start.next = pre
    first.next = cur
    return first
  }
};
```

- [二叉树](https://www.nowcoder.com/jump/super-jump/word?word=二叉树)各种遍历（前中后序遍历，递归非递归，DFS,BFS）★★★

```js
function inorder(root) {//中序非递归   BST第K小的数   第K大见下面
   if (!root) return null;
    var stack = [];
    var p = root;
    //var pre=-Infinity;
    while (stack.length > 0 || p) {
        if (p) { //当前非空，当前入栈，左移
            stack.push(p);
            p = p.left;
        } else { //栈弹出，并右移
            p = stack.pop();
            console.log(p.value);//在此和前一个数比较 判断是否为二叉搜索树 
            p = p.right;
        }
    }
}
function BFS(root) { //广度优先遍历(层序)
    var queue = [];
    queue.push(root); // 先进先出
    while (queue) {
        var temp = queue.shift();
        console.log(temp.value);
        if (temp.left != null)
            queue.push(temp.left);
        if (temp.right != null)
            queue.push(temp.right);
    }
}
function pre(root) {//先序非递归 中左右
    var stack = [];
    stack.push(root);
    while (stack) {
        // 移除最后一个
        var temp = stack.pop();
        console.log(temp.value);
        // 后进先出
        if (temp.right != null)
            stack.push(temp.right);
        if (temp.left != null)
            stack.push(temp.left);
    }
}
function pos(root) {//后序非递归 中右左=>左右中  
    var stack1 = [];
    var stack2 = [];
    stack1.push(root);
    while (stack1) {
        var temp = stack1.pop();
        stack2.push(temp);
        if (temp.left != null)
            stack1.push(temp.left);
        if (temp.right != null)
            stack1.push(temp.right);
    }
    while (stack2) {
        console.log(stack2.pop().value);
    }
}
var preOrder = function (node) { //先序遍历  递归版(中序和后序修改打印位置即可)
    if (node) {
        console.log(node.value); //arr.push(node.val)
        preOrder(node.left);
        preOrder(node.right);
    }
}
```

- [二叉树](https://www.nowcoder.com/jump/super-jump/word?word=二叉树)遍历涉及到的一些[算法题](https://www.nowcoder.com/jump/super-jump/word?word=算法题)

  （好多题其实就是[二叉树](https://www.nowcoder.com/jump/super-jump/word?word=二叉树)深度或者广度非递归遍历稍微改一下即可）

  - 前序和中序[重建二叉树](https://www.nowcoder.com/jump/super-jump/word?word=重建二叉树)★★

  ```js
  function reConstructBinaryTree(pre, vin)
  {
  if (pre.length == 0 || vin.length == 0) {
      return null;
  };
  var index = vin.indexOf(pre[0]);
  var left = vin.slice(0, index);
  var right = vin.slice(index + 1);
  var node = new TreeNode(vin[index]);
  node.left = reConstructBinaryTree(pre.slice(1, index + 1), left);
  node.right = reConstructBinaryTree(pre.slice(index + 1), right);
  return node;
  }
  ```

  - BST第K大的数和第K小的数★

  ```js
  function kmax(root,k) {//逆中序 右中左 第k大的数
  //也可以用递归中序放进一个数组
      if (!root) return null;
      var stack = [];
      var count=0;
      var p = root;
      while (stack.length > 0 || p) {
          if (p) { //当前非空，当前入栈，右移
              stack.push(p);
              p = p.right;
          } else { //栈弹出，并左移
              p = stack.pop();
              if (++count == k) {
                  return p.value;
              }
              p = p.left;
          }
      }
  }
  ```

  - [二叉树](https://www.nowcoder.com/jump/super-jump/word?word=二叉树)按层求和（层序遍历改进）★

  ```js
  function sum(pNode) {
  let output = [];
  if (pNode.left === null && pNode.right === null) {
      output.push(pNode.value);
      return output;
  }
  let storeArr = [];
  storeArr.push(pNode);
  while (storeArr.length > 0) {
      let tempSum = 0;
      let len = storeArr.length;
      for (let j = 0; j < len; j++) {//1,2,4个
          let tempNode = storeArr.shift();
          tempSum += parseInt(tempNode.value);
          if (tempNode.left !== null) {
              storeArr.push(tempNode.left);
          }
          if (tempNode.right !== null) {
              storeArr.push(tempNode.right);
          }
      }
      output.push(tempSum);
  }
  return output;
  }
  ```

  - Z字型（之字形）遍历[二叉树](https://www.nowcoder.com/jump/super-jump/word?word=二叉树)★

  ```js
  function Print(pRoot) {
  var lists = [];
  if (pRoot === null) {
      return lists;
  }
  var stack1 = [];
  var stack2 = [];
  stack2.push(pRoot);
  var i = 1;
  while (stack1.length !== 0 || stack2.length !== 0) {
      var list = [];
      // 为奇数层 从左至右
      if ((i & 1) === 1) {
          while (stack2.length !== 0) {
              var tmp = stack2.pop();
              list.push(tmp.val);
              if (tmp.left !== null) stack1.push(tmp.left);
              if (tmp.right !== null) stack1.push(tmp.right);
          }
      }
      // 为偶数层 从右至左
      else {
          while (stack1.length !== 0) {
              var tmp = stack1.pop();
              list.push(tmp.val);
              if (tmp.right !== null) stack2.push(tmp.right);
              if (tmp.left !== null) stack2.push(tmp.left);
          }
      }
      i++;
      lists.push(list);//可以用for循环将list逐个push进lists
  }
  return lists;
  }
  ```

- [二叉树](https://www.nowcoder.com/jump/super-jump/word?word=二叉树)深度相关

  - [二叉树](https://www.nowcoder.com/jump/super-jump/word?word=二叉树)深度★★★

  ```js
  function TreeDepth(pRoot) {
    //树的深度=左子树的深度和右子树深度中最大者+1
    if (pRoot === null) return 0;
    var leftDep = TreeDepth(pRoot.left);
    var rightDep = TreeDepth(pRoot.right);
    return Math.max(leftDep, rightDep) + 1;
  }
  ```

  - [二叉树](https://www.nowcoder.com/jump/super-jump/word?word=二叉树)最小深度★★

  ```js
  最小深度是从根节点到最近叶子节点的最短路径上的节点数量。
  var minDepth = function (root) {
    if (root == null) {
      return 0;
    }
    if (root.left == null && root.right == null) {
      return 1;
    }
    let ans = Infinity;
    if (root.left != null) {
      ans = Math.min(minDepth(root.left), ans);
    }
    if (root.right != null) {
      ans = Math.min(minDepth(root.right), ans);
    }
    return ans + 1;
  };
  ```

  - 树找两(叶子)节点最长距离（相隔的最长路径）★

  ```js
  我们要求的二叉树的最大距离其实就是求：肯定是某个节点左子树的高度加上右子树的高度加2，所以求出每个节点左子树和右子树的高度，取左右子树高度之和加2的最大值即可，假设空节点的高度为-1
  function TreeDepth(pNode,nMaxDistance) {
      if (pNode == null)
          return -1; //空节点的高度为-1
      //递归
      var leftDep  = TreeDepth(pNode.left, nMaxDistance) + 1; //左子树的的高度加1
      var rightDep  = TreeDepth(pNode.left, nMaxDistance) + 1; //右子树的高度加1
      var nDistance = leftDep  + rightDep ; //距离等于左子树的高度加上右子树的高度+2
      nMaxDistance = nMaxDistance > nDistance ? nMaxDistance : nDistance; //得到距离的最大值
      return leftDep  > rightDep  ? leftDep  : rightDep ;
  } //nMaxDistance为所求
  ```

  - 判断[二叉树](https://www.nowcoder.com/jump/super-jump/word?word=二叉树)是否为[平衡二叉树](https://www.nowcoder.com/jump/super-jump/word?word=平衡二叉树)★★

  ```js
  function IsBalanced(pRoot) { //方法1：节点重复遍历了，影响效率了
      if (pRoot == null) return true;
      let leftLen = TreeDepth(pRoot.left);
      let rightLen = TreeDepth(pRoot.right);
      return Math.abs(rightLen - leftLen) <= 1 && IsBalanced(pRoot.left) && IsBalanced(pRoot.right);
      //左右子树均平衡，且左右子树高度差不超过1
  }
  
  function TreeDepth(pRoot) {
      if (pRoot == null) return 0;
      let leftLen = TreeDepth(pRoot.left);
      let rightLen = TreeDepth(pRoot.right);
      return Math.max(leftLen, rightLen) + 1;
  }
  
  方法2：
  //改进办法就是在求高度的同时判断是否平衡，如果不平衡就返回-1，否则返回树的高度。
  //并且当左子树高度为-1时，就没必要去求右子树的高度了，可以直接一路返回到最上层了
   function IsBalanced(pRoot) {
      return TreeDepth(pRoot) !== -1;
  }
  
  function TreeDepth(pRoot) {
      if (pRoot === null) return 0;
      const leftLen = TreeDepth(pRoot.left);
      if (leftLen === -1) return -1;//当左子树高度为-1时，就没必要去求rightLen，可以直接一路返回到最上层了
      const rightLen = TreeDepth(pRoot.right);
      if (rightLen === -1) return -1;
      return Math.abs(leftLen - rightLen) > 1 ? -1 : Math.max(leftLen, rightLen) + 1;
  } 
  ```

  - [二叉树](https://www.nowcoder.com/jump/super-jump/word?word=二叉树)右视图（左）★

  ```js
  方法一：DFS
  var rightSideView = function(root) {
    if(!root) return []
    let arr = []
    dfs(root, 0, arr)
    return arr
  };
  function dfs (root, step, res) {
    if(root){
      if(res.length === step){
        res.push(root.val)           
  // 当数组长度等于当前 深度 时, 把当前的值加入数组
      }
      dfs(root.right, step + 1, res) 
  // 先从右边开始, 当右边没了, 再轮到左边
      dfs(root.left, step + 1, res)
    }
  }
  ```

- [二叉树](https://www.nowcoder.com/jump/super-jump/word?word=二叉树)路径相关

  - 路径总和1★★★

  ```js
  给定一个二叉树和一个目标和，判断该树中是否存在根节点到叶子节点的路径，这条路径上所有节点值相加等于目标和。存在返回true 否则false
  var hasPathSum = function (root, sum) {
    // 根节点为空
    if (root === null) return false;
    // 叶节点 同时 sum 参数等于叶节点值
    if (root.left === null && root.right === null) return root.val === sum;
    // 总和减去当前值，并递归
    sum = sum - root.val
    return hasPathSum(root.left, sum) || hasPathSum(root.right, sum);
  };
  ```

  - 路径总和2（回溯法）★

  ```js
  按字典序打印出二叉树中结点值的和为输入整数的所有路径。 回溯法（DFS）
  function TreeNode(val) {
      this.val = val;
      this.left = this.right = null;
  }
  
   function FindPath(root, expectNumber) {
     //深度遍历整个树  但是做不到 在返回值的list中，数组长度大的数组靠前啊啊啊啊啊！！！！！！
     const path = [],
       result = [];
     return findpath(root, expectNumber, path, result);
   }
  
   function findpath(root, expectNumber, path, result) {
     if (root === null) {
       return result;
     }
     path.push(root.val);
     var x = expectNumber - root.val;
     if (root.left === null && root.right === null && x === 0) {
       result.push(path.slice(0)); //result.push(Array.of(...path)) 拷贝path
     }
     findpath(root.left, x, path, result);
     findpath(root.right, x, path, result);
     path.pop(); //在遍历完之后需要把最后一颗节点弹出来 来返回上一层
     return result;
   }
  ```

- DP

  - [斐波那契数列](https://www.nowcoder.com/jump/super-jump/word?word=斐波那契数列)★★★

  ```js
  function Fibonacci(n) {
      //DP问题
      var a = 0,
          b = 1,
          c;
      if (n <= 1) {
          return n;
      }
      for (var i = 2; i <= n; i++) {
          c = a + b;
          a = b;
          b = c;
      }
      return c;
  }
  // 递归法
   function Fibonacci(n) {
      if(n<2){
          return n;
      }else{
          return Fibonacci(n - 1) + Fibonacci(n-2);
      }
  } 
  
   function Fibonacci(n, a, b) {//尾递归 不爆栈
    if (n <= 1) {
      return a;
    }
    return Fibonacci(n - 1, b, a + b)
  }
  ```

  - [最长公共子序列](https://www.nowcoder.com/jump/super-jump/word?word=最长公共子序列) LCS ★★

  ```js
  var longestCommonSubsequence = function (text1, text2) {
      let n = text1.length;
      let m = text2.length;
      let dp = Array(n + 1).fill(0).map(() => new Array(m + 1).fill(0)); //多一个第0行 第0行和0列均为0 n*m
      for (let i = 1; i <= n; i++) {
          for (let j = 1; j <= m; j++) {
              if (text1[i - 1] == text2[j - 1]) {
                  dp[i][j] = dp[i - 1][j - 1] + 1;
              } else {
                  dp[i][j] = Math.max(dp[i][j - 1], dp[i - 1][j]);
              }
          }
      }
      return dp[n][m];
  };
  ```

  - 最长上升子序列 ★★

  ```js
  function LISbyDP(arr) {
      if (arr.length <= 1) {
          return arr.length;
      }
      let maxLen = 0;
      let fs = [];
      fs[0] = 1;
      for (let i = 1; i < arr.length; i++) {
          fs[i] = 1; //初始化为1  很关键 子序列最小也要包括自己
          for (let j = 0; j < i; j++) {
              if (arr[j] < arr[i]) {
                  fs[i] = Math.max(fs[j] + 1, fs[i]); //fs[i]表示到以arr[i]结尾的最长递增子序列的长度
              }
          }
      }
      maxLen = Math.max.apply(null, fs); //计算出以每一个数结尾的最长递增子串数 求数组最大值
      return maxLen;
  }
  let arr = [1, 5, 3, 4, 6, 9, 7, 8];
  console.log(LISbyDP(arr));//6
  ```

  - 连续子数组（子串）的最大和★★

  ```js
  输入: nums = [-2,1,-3,4,-1,2,1,-5,4]
  输出: 6
  解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
  var maxSubArray = function(nums) {
  for(var i = 1; i < nums.length; i++){
      if(nums[i - 1] > 0){
          nums[i] += nums[i - 1];
      }
  }
  return Math.max(...nums)
  };
  //DP法
  var maxSubArray = function (nums) {
  if (!nums.length) return null
  let max = nums[0], record = nums[0];
  for (let i = 1; i < nums.length; i++) {
      record = Math.max(record + nums[i], nums[i]);
      if (record > max) max = record;
  }
  return max
  };
  ```

  - 硬币找零（最少硬币个数）★★

  ```js
  var coinChange = function (coins, amount) {
      let dp = new Array(amount + 1).fill(Infinity);
      dp[0] = 0;
  
      for (let i = 1; i <= amount; i++) {
          for (let coin of coins) {
              if (i - coin >= 0) {
                  dp[i] = Math.min(dp[i], dp[i - coin] + 1);
              }
          }
      }
  
      return dp[amount] === Infinity ? -1 : dp[amount];
  }
  ```

  - 0-1背包问题★

  ```js
   * @param {*} capacity 允许负重大小
   * @param {*} wt 重量数组
   * @param {*} val 价值数组
   * @param {*} n 几件物品 val数组的长度
  function knapSack (capacity, wt, val, n) {
      var i, w, a, b, dp = [];
      for (let i = 0; i <= n; i++) {
          dp[i] = [];    
      }
  
      for (let j = 0; j <= n; j++) { //dp[i][w]表示：对于前i个物品，当前背包容量为w时，这种情况下可以装下的最大价值
          for (let w = 0; w <= capacity; w++) { //从0开始计数
              if (j == 0 || w == 0) {
                  dp[j][w] = 0;
              } else if (wt[j-1] <= w) {
                  a = val[j - 1] + dp[j - 1][w - wt[j - 1]]; // 第j个放 wt[j - 1]表示第j个物品的重量
                  b = dp[j - 1][w]; //  第j个不放
                  dp[j][w] = (a > b) ? a : b; // max(a,b)
              } else {
                  dp[j][w] = dp[j-1][w];
              }
          }
      }
      return dp[n][capacity];
  }
  ```

  - 完全背包问题（了解即可）★

  ```js
  var val = [3, 4, 4, 6, 5];
  var wt = [3, 5, 3, 4, 3];
  var capacity = 13;
  
  function all(a, b, n, m) { //重量 价值 物品种类数 总容量
      var f = [];
      for (let i = 0; i <= n; i++) {
          f[i] = [];
      }
      for (var i = 0; i <= n; i++) {
          for (var j = 0; j <= m; j++) {
              if (i == 0 || j == 0) {
                  f[i][j] = 0;
              } else {
                  f[i][j] = f[i - 1][j]; //不取第i件物品
                  for (var k = 1; k * a[i - 1] <= j; k++) { //取第i件物品，且第i件物品可以取k个  第i件物品数组下标为i-1
                      f[i][j] = Math.max(f[i][j], f[i - 1][j - k * a[i - 1]] + k * b[i - 1]);
                  }
              }
  
          }
      }
      return f[n][m];
  }
  console.log(all(wt, val, val.length, capacity));
  ```

- 全排列（回溯法）见LeetCode 全排列1、全排列2 ★



## **CSS**

1.CSS画各种图形（等腰三角形、等腰梯形、扇形、圆、半圆）  ★

```js
<div class="a"></div>
//等腰三角形
.a{
width: 0px;
height: 0px;
border-bottom: 50px solid red;
border-left: 50px solid transparent;
border-right: 50px solid transparent;
}

//等腰梯形
.a{
width: 50px;
height: 0px;
border-bottom: 50px solid red;
border-left: 50px solid transparent;
border-right: 50px solid transparent;
}

//扇形
.a{
width: 0px;
height: 0px;
border-bottom: 50px solid red;
border-left: 50px solid transparent;
border-right: 50px solid transparent;
border-radius: 50%;
}

//圆
.a{
width: 50px;
height: 50px;
background-color: blue;
border-radius: 25px;//50% 边框半径为宽高的50%
}
半圆
.a{
width: 100px;
height: 50px;
background-color: blue;
border-top-left-radius: 50px;
border-top-right-radius: 50px;
/* border-bottom-left-radius: 50px; */
}
```

2.三列布局（至少掌握三种方法）  ★★★

3.垂直水平居中（至少掌握三种方法）  ★★★