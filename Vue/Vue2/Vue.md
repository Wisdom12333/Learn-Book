# Vue
> 一套用于**构建用户界面**的**渐进式**JavaScript框架

## Vue实例

### 创建

``` js
const vm = new Vue({
	el:'#id', //element,指定Vue组件绑定的容器
	data:{
		name:'Vue'
	}
})
```

> 一个Vue实例,能且只能对应一个容器

Vue组件与容器的绑定,可以通过`vm.$mount('')`实现.

data的 #函数式写法

``` js
data:function(){
    return{
        //一个对象
    }
}
//函数式的data写法不能使用箭头函数
//同样适用于Vue管理的函数
```

#### 数据代理

 **`Object.defineProperty()`**

这个方法可以控制对象属性

Ex:

``` js
let person = {
    name : 'uu',
}
let number = 18
Object.defineProperty(person,//控制的对象
                     'age',{//属性
    //当读取person.age时,调用get方法
	get(){
        return number;//使age与number绑定
    },
    //当修改person.age时,调用set方法
    set(value){
        number = value;
    }
})
```

Vue实际上是对 data 做了数据代理.

### 数据监视

Vue中对于数据的监视,是基于getter,setter方法的监视.

数据改变时调用setter,改变数据的值,并且更新界面.

向对象中添加对象,Vue默认不添加监视.

要使用

```js
Vue.set(target, propertyName/index, value)
//或者
vm.$set()
```



对于数组,Vue重写了数组的常用方法.

### 模板语法

使用`v-bind:`对HTML标签属性进行绑定.

``` html
<div id="root">
    <div v-bind:id="id"></div>
	<!--<div :id="id"></div> 简写-->
</div>
<script type="text/javascript">
        const vm = new Vue({
            el: '#root',
            data: {
                id: 'vbind'
            }
        })
</script>
```

v-bind是单向绑定,Vue实例中对象的改变会导致页面的改变,但是页面数据的改变不会影响对象.

要进行双向绑定,使用`v-model`.需要注意的是,`v-model`只能使用于表单类的标签,即标签必须要有**value**属性.

> 所以当使用`v-model`对标签的value属性进行绑定时,可以将
>
> `v-model:value=""`简写为
>
> `v-model=“”`



### 事件

使用`v-on:click=""`(`@click=""`)

绑定的方法需要在Vue实例中定义

```js
new Vue({
    methods:{
        show(){
            //方法默认拥有参数event
            console.log(event);
        }
    }
})
```

#### 事件修饰符

- `@click.prevent=""`阻止标签的默认行为.

- `.stop`:阻止事件冒泡
- `.once`:事件只触发一次(一次性行为)
- `.capture`:使用事件的捕获模式
- `.self`:只有`event.target`是当前操作的元素时才触发事件
- `.passive`:事件默认行为立即执行,无需等待回调

#### 键盘事件

``` html
<input type="text" placeholder="回车输入"
       @keyup.enter="">
<!-- keydown键盘按下触发,keyup键盘起来触发-->
```

按键别名

- `enter` => 回车
- `delete` => 删除和退格
- `esc`
- `space`
- `tab`
- `up`
- `down`
- `left`
- `right`

### 计算属性

对于任何复杂逻辑,都应当使用**计算属性**.

``` js
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      //当只有getter时,可以简写为reversedMessage(){}
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  }
})
```

> 计算属性的值在每次`get`被调用时,都会生成缓存,所以计算属性在页面多次使用,只会调用一次`get`

`get`调用的条件:

- 页面初始化时
- 依赖的数据发生变化时

计算属性同样可以增加**setter**.

```js
// ...
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
// ...
```

这样使用时,如果运行`vm.fullName = 'John Doe'`,调用setter,可以更新`vm.firstName` 和 `vm.lastName`.

计算属性并不能看作一个Vue实例的对象,实际上只是在生成时,产生了一个同名的对象,对象的值为计算属性setter的返回值.所以并不能使用对象属性的方法.

### 侦听器

计算属性在大多数情况下更合适,但有时也需要一个自定义的侦听器. Vue 通过 `watch` 选项提供了一个更通用的方法,来响应数据的变化. 当需要在数据变化时执行异步或开销较大的操作时,这个方式是最有用的.

```js
const vm = new Vue({
  el: '#watch-example',
  data: {
    question: ''
  },
  watch: {
    // 如果 `question` 发生改变，这个函数就会运行
    question(newValue, oldValue) {
      console.log(question);
    }
    //非简写形式
    question:{
      handler(newValue, oldValue) {
        console.log(question);
      }
  	} 
  }
}
//JPI写法
vm.$watch('question',function(newValue, oldValue){
    console.log(question);
})
```

