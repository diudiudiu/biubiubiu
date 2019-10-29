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







原型链上的属性会被多个实例共享，构造函数中不会。

ES5继承  对象冒充继承

.call()  

可以继承构造函数里面的属性和方法，但是无法继承原型链上面的属性和方法

原型链继承  

`子类.prototype = new 父类（）`

可以继承构造函数里面的属性和方法也可以继承原型链上面的属性和方法。

但是实例化子类没法给父类传参数

```javascript
//ES5完美继承
function Person(name, age){
    //父类
    this.name = name
    this.run = function () {
        alert(this.name+'run')
    }
}
person

```



## 事件

### onmouseout

离开当前DOM元素或其子元素时候触发

### onmouseleave

离开当前绑定的DOM元素时候触发





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
- Enumerables 同数据属性的 Enumerables 一样，是否可枚举，for-in或Object.keys()返回，字面量定义，默认为true
- Get 给属性提供的 getter 的方法，如果没有 getter 则为 undefined。
- Set 给属性体统 Setter 的方法，该方法接受**唯一参数**，如果没有 setter 则为 undefined。

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


