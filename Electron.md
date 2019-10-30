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



