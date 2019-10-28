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



### RegExp 正则表达式

正则表达式是一种匹配模式，要么匹配字符，要么匹配位置。

#### 字符组的常用的简写形式

\d 匹配一个数字符，[ 0-9 ]

\D 匹配一个非数字符，[ ^0-9 ]

\w 匹配一个 字母数字下划线，[ a-zA-Z0-9 _ ]

\W 匹配一个非单词字符，[ ^a-zA-Z0-9_ ]

 \s 匹配一个空白字符，[  \n\r\f\t\v ] 空格  水平制表符\t 垂直制表符\v 换行符\n 回车符\r 换页符\f

\S 匹配一个非空白字符，等于[ ^\n\f \r\t\v ]

.  匹配一个除换行符 回车符 行分隔符 段分隔符 之外的任意字符 [ ^\n\r\u2028\u2029 ]

#### 位置

^ 匹配开头，当有正则修饰符 m 时，表示行开头位置。

$ 匹配结尾，当有正则修饰符 m 时，表示行结尾位置。

\b 匹配一个单词的边界， 即，\w 与 \W，^ 与 \w，\w 与 $ 之间的位置。

\B 匹配一个单词的非边界，即，\w 与 \w，\W 与 \W， ^ 与 \W，\W与 $ 之间的位置

##### 断言也是匹配位置

(?=abc) 正向先行断言 匹配某一项，其后面一定接着abc。

(?!abc) 负向先行断言 匹配某一项，其后面一定不是abc。

(?<=abc) 正向后行断言 匹配某一项，其前面一定是abc。

(?<!abc) 负向后行断言 匹配某一项，其前面一定不是abc。

###### 断言练习

```javascript
// 密码必须包含数字，且必须以字母开头，一共8-12位。
var exp = /^[a-zA-Z](?![a-zA-Z]+$)\w{7,11}$/
// ^ 匹配一行的开头位置
// (?![0-9]+$) 预测该位置后面不全是数字
```



#### 括号作用

(ab) 捕获型分组 ab为一个整体，进行缓存，进行分组编号，可以反引。

(?:ab) 非捕获型分组 ab为一个整体，不进行缓存，不进行分组编号，不可以反引。

(good|nice) 捕获分支结构 good或nice，进行分组编号，可以反引。

(?:good|nice) 非捕获分支结构 good或nice，进行分组编号，可以反引。

\2 反向引用。 表示饮用第二个括号内捕获的数据。

#### 修饰符

