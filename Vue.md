# Vue.js

components组件内data为什么返回对象？
如果引用赋值给组件的话，所有组件公用一个数据，一个改变其它都改变。

数据双向绑定
原理:通过Object.defineProperty()来劫持各个属性的setter，getter，在数据变动时发布消息给订阅者，触发相应的监听回调。

首先需要定义监听器把对象每一项都转化成可观测对象
创建订阅器Dep类 订阅器添加在监听器
创建订阅者Watcher





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



父组件调用子组件的方法或属性

```

```

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
