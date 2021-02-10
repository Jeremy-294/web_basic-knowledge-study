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



### 16. DOM常见的操作方式

### 17. Array.sort()方法与实现机制

### 18. Ajax的请求过程

### 19. JS的垃圾回收机制

### 20.  JS中的String、Array和Math方法