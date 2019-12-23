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



### Set Map WeakSet WeakMap 

1. ### Set 集合

   类似数组，以[value,value]存储的，但是里面不能有重复值，如果有重复值将只显示一个。

   - size：属性 返回集合的元素个数。（类似数组的长度length）

   - add(value)方法: 向集合中添加一个元素value。

     **如果向集合中添加一个已经存在的元素，不报错但是集合不会改变。**

   - delete(value)方法: 从集合中删除元素value。

   - has(value)方法: 判断value是否在集合中，返回true或false.

   - clear()方法: 清空集合。

2. ### Map 字典

   种类型的值（包括object）都可以作为key，以[key,value]存储的，里面不能有重复值。

   - size: 属性，取出 Map 的长度
   - set(key, value)：方法，向 Map 中添加新元素
   - get(key)：方法，通过键查找特定的数值并返回
   - has(key)：方法，判断 Map 中是否存在键key
   - delete(key)：方法，通过键 key 从 Map 中移除对应的数据
   - clear()：方法，将这个字典中的所有元素删除
     

3. ### WeakMap

   WeakMap 跟 Map 结构类似，也拥有 get 、has 、 delete 等方法，使用法和使用途都一样。

   不同之处

   - WeakMap 只接受对象作为键名，但 null 不能作为键名。
   - WeakMap 不支持 clear 方法，不支持遍历，没有 keys 、 values 、entries 、 forEach 这4个方法，也没有 size 属性。
   - WeakMap 键名中的引用类型是弱引使用，假如这个引使用类型的值被垃圾机制回收了，WeakMap 实例中的对应键值对也会消失。WeakMap 中的 key 不计入垃圾回收，若只有 WeakMap 中的 key 对某个对象有引用，那么此时执行垃圾回收时就会回收该对象， Map 中的 key 是计入垃圾回收。

4. ### WeakSet

   WeakSet 跟 Set 结构类似，只有 get 、has 、 delete 三个方法。

   不同之处：

   - WeakSet 的成员只能是对象。
   - WeakSet 不支持 clear 方法，不支持遍历，没有 forEach 方法和 size 属性。
   -  WeakSet 中的对象都是弱引用，即垃圾回收机制不考虑  WeakSet 对该对象的引用，只 WeakSet 引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存。



### apply，call，bind 的区别

这三个函数都是改变了当前函数的 this 指向。

- apply 接收的是数组，并会立即执行
- call 接收的是用逗号隔开的参数，并会立即执行
- bind 接收的是用逗号隔开的参数，但是不会立即执行，而是返回一个新的函数

**进阶** ：源码实现

实现思路

1. 把函数变成 object 的一个属性。
2. 执行这个 object 下面的函数。
3. 删除这个 object 下的这个函数。

```javascript
Function.prototypr.call(context, ...args){
	context = context || window // 考虑 context = null 
  context.fn = this
  let result = context.fn(..args)
  delete context.fn
  return result
}
```

apply 和 call 是一个道理

```javascript
Function.prototype.apply(context, args)
{
    context = context || window;
    context.fn = this;
    var result = context.fn(...args);
    delete context.fn;
    return result;
}
```

bind 有一点特殊

