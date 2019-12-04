数组排序sort() 2019年6月18日

1.升序
arr.sort(function (a, b) {
  return a-b
})
2.降序

arr.sort(function (a, b) {
  return b-a
})

3.按照数组对象中某个属性值进行排序
arr.sort(function (attr) {
  return function(a, b){
    var value1 = a[attr]
    var value2 = b[attr]
    return value1-value2
  }
})


数组去重 2019年6月18日

1.新建一个数组，遍历去要重的数组，当值不在新数组的时候（indexOf为-1）就加入该新数组中
function unique1(arr){
  var hash = []
  for (var i = 0; i < arr.length; i++) {
     if(hash.indexOf(arr[i]) === -1){
      hash.push(arr[i])
     }
  }
  return hash
}

2.测到有重复值时终止当前循环同时进入外层循环的下一轮判断

function unique2(arr){
  var hash=[];
  for (var i = 0; i < arr.length; i++) {
    for (var j = i+1; j < arr.length; j++) {
      if(arr[i] === arr[j]){
        ++i
      }
    }
      hash.push(arr[i])
  }
  return hash
}

3.前向查找与后向查找的索引为同一个值
function unique3(arr){
  var hash = []
  for (var i = 0; i < arr.length; i++) {
     if(arr.indexOf(arr[i]) === arr.lastIndexOf(arr[i])){
      hash.push(arr[i])
     }
  }
  return hash
}

4.Set
function unique4(arr) {
    return Array.from(new Set([...arr]))
}
5.for...of + Object利用对象的属性不会重复这一特性，校验数组元素是否重复
function unique5(arr) {
    let result = []
    let obj = {}

    for (let i of arr) {
        if (!obj[i]) {
            result.push(i)
            obj[i] = 1
        }
    }
    
    return result
}


扩展运算符...与剩余参数... 2019年6月27日

1.剩余参数 把多个独立的参数合并到一个数组中
要求：
（1）一个函数只能有一个剩余参数，且必须把它放在最后。
（2）剩余参数不能在对象字面量的 setter 属性中使用。
setter 只能使用单个参数...加变量的形式表示所有参数
... 与 arguments 不同 后者会把之前的参数都带上
2.扩展运算符 将一个数组分，并将各个项作为分离的参数传给函数
作用：
（1）拼接数组
  const strs = ['a','b','c','d']
  const nums = ['1','2','3']
  let members = [...strs,...nums] //['a','b','c','d','1','2','3']
（2）复制数组
正常情况下复制数组时复制的是引用
  const strs = ['a','b','c','d']
  let newStrs = strs // ['a','b','c','d']
  strs[1] = 'bb'
  console.log(strs)  //['a','bb','c','d']
  console.log(newStrs) //['a','bb','c','d']
（3）数组调用Math.max()
let values = [20,25,30,90]
之前的操作 
Math.max.apply(Math, values)
使用扩展运算符
Math.max(...values)
还可以附加参数 比如防止找到负数为最大值 
Math.max(...values, 0)

Symbol符号
创建
let a = Symbol('asd') 里面的参数可以省略，参数是用于描述该符号值，该描述不能用来访问对应属性
let person = {}
person[a] = 'John'
console.log("asd" in person)  //false
console.log(person[a])        //"asd"
console.log(a)                //Symbol(asd)

共享符号值

Object 方法

Object.propertype.hasOwnProperty() 方法可以用来检测一个对象是否含有特定的自身属性；和 in 运算符不同，该方法会忽略掉那些从原型链上继承到的属性。

JavaScript中只有六种情况为值为假[0(+0,-0), false, undefined, null, '', NaN, 0n, -0n]

JavaScript中typeof结果"number", "string", "boolean", "object", "function", "undefined", "symbol", "bigint"

JavaScript eval()函数 只能在非严格模式下使用
eval() 函数计算 JavaScript 字符串，并把它作为脚本代码来执行。
接收一个参数s，如果s不是字符串，则直接返回s。否则执行s语句。如果s语句执行结果是一个值，则返回此值，否则返回undefined。
如果参数是一个表达式，eval() 函数将执行表达式。如果参数是Javascript语句，eval()将执行 Javascript 语句。
eval("2+3")	// 返回 5



