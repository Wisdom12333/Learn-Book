# Vue3

## setup

 Vue3中的新配置项，它的值是一个函数。

它是所有Composition API（组合API）“表演的舞台”。

`setup`的调用发生在 `data` property、`computed` property 或 `methods` 被解析之前，所以它们无法在 `setup` 中被获取。

组件中所用到的数据、方法等，都要配置在其中。

`setup`有两种返回值

- 一个对象，对象中属性、方法在模版中都可以直接使用。
- 一个渲染函数，可以自定义渲染内容。

```vue
<script>
	export default {
    name: "App",
    setup(){
      //数据
      let name = "Jack";
      let age = 18;
      
      //方法
      function sayHello(){
        console.log("Hello");
      }
      
      return {
        name,age,
        sayHello,
      }
    }
  }
</script>
```

> 尽量不要将setup与Vue2的配置进行混用。

setup 可以接收两个参数,首先是`props`,收到的是父组件传递且组件内部声明接收的`props`.

然后是`context`,其中包括`attrs`,`emit`和`slots`.



## ref函数

直接添加入`setup`函数中的数据,Vue并不会进行响应式的数据代理.

需要使用到`ref`函数.

```Vue
<script>
    import {ref} from 'vue'
    export default {
        name: 'App',
        setup(){
            //data
            let name = ref("Jack");
            let age = ref(18);
            let job = ref({
                type:"1",
                salary:"2K",
            })
            
            //methods
            function changeInfo(){
                name.value = "Hans";//要使用data.value才能改变数据.
                job.value.type = "2";//对象中的数据不需要再次添加value.
            }
            return {
                name,age,changeInfo
            }
        }
    }
</script>
```

## reactive函数

定义一个对象类型的响应式数据.

使用reactive函数代理的数据不同于ref函数,没有value属性.

因为reactive是通过`Proxy`来实现响应式的,并通过`Reflect`操作源对象内部数据.



## `computed`计算属性

使用`computed()`方法,声明一个计算属性.

```js
person.fullName =  computed(()=>{
    return person.firstName + '-' + person.lastName;
})
```

可以直接向一个响应式的对象中添加一个计算属性,它同样会被Vue侦测.

## `watch`监视

```js
watch(data,(newValue,oldValue)=>{
    //data为基本类型
},{deep:true});//监视的配置为第二个参数
//监视多个数据
watch([data1,data2],(newValue,oldValue)=>{
    //收到的参数也是数组
});

```

`watch`监视的由`reactive`定义的响应式数据,

- 无法正确获取到 oldValue,而`ref`可以.

- 并且会强制深度监视.

同时,如果要监视`reactive`对象中的某个属性,需要如下写法:

`watch(()=>person.name,(newValue,oldValue)=>{})`

### watchEffect

智能监视,

```js
watchEffect(()=>{
    ...
})
```

会自动监视,其中使用到的响应式数据.





## 生命周期钩子

在Vue3中,依然可以使用Vue2的生命周期钩子.

只是Vue3删除了`destroy`,使用`unmount`作为代替.

在Vue3中,增加了组合式API,在`setup`中生命周期钩子有所不同,需要向其添加`on`作为前缀.

![image-20211230115844963](image-20211230115844963.png)



## 自定义hook函数

hook  -- 对组合式API进行封装的函数.

简单来说就是将逻辑封装到单独的`.js`文件中,在`.vue`中引入并且使用.	

## toRef与toRefs

假设存在响应式对象`person`,含有一个`name`属性,

> `const name = person.name`,得到的是一个非响应式的数据.

如果要获取响应式的属性,使用`const name = toRef(person,'name')`写法.

toRefs将处理一个对象的所有属性.

### shallowReactive和shallowRef

shallowReactive只处理对象类型,shallowRef只处理普通类型.
