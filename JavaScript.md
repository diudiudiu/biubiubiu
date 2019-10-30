# JavaScript

以下是我对 JavaScript 学习和遇到问题时候所记录和理解的。

## 数据类型

分为基本类型（又称原始类型、基本数据类型）和 引用类型。

基本类型：undefined，null，boolean，number，string，symbol(ES6)

引用类型：Object

基本类型又包括基本包装类型（String，Number，Boolean）后面会讲到。

引用类型中包括内置对象 （Object，Array，String，Date，Math,regex）

## 基本包装类型(String Number Boolean)

```javascript
var str = 'hello'; //string 是 基本类型
var str2 = str.charAt(0); //基本类型调用方，会执行以下操作
{
 var _str = new String('hello'); // 1.首先通过包装对象创建出一个和基本类型值相同的对象
 var str2 = _str.chaAt(0); // 2 然后包装对象下的方法，并且返回结果给str2
 _str = null;  // 3 最后临时创建的对象被销毁
} 
```

引用类型和基本包装类型区别在于: 生存期。

引用类型所创建的对象，在执行的期间一直在内存中，而基本包装对象只是存在了一瞬间，我们无法直接给基本类型添加方法。

另一种说法：本身是基本类型，但是在执行代码的过程中，如果这种类型调用了属性或方法，那么这种类型就不再是基本类型，而是基本包装类型，这个遍历也不是普通变量，而是基本包装类型对象。

用一段代码来举例：

```javascript
var str = 'hello';
str.number = 10; //假设我们想给字符串添加一个属性number ，后台会有如下步骤
｛
 var _str = new String('hello'); // 1 首先通过包装对象创建出一个和基本类型值相同的对象
  _str.number = 10; // 2 然后包装对象下的方法，但结果并没有被任何东西保存
 _str =null; // 3 最后临时对象被销毁
 ｝
alert(str.number); //undefined  当执行到这一句的时候，又会重新重复上面的步骤
｛
 var _str = new String('hello'); // 1 首先通过包装对象创建出一个和基本类型值相同的对象
  _str.number = 10; // 2 然后包装对象下的方法，但结果并没有被任何东西保存
 _str =null; // 3 最后临时对象被销毁
 ｝
```

而引用类型则会保存。





###  内置对象

### Object

`Object.assign(newobj,obj1,obj2....) `拷贝对象自身可枚举属性到目标对象（newobj）

1. 浅拷贝  
2. 继承和不可枚举属性不可拷贝
3. 原始类型会被包装为对象（只有字符串的包装对象才可能有自身可枚举属性）
4. 异常会打断后续拷贝任务

`Object.create()` 创建一个新对象，使用现有的对象来提供新对象的原型。

```javascript
//一个参数传入的是目标
o = Object.create(null)
o2 = Object.create({}, {
  p: {
    value: 42, 
    writable: true,
    enumerable: true,
    configurable: true 
  } 
})
```

数据描述符 （属性描述符）

- Configurable 是否能通过delete删除此属性，能否修改属性的特性，或修改访问器属性，字面量定义对象，默认为true
- Enumerables 是否可枚举，for-in或Object.keys()返回，字面量定义，默认为true
- Writable 是否修改属性的值，字面量定义，默认为true
- Value 属性对应的值，默认为undefinde

访问器属性（存取描述符）

-  Configurable 同数据属性的 Configurable一样， 是否能通过delete删除此属性，能否修改属性的特性，或修改访问器属性，字面量定义对象，默认为true
-  Enumerables 同数据属性的 Enumerables 一样，是否可枚举，for-in或Object.keys()返回，字面量定义，默认为true
-  Get 给属性提供的 getter 的方法，如果没有 getter 则为 undefined。
-  Set 给属性体统 Setter 的方法，该方法接受**唯一参数**，如果没有 setter 则为 undefined。

`Object.defineProperty(obj, prop, descriptor) ` 直接在一个对象上定义一个新属性，或者修改一个对象的现有属性， 并返回这个对象。如果不指定configurable, writable, enumerable ，则这些属性默认值为false，如果不指定value, get, set，则这些属性默认值为undefined

`Object.defineProperties(obj, props)`  直接在一个对象上定义一个或多个新的属性或修改现有属性，并返回该对象。

```javascript
Object.defineProperty(obj, 'name', {
    configurable: false,
    writable: true,
    enumerable: true,
    value: '张三'
})
Object.defineProperties(obj, {
    name: {
        value: '张三',
        configurable: false,
        writable: true,
        enumerable: true
    },
    age: {
        value: 18,
        configurable: true
    }
})
```

`Object.entries(obj)` 返回 obj 可枚举字符串的[key,value] 数组，与for-in 不同的是此函数只遍历**自身**属性。

可通过该方法将obj转为Map`let map = new Map(Object.entries(obj))`

`Object.freeze(obj)` 冻结obj 类似于const声明obj ,**浅冻结**，如果属性是的值是一个对象的话可以改变对象属性的值，字符串，数字和布尔值总是不可变的。