数组方法reduce()
接收一个函数作为累加器，数组中的每个值（从左到右）开始缩减，最终为一个值。
第一个参数为 callback 函数，第二个参数是作为第一次调用 callback 的第一个参数

如何知道一串字符串中每个字母出现的次数？
arrString.split('').reduce(function(res, cur) {
    res[cur] ? res[cur] ++ : res[cur] = 1
    return res;
}, {})

原型prototype

function Person(){}
Person.prototype指向的是Person的prototype对象，
Person的prototype对象，默认的只有一个叫做constructor的属性指向的是构造函数本身
每个实例对象都有一个隐藏的属性——“__proto__”（隐式原型），指向创建该对象的函数的prototype。
Object.prototype确实一个特例——它的__proto__指向的是null


Instanceof的判断规则是：
沿着A的__proto__这条线来找，同时沿着B的prototype这条线来找，
如果两条线能找到同一个引用，即同一个对象，那么就返回true。如果找到终点还未重合，则返回false。



闭包有三个特性：
1.函数嵌套函数 
2.函数内部可以引用外部的参数和变量 
3.参数和变量不会被垃圾回收机制回收





循环遍历
1.for-in
  for-in 循环主要用于遍历对象（可以遍历数组，遍历的是索引），
         循环遍历对象自身的和继承的可枚举属性(不含Symbol属性).
  遍历时不仅能读取对象自身上面的成员属性，也能延续原型链遍历出对象的原型属性
  所以，可以使用hasOwnProperty判断一个属性是不是对象自身上的属性。
  obj.hasOwnProperty(keys)==true 表示这个属性是对象的成员属性，而不是原先属性
2.for-of
  for-of循环可以使用的范围包括数组、Set 和 Map 结构、某些类似数组的对象（比如arguments对象、DOM NodeList 对象）、后文的 Generator 对象，以及字符串。  
  for-in循环读取键名，for-of循环读取键值。
3.forEach
  循环遍历数组中的每一项。
  forEach()里面每一次执行匿名函数都支持3个参数：数组中的当前项item,当前项的索引index,原始数组input。
  匿名函数中的this都是指Window。
  只能遍历数组。
  没有返回值。
4.map
  循环遍历数组中的每一项。
  map()里面每一次执行匿名函数都支持3个参数：数组中的当前项item,当前项的索引index,原始数组input。
  匿名函数中的this都是指Window。
  只能遍历数组。
  返回的是新数组
5.filter该方法不会改变原数组。
  用于过滤数组成员，满足条件的成员组成一个新数组返回。
  它的参数是一个函数，所有数组成员依次执行该函数，返回结果为true的成员组成一个新数组返回。

  filter循环有返回值
6.some & every()
  some()返回一个boolean，判断是否有元素符合func条件，如果有一个元素符合func条件，则循环会终止。
  every()返回一个boolean，判断是否有元素符合func条件，如果该函数对每一项返回true,则返回true
map，foreach，filter循环的共同之处：
  1.foreach，map，filter循环中途是无法停止的，总是会将所有成员遍历完。
  2.他们都可以接受第二个参数，用来绑定回调函数内部的this变量，将回调函数内部的this对象，指向第二个参数，间接操作这个参数（一般是数组）。
  3.map、forEach和filter循环都会跳过空位

循环中断  
forEach 除了用异常  剩下的 无法进行中断
map无法进行中断
for循环、for...in,for...of，支持await
for和for...of中可以使用break和continue
for...in会忽略continue和break

原型链

每一个javascript对象(除null外)创建的时候，就会与之关联另一个对象，这个对象就是我们所说的原型，每一个对象都会从原型中“继承”属性。

这是每个对象(除null外)都会有的属性，叫做__proto__，这个属性会指向该对象的原型。

每个原型都有一个constructor属性，指向该关联的构造函数。

