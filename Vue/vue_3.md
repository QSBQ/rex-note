# Vue3快速上手

<img src="https://user-images.githubusercontent.com/499550/93624428-53932780-f9ae-11ea-8d16-af949e16a09f.png" style="width:200px" />

## 创建Vue3.0

- vue create 文件名(和Vue2一样，但需要下面这行代码)
- npm i --save-dev vue-loader-v16

## 拉开序幕的setup

app:

```
<h1>{{ name }}</h1>
……
  setup() {
    let name = "洛岚歌";
    return {
      name,
    };
  }
```

##  ref函数

定义一个响应式数据

app:

```
<h1>{{ name }}</h1>
<button @click="knife">修改为她的刀</button>
……
import { ref } from "vue";
……
  setup() {
    let name = ref("洛岚歌");
    function knife() {
      name.value = "往生";
    }
    ……
  }
```

## reactive函数 

定义一个<strong style="color:#DD5145">对象类型</strong>的响应式数据

app:

```
<h1>{{ knife.name }}</h1>
<button @click="queen">修改为她的刀</button>
……
import { reactive } from "vue";
……
  setup() {
    let knife = reactive({
      name: "八方死神玉",
    });
    function queen() {
      knife.name = "往生";
    }
……
  }
```

### reactive对比ref

-  ref用来定义基本类型数据;reactive用来定义对象或数组类型数据

-  ref操作数据需要```.value```;reactive不需要```.value```

*注：ref也可以用来定义对象或数组类型数据, 但它内部会调用reactive执行*

## computed计算属性(函数)

app:

```js
<h1>{{ ac }}</h1>
……
import { computed } from "vue";
……
    let ac = computed(() => {
      return knife.value + knife2.value;
    });
……
```

## watch监视属性(函数)

sword.vue:

```
<h1>{{ knife }}</h1>
……
import { watch } from "vue";
……
    let ac = watch(knife,(newValue, oldValue) => {
        console.log(newValue, oldValue);
      },{ immediate: true })
……
```

## watchEffect函数

- watchEffect可以完全替代watch

sword.vue:

```js
<h1>{{ knife }}</h1>
<button @click="queen">点我线数增1</button>
<h1>线数:{{ n9m9 }}</h1>
……
import { watchEffect } from "vue";
……
    function queen() {
      this.knife += "线";
    }
    watchEffect(() => {
      knife.value;
      n9m9.value += 1;
    });
……
```

## vue3的生命周期

vue2的生命周期:

```
beforeCreate:创建之前
created:创建之后
beforeMount:装载之前
mounted:装载之后
beforeUpdate:更新之前
updated:更新之后
beforeDestroy:销毁之前
destroyed:销毁之后
```

```
创建之前：在内存中创建vue实例
创建之后：data、methods等配置项初始化
装载之前：模板编译
装载之后：挂载到页面
更新之前：更新存储的数据
更新之后：更新页面的数据
销毁之前：执行销毁命令，但未销毁，vue实例还可以用
销毁之后：销毁之后，vue实例完全被清除了，不可以使用了
```

vue3的生命周期:

```
setup:创建之前与创建之后
onBeforeMount:装载之前
onMounted:装载之后
onBeforeUpdate:更新之前
onUpdated:更新之后
onBeforeUnmount:卸载之前
onUnmounted:卸载之后
```

- vue2到vue3,生命周期钩子从8个变为7个，其中两个被合并，两个被更名:`beforeCreate`和`created`被合并为`setup`,`beforeDestroy`和`destroyed`分别被更名为`onBeforeUnmount`、`onUnmounted`

使用-sword:

```
import {onMounted} from "vue";
……
  setup() {
……
    onMounted(() => {
      ……
    });
  }
……
```

## hook函数(类似于mixin混合)

- 创建文件`src/hook/hammer.js`-hammer.js:

```
import { ref } from 'vue'
export default function() {
    let knife = ref(5);
    return {
        knife
    }
}
```

- sword.vue:

```
<h1>{{ ac.knife }}</h1>
……
import hammer from "./../hook/hammer";
……
  setup() {
    let ac = hammer();
    return {
      ac,
    };
  }
```

## toRefs

简化插值语法

toRefs的使用-king:

```
<h1>{{ name }}</h1>
<h1>{{ age }}</h1>
……
import { reactive, toRefs } from "vue";
……
    let knife = reactive({
      name: "天丛云剑",
      age: 3000,
    });
    return {
      knife,
      ...toRefs(knife),
    }
……
```

## 不常用的API

### shallowReactive 与 shallowRef