配置`deep:true`使能深度监视.即对象中属性值改变时也触发监视.

例如,`number.a`的值改变,`deep:false`时不触发,为`true`时触发.

### 过滤器

过滤器可以用于对数据进行处理.

一般要通过`|`管道符使用

`{{time | timeFormater}}`.

```js
const vm = new Vue({
  el: '#watch-example',
  data: {
    time: ''
  },
  filters:{
      timeFormater(value){
          console.log(value);
          return '';
      }
  }
}
```

过滤器可以添加参数,但是默认第一个参数为管道符前的数据.  

#### 全局过滤器

在Vue实例中定义的过滤器为局部过滤器.

如果其他实例需要使用,要定义全局过滤器.

```js
Vue.filter('timeFormater',function(value){
	return '';
})
```

### 条件渲染

#### v-show

`v-show="state"`控制标签显示与否.

不显示的底层原理其实是`style="display:none"`

#### v-if&v-else

`v-if="state"`也可以控制,但是实现是在HTML渲染时跳过.

相对于`v-show`,`v-if`更加消耗性能.在频繁的变化下,应该使用`v-show`.

``` html
<!--v-if,v-else-if,v-else-->
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
```

如果`v-if`和`v-else`中拥有相同的元素,Vue为了高效的渲染,会复用已有元素.

所以如果同一个`<input>`标签,当切换时不会替换内容.

此时,可以向标签添加属性`key`来表示其独立性.

> 如果多个元素需要使用相同的条件渲染,为了不破坏页面结构,可以使用`<template>`标签,最终渲染时将会排除它.
>
> ``` html
> <template v-if="ok">
>   <h1>Title</h1>
>   <p>Paragraph 1</p>
>   <p>Paragraph 2</p>
> </template>
> ```

### 列表渲染

使用`v-for`渲染列表

```html
<ul id="example-1">
  <li v-for="item in items" :key="item.message">
    {{ item.message }}
  </li>
</ul>
<!-- 指定key作为标识 -->
<!-- 'in' 可以使用 'of' 代替-->
```

`items`是被Vue管理的数组.

`v-for=“(item, index) in items”`,添加第二个参数,作为循环的索引.

同样可以使用`v-for`遍历对象,这时第一个参数为对象的`value`,第二个为对象的`key`.

字符串也可以通过`v-for`遍历,参数为 `char,index`.

> 另外可以使用`v-for="(number,index) of 10"`,输出一个数列.

#### Key

用于标识节点,使其具有独立性.

当列表无需更新,或者更新不会破坏原有的顺序结构时,可以使用`index`作为key,如果列表需要更新,不建议使用.应该使用数组数据的唯一标识.

### Vue表单

对于表单中的元素,使用`v-model`进行数据的双向绑定.

特殊的元素,需要进行不同的绑定.

#### 单选

对于单选框,使用`v-model`绑定一个字符型变量,同时需要对元素添加`value`属性,选择之后变量值与value绑定.

```html
<form>
    性别:
    男<input type="radio" name="sex" v-model="userInfo.sex" value="0">
    女<input type="radio" name="sex" v-model="userInfo.sex" value="1">
</form>
```

#### 多选

和单选相似,但是必须绑定一个数组型的变量.

如果绑定字符型变量,则变量为`true`或`false`.

#### 选择

形如单选

#### `v-model`

同样可以添加修饰符,来进行控制.

- `lazy`:元素失去焦点时才收集.
- `number`:输入的字符转为数字存入变量.
- `trim`:消除字符前后的空格.

### 内置指令

#### `v-text`

向标签插入文本.会替换原来的内容.但是不会渲染HTML.

#### `v-html`

与`v-text`对应,会渲染HTML.

#### `v-cloak`

页面渲染时移除该属性.

#### `v-once`

初始渲染之后,无论其中数据如何变化,不再渲染.

#### `v-pre`

跳过所在节点的编译.可以加快编译.

### 自定义指令

#### 函数型

``` js
new Vue({
    directives:{//指令对象
    	big(element,binding){//(元素,指令实例)
    		element.innerText = binding.value * 10;
		}
	}
})
```

#### 对象型

