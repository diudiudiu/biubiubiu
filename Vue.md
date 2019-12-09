# Vue.js

### 数据双向绑定

原理:通过Object.defineProperty()来劫持各个属性的setter，getter，在数据变动时发布消息给订阅者，触发相应的监听回调。

首先需要定义监听器把对象每一项都转化成可观测对象，创建订阅器 Dep类 订阅器添加在监听器，创建订阅者Watcher





components组件内data为什么返回对象？
如果引用赋值给组件的话，所有组件公用一个数据，一个改变其它都改变。



## vue 的八个生命周期

1. ### beforeCreate() 

   在实例初始化之后，数据观测 和 事件配置之前被调用。

   下一步进行事件初始化，数据观测。

2. ### created()

   在实例创建完成后被立即调用。在这一步实例完成以下配置：数据观测(data observer),属性和方法的运算，watch/event事件回调。

   下一步判断实例对象是否有 el 属性。

   - 没有 el 属性，停止编译，停止生命周期，直到该 vue 实例上调用 `vm.$mount(el)`。
   - 有 el 属性，判断是否有template参数，该参数有无对生命周期无影响。
     - 有template参数，将其作为模版编译成render函数。
     - 没有template参数，将外部HTML作为模版编译。
     - ⚠️ 模版优先级：render函数选项>template选项>outer HTML。

3. ### beforeMount()

   在挂载开始之前被调用：相关的render函数首次被调用。此处还未能获取DOM。该钩子在服务器端渲染期间不被调用。

   下一步创建 ‘el’ ，给vue实例对象添加$el成员，并且替换掉挂载的DOM元素。

4. ### mounted()

   el被新创建vm.$el替换，并挂载到实例上去之后调用该钩子。该钩子在该钩子服务器端渲染期间不被调用，只有初次渲染会在服务端进行。

5. ### beforeUpdata()

   数据发生改变，在虚拟DOM打补丁之前触发。

   下一步vue实例发现data中的数据发生了改变，触发对应组件的重新渲染打补丁。

6. ### updated()

   由于数据更改导致的虚拟DOM重新渲染和打补丁，在这之后会调用该钩子。该钩子在该钩子服务器端渲染期间不被调用。

7. ### beforeDestroy()

   当显示调用destory()函数销毁组件或关闭页面，组件开始执行销毁程序之前触发。在这一步实例仍然完全可用。

   下一步解绑实例指示的所有东西包括监听器，实例，事件。

8. ### destoryed()

   Vue实例销毁后调用。调用后，Vue实例指示的所有东西都会解绑，所有的事件监听器会被移除，所有的子实例也会被销毁。该钩子在该钩子服务器端渲染期间不被调用。

### 事件修饰符

## methods 方法，watch 侦听器，computed 计算器



### computed 计算器

以Vue提供的依赖追踪系统为基础，只要依赖数据没有发生变化, computed 就不会再度进行计算。



### watch 侦听器

当依赖数据发生变化时，会进行 handler 函数的调用。

#### handler：侦听数据的执行函数 （function（newValue[，oldValue]）{}）

#### immediate：立即执行 （true or false）

watch 对变量初始化赋值时不进行 handler 的函数调用，当 immediate: true 时，会在变量初始化时就触发 handler 执行函数。

#### deep：深度侦听 （true or false）

watch  侦听对象时，是浅侦听（与浅拷贝意思相近）, 当 deep: true 时，会侦听对象内部属性的值的变化。



### methods 与 (watch computed)

methods 里面定义的函数，是需要主动调用的，而和 watch 和 computed 相关的函数，会自动执行预先定义的函数。



### watch 与 computed

watch擅长处理的场景：一个数据影响多个数据

computed擅长处理的场景：一个数据受多个数据影响，且会缓存数据。





## vue中打包导致路径改变，图片无法显示解决方法。

1. 只需要在build/utils.js文件中添加如下一行代码即可。

   ```javascript
   //在下面位置中加入publicPath:'../../'
   if (options.extract) {
       return ExtractTextPlugin.extract({
           use: loaders,
           publicPath: '../../',
           fallback: 'vue-style-loader'
       })
   } else {
       return ['vue-style-loader'].concat(loaders)
   }
   
   ```

2. require（‘../../img/picture.png’）



数组中的 filter(),concat(),slice() 方法不会触发页面的渲染。
还有直接更改数组中的某一项的值或更改数组长度也不会触发页面的渲染。
直接更改数组中的某一项的值可使用 Vue.set(数组名,要更改的索引,更改的内容) 方法

vue-router模式mode : hash  history

## 组件

用 v-if 控制的组件，每次的显示与隐藏都会触发其组件的 mounted() 以及之前的钩子函数。

用 v-show 控制的组件，只有最开始加载的时候会触发其组件的 mounted() 以及之前的钩子函数。



父组件调用子组件的方法或属性

1. ref 属性

   ```vue
   // 给子组件一个ref 属性
   this.$ref.ref的名.属性或方法
   ```

2. 

子组件调用父组件传入的方法 this.$emit(父组件绑定的方法名,参数)
子组件调用父组件传入的数据 props:['父组件绑定的数据名']

keep-alive 标签
能够缓存:is的组件内容





# vuex

### State: vuex中的数据源,相当于vue组件中的data。可在页面通过 `this.$store.state` 获取



### Getters：vuex可以用于监听、state中的值的变化,相当于vue中的computed计算属性。

```javascript
computed: {
  // 使用对象展开运算符将 getter 混入 computed 对象中
    ...mapGetters([
      'doneTodosCount',
      'anotherGetter',
      // ...
    ])
  }
```



### Mutations：vuex用于修改State数据的方法。



### Actions: vuex更改数据之前提交一下actions，再从actions中提交mutations中修改状态值，相当于vue中的methods,可以包含任意异步操作。

commit 用于调用mutation，当前模块和其他模块；
dispatch 用于调用action，当前模块和其他模块；
getters 用于获取当前模块getter；
state 用于获取当前模块state；
rootState 用于获取其它模块state；
rootGetters 用于获取其他模块getter；

在所有的vuex中 有一个 **rootState** 表示的是列表中所有vuex文件中的 state ,调用时候 **rootState**.文件名.state属性























## Vue3.0 响应系统

- ts语法


- 数据绑定（proxy） 2.x  （Object.defineprototype()）
- 基于函数的 composition API

- 初始化

1. ```javascript
   const App = {
   	setup() { //类似于created ,只会执行一次
   		let state = Vue.reactive({name: 'zf'}) // 返回的是一个 proxy
       return { // 类似于 data ，这个对象会座位渲染的上下文
         state
       }
   	}
   }
   Vue.createApp().mount(App,container) 
   // 创建并挂载到 container 上，不再是 new 一个 Vue 对象
   ```

   





react setstate（）

Vue Watcher 响应式 单文件组件

dep  通知器 收集器





#### computed 缓存 懒模式 与 getter区别

拦截 获取设置 setter getter



数组的监听 重写方法

 添加对象属性 有局限

## vue3

Proxy 元编程

Reflect  comlink



Map 代替dep

effect 代替watcher

 懒监听响应式 取值的时候

函数 push 触发get2次 一次是函数 一次是length



Proxy obj define

属性添加和删除 数组更改长度变化

性能 

