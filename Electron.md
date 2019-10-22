# Electron





## 安装Electron+Vue

1. 安装[node.js](http://nodejs.org/)及npm。命令行node -v npm -v 试试。

2. 安装vue-cli脚手架

   ```powershell
   cnpm install --global vue-cli
   ```

   命令行vue一下试试

3. 搭建vue项目

   ```powershell
   vue init webpack demo
   ```

   

4. 修改config > index.js文件中dev下 port端口项为8080， autoOpenBrowser自动打开浏览器项为false，build下 `assetsPublicPath: './'`,。命令行npm run dev试试。

5. 打包

   ```powershell
   npm run build
   ```

   这时会出现dist文件夹，并把**路径切换到dist文件夹**下。

6. 安装 Electron

   ```shell
   npm install electron -g
   ```

   命令行 electron -v 一下试试

7. 创建 main.js 文件以及 package.json

   main.js 就是electron的窗口设计可以自行设计，以下有默认的基本个样式代码。

   ```javascript
   // electron 在一开始会通过node去执行当前 main.js 文件
   
   // app 控制整个应用程序的生命周期
   // BrowserWindow 用来创建一个浏览器窗口
   const {app, BrowserWindow} = require('electron')
   
   const path = require('path')
   
   
   let mainWindow
   
   function createWindow () {
     //创建一个新的窗口
     mainWindow = new BrowserWindow({
       width: 800,
       height: 600,
       webPreferences: {
         preload: path.join(__dirname, 'preload.js')
       }
     })
   
     // 加载HTML文件
     mainWindow.loadFile('index.html')
     // mainWindow.loadFile('https://github.com')
    
     // 开发者工具
     mainWindow.webContents.openDevTools()
   
     // 关闭释放资源
     mainWindow.on('closed', function () {
       
       mainWindow = null
     })
   }
   
   
   
   // 注册事件，当应用准备完成触发
   app.on('ready', createWindow)
   
   // 针对 macos
   app.on('window-all-closed', function () {
     // On macOS it is common for applications and their menu bar
     // to stay active until the user quits explicitly with Cmd + Q
     if (process.platform !== 'darwin') app.quit()
   })
   
   app.on('activate', function () {
     // On macOS it's common to re-create a window in the app when the
     // dock icon is clicked and there are no other windows open.
     if (mainWindow === null) createWindow()
   })
   ```

   package.json **注释要删除**

   ```json
   {
     "name": "demo",
     "productName": "项目名称",
     "author": "作者",
     "version": "1.0.4",//版本号
     "main": "main.js",//主文件入口
     "description": "项目描述",
     "scripts": {
       "pack": "electron-builder --dir",
       "dist": "electron-builder",
       "postinstall": "electron-builder install-app-deps"
     },
     "build": {
       "electronVersion": "1.8.4",
       "win": {
         "requestedExecutionLevel": "highestAvailable",
         "target": [
           {
             "target": "nsis",
             "arch": [
               "x64"
             ]
           }
         ]
       },
       "appId": "demo",//程序id
       "artifactName": "demo-${version}-${arch}.${ext}",
       "nsis": {
         "artifactName": "demo-${version}-${arch}.${ext}"
       },
       "extraResources": [
         {
           "from": "./static/xxxx/",//需要打包的静态资源
           "to": "app-server",//静态资源存放路径
           "filter": [
             "**/*"//打包静态资源文件夹内的所有文件  如果没有静态资源要打包进去，extraResources 这段代码去掉
           ]
         }
       ],
       "publish": [
         {
           "provider": "generic",
           "url": "http://xxxxx/download/"//自动更新的安装包地址，初步使用publish这段代码不需要
         }
       ]
     },
     "dependencies": {
       "core-js": "^2.4.1",
       "electron-packager": "^12.1.0",//不打包成exe程序可以去掉
       "electron-updater": "^2.22.1",//不打包成exe程序可以去掉
     }
   }
   ```

   命令行

   ```powershell
    electron . 
   ```

   8.

   

8. 打包成软件包

   先安装依赖

   ```powershell
   npm install electron-builder -g
   
   npm install electron-package -g
   ```

   执行打包命令

   ```powershell
   electron-builder
   ```

9. 完成



Electron分为 main process 主线程 和 renderer process 渲染线程

main.js 是在主线程中

### `main.js`通过Electron创建新窗口

```javascript
// electron 在一开始会通过node去执行当前 main.js 文件

// app 控制整个应用程序的生命周期
// BrowserWindow 用来创建一个浏览器窗口
const {app, BrowserWindow} = require('electron')

const path = require('path')


let mainWindow

function createWindow () {
  //创建一个新的窗口
  mainWindow = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      preload: path.join(__dirname, 'preload.js')
    }
  })

  // 加载HTML文件
  mainWindow.loadFile('index.html')
  // mainWindow.loadFile('https://github.com')
 
  // 开发者工具
  mainWindow.webContents.openDevTools()

  // 关闭释放资源
  mainWindow.on('closed', function () {
    
    mainWindow = null
  })
}



// 注册事件，当应用准备完成触发
app.on('ready', createWindow)

// 针对 macos
app.on('window-all-closed', function () {
  // On macOS it is common for applications and their menu bar
  // to stay active until the user quits explicitly with Cmd + Q
  if (process.platform !== 'darwin') app.quit()
})

app.on('activate', function () {
  // On macOS it's common to re-create a window in the app when the
  // dock icon is clicked and there are no other windows open.
  if (mainWindow === null) createWindow()
})

```



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





### 类