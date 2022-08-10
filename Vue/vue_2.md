# Vue脚手架(项目构建工具Vue-cli)

## 使用Vue-cli

1. 全局安装:npm install -g @vue/cli(仅第一次执行)
2. 进入要创建项目的目录,使用命令创建项目:vue create 项目名
3. 启动项目:npm run serve

*注1：下载缓慢可以配置npm淘宝镜像：npm config set registry*

*注2：Vue脚手架隐藏了所有webpack相关的配置，想查看需执行：vue inspect > output.js*

## ref属性

- 在html标签上使用获取的是真实DOM元素，在组件标签上使用获取的是组件实例对象（vc）
- 使用方式：
  1. 打标识：```<h1 ref="xxx"></h1>``` 
  2. 获取：```this.$refs.xxx```

## props配置项

- 功能：组件接收数据(组件间通信方式之一)
- 传递数据：`<king name="xxx"/>`
- 接收数据：`props:['name']`

*注：props接收过来的数据无法修改*

## mixin(混入)

- 功能：将组件共同的东西提取出来

- 使用：

​    1.在mixin.js中定义:

```
export const two = {
    data() {
        return {
            name1: "奈落泉切",
            name2: "奈落冷泠"
        }
    },
}
```

2. 在main.js中引入：

```
import { two } from './two_mixin'
Vue.mixin(two)
```

## 插件

- 功能：跟混入同功能，但不同语法

- 定义插件： 

  ```
  export default {
      install(Vue) {
          //混入
          Vue.mixin({
              data() {
                  return {
                      name1: "奈落泉切",
                      name2: "奈落冷泠"
                  }
              },
          });
          //自定义指令
          Vue.directive('xz', {
              //指令与元素成功绑定时（一上来）
              bind() {
                  console.log("a");
              },
              //指令所在元素被插入页面时
              inserted() {
                  console.log("b");
              },
              //指令所在的模板被重新解析时
              update() {
                  console.log("c");
              }
          });
      }
  }
  ```

- 使用插件：

  ```
  import king from './king'
  Vue.use(king)
  ```

## scoped样式

1. 作用：让样式在局部生效，防止冲突。
2. 写法：```<style scoped>```

## 总结TodoList案例

组件化编码流程：

1. 拆分静态组件按功能拆

2. 数据是多个组件在用，就把数据放在它们共同的父组件上

3. 实现交互从绑定事件开始

- props作用于父子间的通信，但子给父传数据时要求父先给子一个函数

*注：v-model绑定的值不能是props传过来的值，因为props不可修改！*

*注：props接收的对象可修改，但不能这么做。*

## webStorage

浏览器端通过 Window.sessionStorage 和 Window.localStorage 属性来实现本地存储机制。

API：

1. ```xxxxxStorage.setItem('key', 'value');```
   	该方法接受一个键和值作为参数，会把键值对添加到存储中，如果键名存在，则更新其对应的值。

2. ```xxxxxStorage.getItem('person');```

   ​		该方法接受一个键名作为参数，返回键名对应的值。

3. ```xxxxxStorage.removeItem('key');```

   ​		该方法接受一个键名作为参数，并把该键名从存储中删除。

4. ``` xxxxxStorage.clear()```

   ​		该方法会清空存储中的所有数据。

1. 备注：

   1. SessionStorage存储的内容会随着浏览器窗口关闭而消失。
   2. LocalStorage存储的内容，需要手动清除才会消失。
   3. ```xxxxxStorage.getItem(xxx)```如果xxx对应的value获取不到，那么getItem的返回值是null。
   4. ```JSON.parse(null)```的结果依然是null。

## 组件的自定义事件

组件间通信方式之一，适用于：<strong style="color:red">子组件 ===> 父组件</strong>(props也可以实现，绑定自定义事件只是另一种实现方式)

- 父组件给子组件传数据，事件的回调在父组件中

- 父组件中绑定自定义事件:

```
……
    <king @xk="a1" />
……

……
  methods: {
    a1() {
      console.log("死亡如风，常伴吾身");
    },
  },
……
```

- 子组件中使用自定义事件:

```
……
    <button @click="a2">触发</button>
……

……
  methods: {
    a2() {
      this.$emit("xk");
    },
  },
……
```

## 全局事件总线（GlobalEventBus）

组件间通信方式之一，适用于<span style="color:red">任意组件间通信</span>。

- main.js中安装全局事件总线：

```js
……
    beforeCreate() {
        Vue.prototype.$bus = this;
    }
……
```

- 发送数据的组件：


```js
……
      this.$bus.$emit("事件名称", 发送的内容);
……
```

- 接收数据的组件：