- shallowReactive：只处理第一层数据的响应式（浅响应式）
- shallowRef：只处理基本数据类型的响应式


### readonly 与 shallowReadonly

作用:让数据不被修改

- readonly: 让一个响应式数据变为深只读
- shallowReadonly：让一个响应式数据变为浅只读

### toRaw 与 markRaw

- toRaw：
  - 作用：将一个由```reactive```生成的<strong style="color:orange">响应式对象</strong>转为<strong style="color:orange">普通对象</strong>
  - 使用场景：用于读取响应式对象对应的普通对象，对这个普通对象的所有操作，不会引起页面更新。
- markRaw：
  - 作用：标记一个对象，使其永远不会再成为响应式对象。
  - 应用场景:
    1. 有些值不应被设置为响应式的，例如复杂的第三方类库等。
    2. 当渲染具有不可变数据源的大列表时，跳过响应式转换可以提高性能

### customRef

- 作用：创建一个自定义的 ref，并对其依赖项跟踪和更新触发进行显式控制


### provide 与 inject

<img src="https://v3.cn.vuejs.org/images/components_provide.png" style="width:300px" />

- 作用：实现<strong style="color:#DD5145">祖与后代组件间</strong>通信


## 响应式数据的判断

- isRef: 检查一个值是否为一个 ref 对象
- isReactive: 检查一个对象是否是由 `reactive` 创建的响应式代理
- isReadonly: 检查一个对象是否是由 `readonly` 创建的只读代理
- isProxy: 检查一个对象是否是由 `reactive` 或者 `readonly` 方法创建的代理

## Composition API 的优势

- vue2的API:Options API
- vue3的API:Composition API

使用传统OptionsAPI中，新增或者修改一个需求，就需要分别在data，methods，computed里修改;

使用Composition API,我们可以更加优雅的组织我们的代码，函数。让相关功能的代码更加有序的组织在一起

## 新的组件

### 1.Fragment

- 在Vue2中: 组件必须有一个根标签
- 在Vue3中: 组件可以没有根标签, 内部会将多个标签包含在一个Fragment虚拟元素中
- 好处: 减少标签层级, 减小内存占用

### 2.Teleport瞬间移动

Teleport能够将它包裹的html结构移动到指定位置

```vue
<teleport to="body">
    <h1>恕瑞玛，你的皇帝回来了！！！</h1>
</teleport>
```

### 3.Suspense

异步组件还未加载完成时，展现其它内容

- 异步引入组件-app:


```js
import { defineAsyncComponent } from "vue"; //静态引入
const king = defineAsyncComponent(() => import("./components/king")); //异步引入
```

- app:

  ```vue
  <Suspense>
      <template v-slot:default>
        <king />
      </template>
      <template v-slot:fallback>
        <h2>暂等</h2>
      </template>
  </Suspense>
  ```

## 其他

### 全局API的转移

vue2中像是注册全局组件、注册全局指令等使用全局API的方式，vue3做出了更改，vue3将全局API(Vue.xxx)调整到了应用实例（```app```）上

| 2.x 全局 API（```Vue```） | 3.x 实例 API (`app`)                        |
| ------------------------- | ------------------------------------------- |
| Vue.config.xxxx           | app.config.xxxx                             |
| Vue.config.productionTip  | <strong style="color:#DD5145">移除</strong> |
| Vue.component             | app.component                               |
| Vue.directive             | app.directive                               |
| Vue.mixin                 | app.mixin                                   |
| Vue.use                   | app.use                                     |
| Vue.prototype             | app.config.globalProperties                 |

### 其他改变

- data选项应始终被声明为一个函数。

- 过度类名的更改：

  - Vue2.x写法

    ```css
    .v-enter,
    .v-leave-to {
      opacity: 0;
    }
    .v-leave,
    .v-enter-to {
      opacity: 1;
    }
    ```

  - Vue3.x写法

    ```css
    .v-enter-from,
    .v-leave-to {
      opacity: 0;
    }
    
    .v-leave-from,
    .v-enter-to {
      opacity: 1;
    }
    ```

- <strong style="color:#DD5145">移除</strong>keyCode作为 v-on 的修饰符，同时也不再支持```config.keyCodes```

- <strong style="color:#DD5145">移除</strong>```v-on.native```修饰符

  - 父组件中绑定事件

    ```vue
    <my-component
      v-on:close="handleComponentEvent"
      v-on:click="handleNativeClickEvent"
    />
    ```

  - 子组件中声明自定义事件

    ```vue
    <script>
      export default {
        emits: ['close']
      }
    </script>
    ```

- <strong style="color:#DD5145">移除</strong>过滤器（filter）