```javascript
Function.prototype.bind = function (context, ...rest) {
    var self = this;
    return function F(...args) {
        return self.apply(context, rest.concat(args)); // 参数合并到一起，作为函数的参数
    }
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

### blur 与 focus 事件

这个两个事件不止存在与 input a元素中，还可以给div等块级元素设置事件，只需要设置tab属性。

**tab属性**：用键盘上的tab键进行移动光标的时候，光标只在具有tab属性的元素上进行跳转。

给div等块元素设置tab属性：tabindex=数字(与z-index类似，计算tab起点)。

定义tab属性后，元素是默认会加上焦点虚线的，IE中设置`hidefocus="true"`去除，其他浏览器设置`outline=0`进行去除。

工作中应用：

<img src="/Users/t/Library/Application Support/typora-user-images/image-20191112150902325.png" alt="image-20191112150902325" style="zoom:50%;" />

需求：点击button的位置B出现，点击其他位置B进行隐藏。

限制条件：

1. 带有 button 的图形为 canvas 是一个完整的插件，B是其一个功能，所以不允许向外层冒方法。
2. A不是幕布，A中有各式各样的组件。
3. button 的点击事件是通过计算点击时的位置返回 boolean类型 来进一步处理和触发B的显示和隐藏。

遇到的问题：

1. 因为是 canvas 无法绑定其中按钮的点击事件，只能通过限制条件3返回为 true 时来进行处理。
2. 当为 true 进行列表显示并为其添加聚焦事件focus()，当点击时就会出现问题，点击分为按下和抬起两个步骤，当按下时候执行上述方法列表显示获取焦点，但抬起时位置在按钮上而不是在B身上，所以触发了B的 blur 事件，列表会显示一下后隐藏。

解决方案：当按钮显示时添加个状态，在B的 blur 事件上 添加 `if` 条件 改变状态值并再次使其获取焦点。

### DOMContentLoaded 与 load 事件

DOMContentLoaded:当初始的 HTML 文档被完全加载和解析完成之后，DOMContentLoaded 事件被触发，而无需等待样式表、图像和子框架的完成加载。

load:当一个资源及其依赖资源已完成加载时，将触发load事件。

首屏时间: “计算这个网页从空白到出现内容所花费的时间”。

这段时间其实就是HTML 文档加载和解析的时间。也就是DOMContentLoaded 事件触发之前所经历的时间。

对于首屏时间而言，js放在HTML文档的开头和结尾处效果是一样的而js放在结尾的目的并不是为了减少首屏时间，而是由于js经常需要操纵DOM，放在后面才更能保证找到DOM节点。

async、defer与DOMContentLoaded详细关系请看[这里](https://www.cnblogs.com/Bonnie3449/p/8419609.html)

### mouseout 与 mouseleave 离开元素事件

- onmouseout 离开当前**DOM元素或其子元素**时候触发
- onmouseleave 离开当前**绑定的DOM元素**时候触发

### mouseover 与 mouseenter 事件

- mouseenter事件只作用于目标元素
- mouseover最用于目标元素及其后代元素。

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

### Promise 状态：

- pending:   初始状态
- fulfilled:     成功
- rejected:    失败

状态改变只能是 pending->fulfilled 或者 pending->rejected，状态**一旦改变则不能再变**。

.then(回调函数)

.catch(回调函数)

Promise 每次调用 .then 或者 .catch 都会返回的一个新的 Promise 实例，从而实现了链式调用。

### 有以下注意点：

1. 构造函数中的 resolve 或 reject 只有第一次执行有效，多次调用没有任何作用。

   ```javascript
   const promise = new Promise((resolve, reject) => {
     resolve('success1')
     reject('error')
     resolve('success2')
   })
   promise
     .then((res) => {
       console.log('then: ', res)
     })
     .catch((err) => {
       console.log('catch: ', err)
     })
   
   // =>then: success1
   ```

   

2. promise 内部状态一经改变，并且有了一个值，那么后续每次调用 .then 或者 .catch 都会直接拿到该值。

   ```javascript
   const promise = new Promise((resolve, reject) => {
     setTimeout(() => {
       console.log('once')
       resolve('success')
     }, 1000)
   })
   
   const start = Date.now()
   promise.then((res) => {
     console.log(res, Date.now() - start)
   })
   promise.then((res) => {
     console.log(res, Date.now() - start)
   })
   
   // =>once
   // =>success 1005
   // =>success 1007
   ```

3. .then 或者 .catch 中 return 一个 error 对象不会抛出错误。

   ```javascript
   Promise.resolve()
     .then(() => {
       return new Error('error!!!')
     })
     .then((res) => {
       console.log('then: ', res)
     })
     .catch((err) => {
       console.log('catch: ', err)
     })
     
     // =>then: Error: error!!!
     
     
     // 如果要抛出错误写成以下格式
     return Promise.reject(new Error('error!!!'))
   	throw new Error('error!!!')
   ```

4. .then 或 .catch 返回的值不能是 promise 本身，否则会造成死循环。

   ```javascript
   const promise = Promise.resolve()
     .then(() => {
       return promise
     })
   promise.catch(console.error)
   ```

5. .then 或者 .catch 的参数期望是函数，传入非函数则会发生值穿透。

   ```javascript
   Promise.resolve(1)
     .then(2)
     .then(Promise.resolve(3))
     .then(console.log)
   
   // =>1
   ```

#### API

##### `Promise.all(iterable)` 

1. 返回一个新的 promise 对象，该 promise 对象在 iterable 参数对象里所有 promise 对象都成功时才触发成功，有任何一个 iterable 里面的 promise 对象失败，立即触发失败。
2. 这个新的 promise 对象在触发成功状态以后，会把一个包含 iterable 里所有 promise 返回值的数组作为成功回调的返回值，顺序跟 iterable 一致；如果这个新的 promise 对象触发了失败状态，它会把 iterable 里第一个触发失败的 promise 对象的错误信息作为它的失败错误信息。

##### `Promise.race(iterable)`

- 当 iterable 参数里的任意一个 promise 被成功或失败后，父 promise 马上会用子 promise 的成功返回值或失败详情作为参数调用父 promise 绑定的相应句柄，并返回该promise对象。

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