```
……
    this.$bus.$on("事件名称", (接收到的内容) => {
         代码体
    });
……
```

## 消息订阅与发布（pubsub）

组件间通信方式之一，适用于<span style="color:red">任意组件间通信</span>(功能与全局事件总线完全重合)

安装pubsub：```npm i pubsub-js```(仅第一次执行)

引入: ```import pubsub from 'pubsub-js'```

- 发送数据的组件：

```js
……
import pubsub from "pubsub-js";
……
    say() {
      pubsub.publish("事件名称", 发送的内容);
    },
……
```

- 接收数据的组件：

```
……
import pubsub from "pubsub-js";
……
    pubsub.subscribe("事件名称", (接收到的事件名称, 接收到的内容) => {
      代码体
    });
……
```

## nextTick

1. 语法：```this.$nextTick(回调函数)```
2. 作用：在下一次 DOM 更新结束后执行其指定的回调。
3. 什么时候用：当改变数据后，要基于更新后的新DOM进行某些操作时，要在nextTick所指定的回调函数中执行。

## Vue封装的过渡与动画

写法：

```
/**进入的起点&&离开的终点 */
.a1-enter,
.a1-leave-to {
  transform: translateX(-100%);
}
/**进入过程中&&离开过程中 */
.a1-enter-active,
.a1-leave-active {
  transition: 2.5s linear;
}
/**进入的终点&&离开的起点 */
.a1-enter-to,
.a1-leave {
  transform: translateX();
}
```

使用```<transition>```包裹要过渡的元素，并配置name属性：

```vue
<transition name="a1">
    <h1 v-show="k1">齐疯子</h1>
</transition>
```

注：若有多个元素需要过度，则需要使用：```<transition-group>```，且每个元素都要指定```key```值

```
<transition-group name="a1" appear>
			<h1 v-show="!k1" key="1">齐疯子</h1>
			<h1 v-show="k1" key="2">洛岚歌</h1>
</transition-group>
```

## 插槽

让父组件可以向子组件指定位置插入html结构，也是一种组件间通信的方式，适用于 <strong style="color:red">父组件 ===> 子组件</strong> 

默认插槽:

- 子组件:

```
<slot>默认值，父组件不传递任何结构时将出现</slot>
```

- 父组件:

```
<king>
   <h1>恕瑞玛，你的皇帝回来了!!!</h1>
</king>
```

具名插槽:

- 子组件:

```
<slot name="k1">默认值，父组件不传递任何结构时将出现1</slot>
<slot name="k2">默认值，父组件不传递任何结构时将出现2</slot>
```

- 父组件:

```
<king>
   <h1 slot="k1">恕瑞玛，你的皇帝回来了!!!</h1>
   <h5 slot="k2">月夜之影</h5>
</king>
```

作用域插槽:

理解：数据在子组件中，父组件按需使用子组件中的数据生成想要的结构

- 子组件:

```
<slot :kingthree="kingthree">默认值，父组件不传递任何结构时将出现</slot>
……
kingthree: ["武夜零", "纪子诛", "陆执子"]
```

- 父组件:

```
<king>
   <template scope="q1">
      {{ q1.kingthree[0] }}
   </template>
</king>
```

注:在作用域插槽中，template是必须的,scope="q1"就相当于取一个名字，随便取，传过来的数据会收录在名字中

## Vuex数据管理

Vuex是一个把数据集中起来进行管理的Vue插件(Vuex数据管理)，组件间通信方式之一，适用于任意组件间的通信。

当多个组件需要共享数据时使用

Vuex可以完美替代:props配置项、组件的自定义事件、全局事件总线、消息订阅与发布、插槽

### 搭建vuex环境

1. 安装Vuex:npm i vuex@3 (vue2只能用vuex3,vue3只能用vuex4)

2. 创建文件：```src/store/index.js```

3. index.js:

   ```
   import Vue from 'vue'
   import Vuex from 'vuex'
   Vue.use(Vuex)
   
   const actions = {}
   const mutations = {}
   const state = {}
   
   export default new Vuex.Store({
       actions,
       mutations,
       state
   })
   ```
   
4. main.js:

   ```
   ……
   //引入store
   import store from './store'
   ……
   new Vue({
       ……
       store
   })
   ```

使用:

- index.js:

```
……
const mutations = {
        Qk(state) {
            state.crazyKing = '洛岚歌'
        }
    }
const state = {
    crazyKing: '齐疯子',
}
……
```

- 组件:

```
<h1>{{ $store.state.crazyKing }}</h1>
<button @click="ac">点我切换王后</button>
……
  methods: {
    ac() {
      this.$store.commit("Qk");
    },
  }
```

### getters加工