``` js
new Vue({
    directives:{
        fbind:{
            bind(){},//指令与元素绑定时调用
            inserted(){},//指令所在元素被插入页面时调用
            update(){}//指令所在模板被重新解析时调用
        }
    }
})
```

### Vue生命周期

四对生命周期钩子

**before**\*, \***ed**

- create
- mount
- update
- destroy

 #### `mounted`

``` js
new Vue({
    mounted(){
        console.log('Vue完成模板解析并挂载真实DOM后调用此方法');
    }
})
```

#### `updated`

```js
new Vue(){
    updated(){
       console.log('当页面数据变化,Vue重新解析时触发'); 
    }
}
```

#### `destroyed`

```js
new Vue(){
    destroyed(){
        console.log('页面销毁完成时调用,不影响之前的工作成果 ');
    }
}
```



## Vue组件

### 非单文件组件

一个HTML文件中有多个组件

```js
//创建一个组件
const school = Vue.extend({
    template:`
      <div>
        <h2>{{schoolName}}</h2>
        <h2>{{address}}</h2>
      </div>
    `,
    data(){
        return {
            schoolName:'HYNU',
            address:'HN'
        }
    },
    methods:{
        
    }
})
//组件的简写形式，要在脚手架中才能使用
const s = {
  name:"123"
}

new Vue({
    el:'#root',
    componets:{
        comp: school//注册组件(局部)
        //或者
        school//直接注册(局部)
    }
})

Vue.component('school',school);//q
```

```html
<!--使用组件-->
<div id="root">
    <comp></comp>
</div>
```



#### 组件嵌套

组件中注册组件

```js
//子组件student要提前定义
const student = Vue.extend({
  name:'student',
  template:``,
  data(){
    
  }
})
//父组件school
const school = Vue.extend({
  name:'school',
  template:``,
  data(){
    
  },
  //注册组件
  component:{
    student
  }
})
```

子组件使用，要写在父组件的`template`中。

#### VueComponent

组件的实质，是一个构造函数。

通过`Vue.extend`生成。



### 单文件组件

.Vue 文件结构，一个组件就是一个.Vue文件

``` vue
<template>
	<!--HTML-->
</template>

<script>
  //引入组件
  import CompName from ''
  
	export default {//暴露组件，使能被引用
    name:'',
    data(){
      
    },
    methods:{
      
    },
    components:{
      //注册组件
    }
  }
</script>

<style>
	/* CSS */
</style>
```

#### `main.js`

vm要注册在这个文件中。

```js
import App from './App.vue'

new Vue({
  el:'#root',//需要创建.html文件
  components:{App},
})
```



## 使用脚手架@vue/cli

> **CLI**：Command Line Interface

> Note: 
>
> - vue新建项目不能使用大写字符

#### Vue中的`ref`

在标签上添加`ref`属性，以替代`id`属性使用。

通过`this.$refs.*`获取，可以获取到组件的实例对象。

#### `props`配置

通过向组件中配置`props`项，使组件可以通过标签属性接受数据。

```vue
<script>
	export default {
    name: 'Student',
    data(){
      return {
        msg: '123'
      }
    },
    props:['name','age','sex'],//简单接收
    props:{//接收并进行类型检查
    	name: String,
      age: Number,
      sex: String
  	},
    props:{//完整写法
      name:{
        type:String,
        required:true
      },
      age:{
        type:Number,
        default:99
      },
      sex:{
        type:String,
        required:true
      }
    }
  }
</script>
```

但是不建议对`props`中的属性值进行修改。

所以应该在`data`中配置。

```vue
<script>
  export default {
    name: 'Student',
    data(){
      return {
        name: this.propName
      }
    },
    props:['propName']
  }
</script>
```

#### `mixins`配置

可以提取组件中的公共配置，以混合形式加入到组件中。

```vue
<script>
  import {mix} from '../mix'
	export default {
    mixins:[mix]
  }
</script>
```

`mix`:

```js
export const mix = {
  methods:{
    
  }
}
```

以上是局部混合，如果要全局混合，可以在`main.js`中添加

```js
//main.js
import {mix} from '../mix'
Vue.mixin(mix)//使用此方法实现全局混合
```

#### 插件

Vue中可以定义插件。

```js
//plugins.js
export default {
  install(){//插件必须使用install方法。
    
  }
}
```

要使用同样需要在`main.js`中进行`import`。之后使用`Vue.use()`方法进行使用。





## MVVM模型

### M: 模型(Model)

对应**data**中的数据

### V: 视图(View)

模板

### VM: 视图模型(ViewModel)

Vue实例对象