`Object.fromEntries(iterable(可迭代))` 将键值对列表转换为对象

`Object.getOwnPropertyDesciptor(obj, prop)` 返回 obj 上 prop 属性的属性描述符

`Object.getOwnPropertyDesciptors(obj)` 返回 obj 上**自身**所有属性的属性描述符

`Object.getOwnPropertyNames(obj)`  返回 obj 上的所有除Symbol以外**自身**属性的数组，包括不可枚举属性。

`Object.getOwnPropertySymbols()` 返回 obj 上**自身**的所有符号属性的数组。

`Object.getPrototypeOf(obj)` 返回 obj 的原型对象

`Object.seal()` 会创建一个密封的对象，这个方法实际上会在一个现有对象上调用Object.preventExtensions(...)并把所有现有属性标记为 configurable:false

#### Object practice

```javascript
//实现一个函数拷贝所有自有属性的属性描述符
function completAssign (target, ...sources) {
    
}
//ES5实现assign
function assign () {
    
}
//混入的方式实现多继承
function extendsMixin () {
    
}
```



## 深拷贝与浅拷贝

### 浅拷贝

浅拷贝是拷贝引用，拷贝的引用都是指向同一个对象的实例，彼此之间的操作会互相影响。

浅拷贝分两种情况：

- 直接拷贝源对象的引用。

  ```javascript
  var a = {name: '小明'} 
  var b = a 
  console.log(a === b) //true
  a.name = 'abcd';
  console.log(b.name) //'abcd'
  ```

- 源对象拷贝实例，但其属性拷贝引用。

  ```javascript
  var a = [{name: '小明'}, {age: 18}];
  var b = a.slice();
  console.log(a === b); // false,外层数组拷贝的是实例
  a[0].age = 20;
  console.log(b[0].age); // 20 元素拷贝的是引用
  ```

### 深拷贝

深拷贝在堆中重新分配内存，并且把源对象所有属性都进行新建拷贝，以保证深拷贝的对象的引用图不包含任何原有对象或对象图上的任何对象，拷贝后的对象与原来的对象是完全隔离，互不影响。

较为简单的方法通过 `JSON.stringfy()` 和 `JSON.parse()` ，但是 undefined、function、symbol 会在转换过程中被忽略。

#### 运用递归的方法进行深拷贝

```javascript
function deepCopy(obj) {
  var result = Array.isArray(obj) ? [] : {}
  for (var key in obj) {
    if (obj.hasOwnProperty(key)) {
      if (typeof obj[key] === 'object' && obj[key]!==null) {
        result[key] = deepCopy(obj[key])   //递归复制
      } else {
        result[key] = obj[key]
      }
    }
  }
  return result
}
```

### 继承

#### ES5继承

- 对象冒充继承（有弊端）

  使用.call()  方法进行继承，可以继承构造函数里面的属性和方法，但是无法继承原型链上面的属性和方法。

- 原型链继承（有弊端）

  `子类.prototype = new 父类（）`

  可以继承构造函数里面的属性和方法也可以继承原型链上面的属性和方法。

  但是实例化子类没法给父类传参数。

- 对象冒充+原型链（完美继承）

  ```javascript
  function Rectangle(length, width) {
    this.length = length
    this.width = width;
  }
  Rectangle.prototype.getArea = function() { 
  	return this.length * this.width
  };
  function Square(length) { 
  	Rectangle.call(this, length, length)
  }
  Square.prototype = Object.create(Rectangle.prototype, { 
    constructor: {
  		value:Square,
      enumerable: true,
      writable: true,
      configurable: true
    } 
  })
  var square = new Square(3)
  console.log(square.getArea())
  console.log(square instanceof Square)
  console.log(square instanceof Rectangle)
  // 9
  // true
  // true
  ```

原型链上的属性会被多个实例共享，构造函数中不会。

#### ES6继承

ES6中引入了 class 类的概念。

```javascript
class Rectangle {
  constructor(length, width) {
  	this.length = length
  	this.width = width
  } 
  getArea() {
    return this.length * this.width
  }
} 

class Square extends Rectangle {
	constructor(length) {
		// 与 Rectangle.call(this, length, length) 相同 
		super(length, length)
	}
}
var square = new Square(3)
console.log(square.getArea())
console.log(square instanceof Square)
console.log(square instanceof Rectangle)
// 9
// true
// true
```

继承了其他类的类被称为派生类。如果派生类指定了构造器，就需要使用 super() ，否则会造成错误。若你选择不使用构造器， super() 方法会被自动调用， 并会使用创建新实例时提供的所有参数。

使用 super() 时需牢记以下几点: 

1. 你只能在派生类中使用 super() 。若尝试在非派生的类(即:没有使用 extends 关键字的类)或函数中使用它，就会抛出错误。 
2. 在构造器中，你必须在访问 this 之前调用 super() 。由于 super() 负责初始化 this ，因此试图先访问 this 自然就会造成错误。 
3. 唯一能避免调用 super() 的办法，是从类构造器中返回一个对象。 