当state中的数据需要经过加工后再使用时，可以使用getters加工

- index.js:

```
const getters = {
    VZ(state) {
        return state.V + '赫连尾续'
    }
}
const state = {
    V: '曹问鼎'
}
```

- 组件:

```
<h1>{{ $store.getters.VZ }}</h1>
```

方法:

1. 组件读取vuex数据：`$store.state.crazyKing`

2. 组件修改vuex数据：`$store.dispatch('action中的方法名',数据)` 或 ```$store.commit('mutations中的方法名',数据)```

### 四个map方法的使用

mapState和mapGetters的作用:使插值语法可以简易的书写

- index.js:

```
const getters = {
    X() {
        return '范青雁'
    }
}
const state = {
    V: '曹问鼎'
}
```

- 组件:

```
    <h1>{{ V }}</h1>
    <h1>{{ X }}</h1>
……
import { mapState, mapGetters } from "vuex";
……
  computed: {
    ...mapState(["V"]),
    ...mapGetters(["X"]),
  }
```

mapActions和mapMutations的作用:使对话的方法可以简易的书写

- index.js:

```
const actions = {
    alliance(context) {
        context.commit('ALLIANCE');
    }
}
const mutations = {
    ALLIANCE(state) {
        state.J = '齐疯子&&洛雷魔&&崔八歧'
    },
    ALLIANCE2(state) {
        state.J = '齐疯子&&洛雷魔&&间夜咒妖'
    }
}
const state = {
    J: '崔道盛'
}
```

- 组件:

```
    <h1>{{ $store.state.J }}</h1>
    <button @click="kqj">第一联盟</button>
    <button @click="kqs">第二联盟</button>
……
import { mapActions, mapMutations } from "vuex";
……
  methods: {
    ...mapActions({ kqj: "alliance" }),
    ...mapMutations({ kqs: "ALLIANCE2" }),
  }
```

### 模块化+命名空间

使数据分类明确，更易于管理

- index.js:

```
import Vue from 'vue'
import Vuex from 'vuex'
import knifeQ from './knifeQ'
Vue.use(Vuex)

export default new Vuex.Store({
    modules: {
        knifeQ
    }
})
```

- knifeQ.js:

```
export default {
    namespaced: true,
    actions: {},
    mutations: {},
    state: {
        knifeName: '碑术刀'
    }
}
```

- 组件:

```
<h1>{{ this.$store.state.knifeQ.knifeName }}</h1>
```

## 路由(vue-router)

单页面应用(SPA)的核心

一个路由（route）就是一组映射关系（key - value），多个路由需要路由器（router）进行管理

前端路由：key是路径，value是组件

### 搭建vue-router环境

1. 安装vue-router:npm i vue-router@3 (vue2只能用vue-router3,vue3只能用vue-router4)

2. 应用插件-main.js:

   ```
   //引入vue-router
   import VueRouter from 'vue-router'
   //引入路由器
   import router from './router'
   ……
   //应用插件
   Vue.use(VueRouter);
   ……
   new Vue({
       router:router
   })
   ```

   3.创建文件：```src/router/index.js```-index.js:

   ```
   //该文件专门用于创建整个应用的路由器
   import VueRouter from 'vue-router'
   //引入组件
   import king from '../pages/king'
   
   //创建并暴露一个路由器
   export default new VueRouter({
       routes: [{
           path: '/king',
           component: king
       }]
   })
   ```

   4.创建文件:`src/pages/king.vue`-king.vue:

   ```
   <h1>西楚霸王项羽</h1>
   ```

   5.app.vue:

   ```
   <!--相当于a标签，跳转，只不过一个跳的是页面，一个跳的是组件-->
   <router-link to="/king">跳转到king</router-link>
   <!--组件将呈现在此处-->
   <router-view></router-view>
   ```

### 多级路由

- router/index.js:


```js
import kingDisciple from '../pages/kingDisciple'
……
        children: [{
            path: 'kingDisciple',
            component: kingDisciple
        }]
```

- kingDisciple:

```
 <div>无言少女少司命</div>
```

- king:

```
<h1>枭雄齐王</h1>
<router-link to="/king/kingDisciple">部下:</router-link>
<router-view></router-view>
```

### 命名路由

作用：可以简化路由的跳转(作用鸡肋，因为本来就没有简化多少，还破坏了代码的语义化)

index.js:

```js
……
        name: 'k',
        path: '/king',
……
```

app:

```vue
<router-link :to="{ name: 'k' }">跳转到king</router-link>
```

### 路由的query参数

路由组件间通信的方式之一

传递参数-app:

