# Typescript





## 安装

```powershell
npm install -g typescript
```



## 自动编译.ts文件（vscode）

1. 创建 tsconfig.json 文件 

   ```powershell
   tsc --init 
   //生成配置文件
   ```

2. 设置tsconfig.json *"outDir"*: "./js",

3. 点击菜单   任务-运行任务    点击tsc:监视 -tsconfig.json 完成

4. 如果任务运行时出现**TS5058** 且目录没问题 请设置终端默认为cmd

## 语法

typeScript 在 JavaScript 基础上 增加了类型校验，变量声明必须指定类型，且不可在后面改变变量的类型。

### 数据类型

1. #### boolean 

   例如：`let flag:boolean = true`  定义后不可更改类型

2. #### number

   例如：`let flag:number = 123`  定义后不可更改类型

3. #### string

   例如：`let flag:string = '123'`  定义后不可更改类型

4. #### array 数组

   ```typescript
   // 第一种 只能都是数字 或 都是字符串
   let arr:number[] = [11, 22, 33]
   // 第二种 只能都是数字 或 都是字符串
   let arr:Array<number> = [11, 22, 33]
   ```

5. #### tuple（元组）

   ```typescript
   // 给每个位置设置类型
   let arr:[number,string] = [123, 'str']
   ```

   

6. #### enum （枚举）

   ```typescript
   //默认从0表示 一旦设置数字，后面随之增加
   enum code{
   	success,//  0
       error = 4,
       OK,    // 5
       notFound = -1,
       b      // 0
   }
   ```

   

7. #### any  （任意）

   可随意更改类型，可设置为对象。

   `let obj:any`

8. #### null 和 undefined 其他（never）类型的子类型

   `var num :number | undefined | null`   数字或undefined或null都可以

9. #### void类型 （表示没有任何类型,一般用于没有返回值的函数）

   ```typescript
   function run():void {
   	console.log('run')
   }
   //一个void类型的变量没有什么大用，因为只能为它赋予undefined和null
   ```

   

10. #### never类型 其他类型（包括 null 和 undefined）的子类型，代表从不会出现的值

    ```typescript
    //声明never 的变量只能被never 类型所赋值
    var a:never
    a=(()=>{
    	throw new Error('错误')
    })()
    ```
    
11. #### object 表示非原始类型





### 类型断言

了解某个值的详细信息

```typescript
//两种形式
//尖括号形式
let someValue: any = "this is a string";

let strLength: number = (<string>someValue).length;
//as语法
let someValue: any = "this is a string";

let strLength: number = (someValue as string).length;
```



### 函数

函数定义

```typescript
//函数声明法
function run ():string {
    return 'bac'
}
//匿名函数
var fun2 = function ():string {
    return 'bac'
}
```

函数传参

​	**可选参数必须配置到参数最后面**

​	**默认参数位置可以随意放置**

```typescript
//参数设置类型
function getInfo (name:string, age:number):string {
    return `${name}---${age}`
}
//参数设置类型 ?表示可选参数  
function getInfo (name:string, age?:number):string {
    return `${name}---${age}`
}
//默认参数 
function getInfo (name:string, age:number=20):string {
    return `${name}---${age}`
}
//剩余参数
function sum (a:number,...result:number[]):number {
	var sum = a 
	for(var i = 0; i < result.length; i++)
		sum += result[i]
    return sum
}
```

函数重载

​	函数名一样参数不同

```typescript
function getInfo (name:string):string 
function getInfo (age:number):number
function getInfo (str:any):any{
    if (typeof str === 'string'){
        return '我叫'+str
    }else{
        return '我年龄'+str
    }
}



function getInfo (name:string):string 
function getInfo (name:string, age:number):string 
function getInfo (name:any,age?:any):any{
    if (age){
        return '我叫'+name +'我年龄'+ age
    }else{
        return  '我叫'+name 
    }
}
```



### 接口

- 接口的作用就是为了检查结构类型
- 带有可选属性的接口与普通的接口定义差不多，只是在可选属性名字定义的后面加一个`?`符号。
- 一些对象属性只能在对象刚刚创建的时候修改其值。 可以在属性名前用 `readonly`来指定只读属性。



```typescript
interface LabelledValue {
  label: string;
  width?: number; // 可选属性
  readonly x: number; // 只读属性
}

function printLabel(labelledObj: LabelledValue) {
  console.log(labelledObj.label);
}
```

LabelledValue 接口就好比一个名字，用来描述上面例子里的要求。 它代表了有一个 label 属性且类型为 string 的对象。

### 类