1. i 忽略大小写匹配
2. m 多行匹配，在一行文本末尾时还会继续寻常下一行中是否与正则匹配的项
3. g 全局匹配 模式应用于所有字符串，而非在找到第一个匹配项时停止
4. u  不再针对码元  (ES6新增）
5. y 粘性匹配 与lastIndex属性有关

#### 贪婪匹配与惰性匹配

```javascript
// 在量词后加'?'表示启用惰性匹配
// 默认形式是贪婪匹配

//贪婪匹配 取最多项
var regex = /\d{2,5}/g
var string = "123 1234 12345 123456"
string.match(regex)
// => ["123", "1234", "12345", "123456"]

//惰性匹配 取最少项
var regex = /\d{2,5}?/g
var string = "123 1234 12345 123456"
string.match(regex)
// => ["12", "12", "34", "12", "34", "12", "34", "56"]
```



#### 可以操作正则的方法：共有6个，字符串实例4个，正则实例2个：

String	search	split	match	replace

RegExp	test	exec

###### String=>str.seach() 

1. 接受一个字符串为参数，如果格式与正则类似，会把字符串转换为正则表达式。
2. 返回正则表达式或字符串匹配到的第一个子串在目标字符串中的下标。
3. 不执行全局匹配，它将忽略标志 g。
4. regexp 的 lastIndex 属性。

```javascript
let str="diudiudiu博客abc";
let reg=/\w+/g 
console.log(str.search(reg)); // => 0
```

###### String=>str.split()

1. 接受两个参数，第一个必填参数，字符串或者正则表达式，并从匹配到的地方分割str。第二个可选参数，数字，指定返回的数组的最大长度。没有设置该参数，整个字符串都会被分割，不考虑它的长度。
2. 返回一个字符串按匹配规则分割出的字符串数组。

如果理解了请看下面例子

```javascript
var colorText = "red,blue,green,yellow";
var colors3 = colorText.split(/[^\,]+/); 
console.log(colors3) // ???
// 你以为是不是这样的 
// =>>> ["red", "blue", "green", "yellow"]


// 其实是这样的
// =>>> ["", ",", ",", ",", ""]
```

`[^\,]+` 意思应该是以不是‘，’的多个字符”作为分隔符，`\,` 是转义‘,’ `^` 是取非，这样看就很明显了。不要被思维思维局限了。

##### RegExp 静态属性

| 属性            | 说明                                                |
| --------------- | --------------------------------------------------- |
| input           | 最近一次目标字符串，可以简写成 $_ 。                |
| lastMatch       | 最近一次匹配的文本，可以简写成 $& 。                |
| lastParen       | 最近一次捕获的文本，可以简写成 $+ 。                |
| leftContext     | 目标字符串中 lastMatch 之前的文本，可以简写成 $` 。 |
| rightContext    | 目标字符串中 lastMatch 之后的文本，可以简写成 $' 。 |
| `$1，$2,...,$9` | 最近一次第 1-9 个分组里捕获的数据                   |



###### String=>str.match()

1. 接受一个字符串为参数，如果格式与正则类似，会把字符串转换为正则表达式。
2. 返回匹配到结果的数组，返回结果的格式，与正则对象是否有修饰符 g 有关。
3. 没有 g ,返回的是标准匹配格式的数组。 第0个元素是整体匹配内容，其余元素是分组捕获的内容。返回的数组还含有两个对象属性。index 属性是匹配文本的起始字符在 str 中的位置，input 属性声明的是对 str 的引用。
4. 有 g ,返回的是所有匹配的内容的数组。
5. 无匹配内容时返回 null。

```javascript
var str="1 plus 2 equal 3"
var match = str.match(/( \d)|( e)/)
console.log(match)				// => Array [" 2", " 2", undefined]
console.log(match.index)  // =>6
console.log(match.input)  // =>"1 plus 2 equal 3"

var macthg = str.match(/( \d)|( e)/g)
console.log(matchg)				// => Array [" 2", " e", " 3"]

```

###### String=>str.replace()

1. 接受两个参数，第一个为字符串或正则表达式，第二个参数为字符串或函数。

2. 返回一个匹配后被替换的新字符串。

3. 如果第二个参数是匿名函数，每次捕获1次，这个匿名函数就会执行1次。

   第二个参数中特殊字符

   | 字符             | 说明                           |
   | ---------------- | ------------------------------ |
   | $&               | 匹配到的子串文本               |
   | $`               | 匹配到的子串的左边文本         |
   | $'               | 匹配到的子串的右边文本         |
   | $$               | 美元符号                       |
   | `$1，$2,...,$99` | 匹配第 1-99 个分组里捕获的文本 |

   

   - 这个匿名函数有三个参数，0：要替换的字符串，1：替换位置，2：原字符串。
   - 当设置参数时，第一个为匹配到的字符串，之后为分组捕获的结果。

   ```javascript
   var st="hello123hello456"
   var reg=/hello/g;
   var ss=st.replace(reg,function(){
   	console.log(arguments)
   	return "world"     
   })
   
   // ==> Object { 0: "hello", 1: 0, 2: "hello123hello456" }
   // ==> Object { 0: "hello", 1: 8, 2: "hello123hello456" }
   
   
   
   var url= window.location.href
   const search = url.substr(url.indexOf('?')+1)
   const obj={}
   search.replace(/([^&=]+)=([^&=]*)/g,function(rs,$1,$2){
     console.log(rs)
     console.log($1)
     console.log($2)
   	obj[decodeURIComponent($1)]=decodeURIComponent($2)
   })
   
   
   
   ```



###### RegExp=>regex.test()

1. 传入一个要检测的字符串。

2. 判断目标字符串中是否有满足正则匹配的子串。返回布尔值。 

3. 正则表达式有个 lastIndex 属性，当全局匹配 g 时，会在 lastIndex 的位置开始匹配，注意绝大多数情况下匹配完要归0。

   ```javascript
   const str="haha haha"
   const reg=/haha/g
   
   console.log(reg.test(str))
   //打印 true
   
   console.log(reg.lastIndex)
   // 打印4，因为匹配到了，所以设置lastIndex为匹配结果紧挨着的字符位置
   
   console.log(reg.test(str))
   // 打印 true
   
   console.log(reg.lastIndex)
   // 打印9，因为匹配到了，所以设置lastIndex为匹配结果紧挨着的字符位置
   
   console.log(reg.test(str))
   // 打印 false，因为从lastIndex位置检索字符串，已经没有匹配结果了
   
   console.log(reg.lastIndex)
   // 打印0，因为没有匹配到结果，所以将lastIndex重置为0
   
   ```

###### RegExp=>regex.exec()

1. 传入需要匹配的字符串。
2. 返回一个数组，其中存放匹配的结果。如果未找到匹配，则返回值为 null。
3. 当没有全局 g 时，返回的是标准匹配格式的数组。 第0个元素是整体匹配内容，其余元素是分组捕获的内容。返回的数组还含有两个对象属性。index 属性是匹配文本的起始字符在传入的字符串 str 中的位置，input 属性声明的是对传入的字符串 str 的引用。
4. 当全局 g 时，它会在 lastIndex 属性指定的字符处开始检索字符串 string。当找到与表达式相匹配的文本时，在匹配后，将 lastIndex 属性设置为匹配文本的最后一个字符的下一个位置。当 exec() 再也找不到匹配的文本时，它将返回 null，并把 lastIndex 属性重置为 0。

exec 比 match 强大,exec 可以接着上一次匹配后继续匹配。

#### 常用的操作：验证（查找），切分，提取，替换

1. 验证（查找） 最常用的是test

   - search
   - test
   - match
   - exec

2. 切分

   - split

3. 提取

   - match
   - exec
   - test
   - search

4. 替换

   - replace

   

   