#### 多继承（ mixin 混入）

JavaScript是单继承语言，多继承需要 mixin 来把需要继承的父类的所有属性都拷贝到子类上。

```javascript
let SerializableMixin = {
	serialize() {
		return JSON.stringify(this)
  }
}
let AreaMixin = {
	getArea() {
		return this.length * this.width
  }
}
function mixin(...mixins) {
	var base = function() {}
  Object.assign(base.prototype, ...mixins)
  return base
}
class Square extends mixin(AreaMixin, SerializableMixin) {
  constructor(length) {
  	super();
		this.length = length
    this.width = length
	} 
}

var x = new Square(3)
console.log(x.getArea())
console.log(x.serialize());
// 9
// "{"length":3,"width":3}"
```



## 事件

### DOMContentLoaded 与 load 事件

DOMContentLoaded:当初始的 HTML 文档被完全加载和解析完成之后，DOMContentLoaded 事件被触发，而无需等待样式表、图像和子框架的完成加载。

load:当一个资源及其依赖资源已完成加载时，将触发load事件。

首屏时间: “计算这个网页从空白到出现内容所花费的时间”。

这段时间其实就是HTML 文档加载和解析的时间。也就是DOMContentLoaded 事件触发之前所经历的时间。

对于首屏时间而言，js放在HTML文档的开头和结尾处效果是一样的而js放在结尾的目的并不是为了减少首屏时间，而是由于js经常需要操纵DOM，放在后面才更能保证找到DOM节点。

async、defer与DOMContentLoaded详细关系请看[这里](https://www.cnblogs.com/Bonnie3449/p/8419609.html)

### onmouseout 与 onmouseleave 离开元素事件

- onmouseout 离开当前**DOM元素或其子元素**时候触发
- onmouseleave 离开当前**绑定的DOM元素**时候触发

### 事件委托

1. 管理的函数变少了。不需要为每个元素都添加监听函数。对于同一个父节点下面类似的子元素，可以通过委托给父元素的监听函数来处理事件。
2. 可以方便地动态添加和修改元素，不需要因为元素的改动而修改事件绑定。
3. JavaScript 和 DOM 节点之间的关联变少了，这样也就减少了因循环引用而带来的内存泄漏发生的概率。

Event对象提供了一个属性叫target，可以返回事件的目标节点，我们成为事件源，也就是说，target就可以表示为当前的事件操作的DOM，但不是真正操作DOM。
标准浏览器用ev.target，IE浏览器用event.srcElement，此时只是获取了当前节点的位置，并不知道是什么节点名称，接下来用nodeName来获取具体的标签名。这样每次操作只执行一次DOM操作。

### JavaScript的执行机制

js是**单线程语言**，JavaScript版的"多线程"都是用单线程模拟出来的。

宏任务：setTimeOut setInterval 

微任务：promise.then promise.catch

执行顺序为宏任务->执行结束->是否有可执行的微任务

- 没有 开始新的宏任务
- 有   执行所有微任务 ->开始新的宏任务

## Promise 

#### Promise 状态：

- pending:   初始状态
- fulfilled:     成功
- rejected:    失败

.then(回调函数)

## async/await 处理异步

### async 函数

使用 async 声明一个函数 ，表示是一个异步的函数。如果 async 函数中有返回一个值，当调用该函数时，内部会调用 Promise.solve() 把它转化为一个 promise 对象作为返回，如果内部抛出错误，就会调用 Promise.reject() 作为返回的 promise 对象。

### await

await 后面可以放任何表达式，一般放的是一个返回的 promise 对象表达式，await 只能放到 async 函数里面。

当函数执行的时候，一旦遇到 await 就会先返回，等到触发的异步操作完成，再接着执行函数体内后面的语句。

```javascript

//正常使用 promise 进行回调
function getFaceResult () {
    this.getLocation(this.phoneNum)
        .then(res => {
            if (res.status === 200 && res.data.success) {
                let province = res.data.obj.province;
                let city = res.data.obj.city;
                this.getFaceList(province, city)
                    .then(res => {
                    if(res.status === 200 && res.data.success) {
                        this.faceList = res.data.obj
                    }
                })
            }
        })
        .catch(err => {
            console.log(err)
        })
}
//使用 await 
async getFaceResult () {
    try {
        let location = await this.getLocation(this.phoneNum);
        if (location.data.success) {
            let province = location.data.obj.province;
            let city = location.data.obj.city;
            let result = await this.getFaceList(province, city);
            if (result.data.success) {
                this.faceList = result.data.obj;
            }
        }
    } catch(err) {
        console.log(err);
    }
}
```



工作中遇到的问题：两种资源要加载（公共资源，第三方资源），其中第三方资源又分为2种（第三方A资源，第三方B资源）。

需求：第三方资源要在公共资源之前加载，且无论第三方资源是否加载成功都要加载公共资源。第三方A资源要在B资源前加载，且无论A资源与B资源哪个失败都互不阻塞。A资源B资源公共资源各有一个接口，加载方法为同一个方法。

```javascript



```