```vue
<router-link :to="'/king?disciple=厄毒修女大司命'">跳转到king</router-link>
……
  data() {
    return {
      disciple: "厄毒修女大司命",
    };
  }
```

接收参数-king:

```js
<div>{{ $route.query.disciple }}</div>
```

### 路由的params参数 

跟query功能一致，仅仅是在搜索框的表现形式不同

路由组件间通信的方式之一

router/index.js:

```
path: '/king/:disciple',
component: king
```

app:

```
<router-link :to="'/king/法术修女少司命'">跳转到king</router-link>
```

king:

```
<div>{{ $route.params.disciple }}</div>
```

### 路由的props配置

作用：简化路由参数的使用

- router/index.js:

```
props: true
```

- king:

```
<div>{{ disciple }}</div>
……
props: ["disciple"]
```

### ```<router-link>```的replace属性

作用：控制历史记录写入方式

浏览器的历史记录有两种写入方式：分别为```push```和```replace```，```push```是追加历史记录，```replace```是替换当前记录。路由跳转时候默认为```push```

开启```replace```模式：`<router-link replace to="/king">跳转到king</router-link>`

注:replace属性必须加在所有属性的前面才奏效

### 历史记录进退

```js
this.$router.forward() //前进
this.$router.back() //后退
this.$router.go() //可前进也可后退
```

### 编程式路由导航

作用：不仅限于使用```<router-link> ```实现路由跳转，让路由跳转更加灵活

app:

```
<button @click="knifeWolf">点我跳转king</button>
……
  methods: {
    knifeWolf() {
      this.$router.push({
        path: "/king",
      });
    },
  }
```

### 缓存路由组件

作用：让不展示的路由组件保持挂载，不被销毁

app:

```vue
<keep-alive include="king">
      <router-view></router-view>
</keep-alive>
```

### 两个新的生命周期钩子

作用：路由组件所独有的两个钩子，用于捕获路由组件的激活状态

注:这两个钩子函数需要配合keep-alive标签使用，否则无效果

1. ```activated```路由组件被激活时触发
2. ```deactivated```路由组件失活时触发

### 路由守卫

作用：对路由进行权限控制

分类：全局守卫、独享守卫、组件内守卫

- 全局守卫-index.js:


```js
……
const router = new VueRouter({
    ……
        meta: { title: '我的刀将杀死一切' }
……

//全局前置路由守卫：初始化的时候被调用，每次路由切换之前被调用(点击进入，但未进入时执行)
router.beforeEach((to, from, next) => {
    if (localStorage.getItem('knife') === '野太刀') {
        next()
    }
})

//全局后置路由守卫: 初始化的时候被调用，每次路由切换之后被调用(进入后执行)
router.afterEach((to) => {
    document.title = to.meta.title || 'vue';
})

export default router
```

- 独享守卫-index.js:


```js
……
        path: '/king',
        component: king,
        //独享守卫
        beforeEnter: ((to, from, next) => {
            if (localStorage.getItem('knife') === '野太刀') {
                next()
            }
        })
```

- 组件内守卫-king：


```js
//进入守卫：通过路由规则，进入该组件时被调用
beforeRouteEnter(to, from, next) {
    if (localStorage.getItem("knife") === "野太刀") {
      next();
    }
}
//离开守卫：通过路由规则，离开该组件时被调用
beforeRouteLeave(to, from, next) {
    next();
}
```

### 路由器的两种工作模式

对于一个url来说，什么是hash值？—— #及其后面的内容就是hash值。

hash值不会包含在 HTTP 请求中，即：hash值不会带给服务器。

- hash模式：带#号;兼容性较好;若手机app校验严格，地址会被标记不合法
- history模式：兼容性较差;需要后端人员协助

把默认的hash模式切换为history模式-index.js:

```
const router = new VueRouter({
    mode: 'history'
    ……
})
```

## Element-UI组件库(vue2)

1. 安装:npm i element-ui -S
2. 引入Element组件-main.js:

```
//完整引入
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css'
……
Vue.use(ElementUI)
```

使用-king:

```
<el-input-number v-model="num" :min="1" :max="10" label="描述文字"></el-input-number>
……
  data() {
    return {
      num: 1,
    };
  }
```

### 按需引入

1. 安装 babel-plugin-component：npm install babel-plugin-component -D
2. 引入Element组件中的InputNumber组件-main.js:

```
import { InputNumber } from 'element-ui'
……
Vue.use(InputNumber)
```

## Element-UI组件库(vue3)

1. 安装:npm install element-plus --save
2. 引入Element组件-main.js:

```
//完整引入
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'
……
createApp(App).use(ElementPlus).mount('#app')
```

使用-king:

```
<el-button>疯子</el-button>
```

