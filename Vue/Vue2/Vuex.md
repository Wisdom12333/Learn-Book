# Vuex

专门在Vue中实现集中式状态（数据）管理的一个Vue插件，对Vue应用中多个组件的共享状态进行集中式的管理（读/写），也是一种组件间通信方式，并且适用于任意组件间通信。

## 什么时候使用

1. 多个组件依赖同一状态。
2. 来自不同组件的行为需要变更同一状态。

## 原理图

![vuex](https://vuex.vuejs.org/vuex.png)

## 使用

首先需要创造`store`

```js
import { createStore } from "vuex";

export default createStore({
  state: {
    //由vuex维护的数据
  },
  mutations: {
    //对state中的数据进行加工
  },
  actions: {
    //加工前的操作
  },
  modules: {},
});
```

在`main.js`中，使用`Vue.use()`方法添加`store`。

要对`vuex`中数据进行管理，使用`this.$store.dispatch("funcName", param)`来进行调用`actions`中的相应方法。

之后该方法进行业务逻辑的处理，然后调用`context.commit("FUNCNAME",param)`调用`mutations`中对应方法。

如果数据无需进行业务的处理，可以在组件中直接使用`this.$store.commit("FUNCNAME",param)`。

> 在`mutations`中，尽量只对`state`进行操作。