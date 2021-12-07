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
import { Vue } from "vue";
import { Vuex } from "vuex";

Vue.use(Vuex);

const state:{};
const actions:{};
const mutations:{};
const getters:{};

export default new Vuex.Store({
  state,//由vuex维护的数据
  mutations,//对state中的数据进行加工
	actions,//加工前的操作
	getters,//类似Vue计算属性，可以对state数据加工并且返回一个值
});
```



要对`vuex`中数据进行管理，使用`this.$store.dispatch("funcName", param)`来进行调用`actions`中的相应方法。

之后该方法进行业务逻辑的处理，然后调用`context.commit("FUNCNAME",param)`调用`mutations`中对应方法。

如果数据无需进行业务的处理，可以在组件中直接使用`this.$store.commit("FUNCNAME",param)`。

> 在`mutations`中，尽量只对`state`进行操作。



## mapState&mapGetters

如果组件内，有多个地方需要用到`state`中对属性，可以使用`mapState`进行映射。

``` vue
<script>
  import { mapState,mapGetters } from 'Vuex'
  export default {
    name: 'Component',
    computed: {
      ...mapState({接收的名字:'需要的属性名',..,..}),
      //数组写法
      ...mapState(['需要的属性名','..','..']),
    }
  }
</script>

```

同理，`getters`也有方法，使用`...mapGetters({接收的名字:'需要的getter名'})`,他也有同样的数组写法。



## mapActions&mapMutations

和`mapState`具有接近的用法，但是需要给生成的方法调用时添加参数。

```vue
<template>
	<button @click="func(1)">click</button>
</template>
<script>
	export default {
    name:"Component",
    methods: {
      ...mapMutations({func:"FUNC"}),
      //func(){ this.$store.commit("FUNC",1); }
    }
  }
</script>
```



## Vuex模块化

使用Vuex中的`modules`进行模块化管理，使各个模块负责各自的功能。

```js
const module1 = {
  namespaced:true,//配置使map能识别
  //state,actions,mutations
};
const module2 = {
  //state,actions,mutations
};

export default new Vuex.Store({
  modules:{
    a: module1,
    b: module2
  }
})
```

使用可以按照原方法使用，也可以单独提取某个模块，如`...mapState('module1',['..','..'])`。

`commit`方法，要针对方法添加模块名，如`this.$store.commit('moduleName/funcName',value)`。

另外，也可以将不同模块配置在不同的js文件中，并暴露，在`store`中进行导入。