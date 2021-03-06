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



## 组件自定义事件

想要在父子组件中传递数据，除了通过父组件给子组件传递函数类型的`props`（子向父传递数据）

也可以使用自定义事件实现。

```vue
<template>
	<!--App.vue-->
	<Student v-on:case="demo"></Student>
</template>
<script>
	export default {
    name: 'App',
    methods: {
      demo(param){}
    }
  }
</script>
```

```vue
<template>
	<!--Student-->
	<button @click="func">
    button
  </button>
</template>
<script>
	export default {
    name: 'Student',
    methods: {
      func(){
        this.$emit('case','param')//通过这个方法来触发自定义事件
      }
    }
  }
</script>
```

另外，也可以给`Student`组件绑定`ref="Student"`属性，当App挂载完毕时，使用

> `this.$refs.student.$on('case',this.demo)`
>
> 将`$on`替换为`$once`绑定仅可触发一次的事件。

来对Student绑定事件。

### 解绑自定义事件

使用`this.$off('case')`来解绑事件。

> 此方法每次仅可解绑单个事件。

若要解绑多个，参数需使用数组类型，即`this.$off(['case','case2'])`。

或者直接使用`this.$off()`解绑所有自定义事件。

### Note

需要注意的是，当在组件标签上使用原生事件的时候，需要添加`.native`修饰符，否则在渲染时会作为自定义事件。

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

#### `$nextTick`

`this.$nextTick(回调函数)

Vue上的这个方法，接收一个函数，在Vue更新完DOM节点之后，执行该函数。

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

#### `scoped`样式

多个组件的样式在使用时会汇总到一起.

相同的样式会根据引入组件的顺序,使用最后引入的样式.

可以通过在`style`标签上添加`scoped`属性解决.

```vue
<style scoped>
    .demo{
        background-color:orange 
    }
</style>
```



## 全局数据总线

用于任意组件间通信。

```js
new Vue({
  el:'app',
  render: h=>h(app),
  beforeCreate(){
    Vue.prototype.$bus= this//安装全局事件总线。
  }
})
```

当两个组件通信时，A组件通过`$on`向`$bus`添加自定义事件，之后B组件使用`$emit`事件触发，并向A组件发送数据。



## 消息订阅与发布

需要接受数据的组件，进行消息的订阅，由提供数据的组件发布消息，并传输数据。

需要使用第三方库，例如`pubsub`。

`npm i pubsub-js`

使用时组件需要引入`import pubsub from 'pubsub-js'`。

### 消息订阅

```js
this.pubId = pubsub.subscribe('msgName',function(msgName,data)=>{
	//todo                 
})
//取消订阅
pubsub.unsubscribe(this.pubId);
```

### 消息发布

```js
pubsub.publish('msgName',data);
```



## 过渡与动画

### 动画

Vue中使用`transition`标签，对其中内容添加动画。

> `transition`标签内部，只能有一个元素。
>
> 多个元素要使用`transition-group`，并且为每个元素添加`key`属性。

当标签中内容显示或隐藏时，分别会触发`.v-enter-active`和`.v-leave-active`绑定的动画。

使用标签的`name`属性，其值替换css中的`v`。即`<transition name="hi"></transition>`，使用`.hi-enter-active`。

添加`appear`属性，使界面初始化时触发进入动画。

### 过渡

```vue
<template>
	<div>
  <transition>
    <h1>HI</h1>
  </transition>
  </div>
</template>
<style scoped>
  .v-enter-active, .v-leave-active{
    transition: 0.5s linear;/*设置动画时间并使其匀速*/
  }
  /*进入的起点，离开的终点*/
  .v-enter, .v-leave-to{
    transform: translateX(-100%);
  }
  /*进入的终点，离开的起点*/
  .v-enter-to, .v-leave{
    transform: translateX(0);
  }
</style>
```

### 第三方动画库

可以使用`Animate.css`库。通过`npm install animate.css --save`安装。

```vue
<template>
	<div>
  <transition 
    name="animate_animated animate_bounce"
    enter-active-class="**"
    leave-active-class="**">
    <h1>HI</h1>
  </transition>
  </div>
</template>
<script>
  import 'animate.css'
  export default {
    name: 'Test',
  }
</script>
```



## Vue解决Ajax跨域问题

使用vue-cli脚手架开启前端相同端口的代理服务器。

数据经过代理服务器来进行转发，避免跨域问题。

在`vue.config.js`中，配置如下属性。

```js
//开启代理服务器
devServer: {
  proxy: 'http://localhost:8181'//后端服务端口号
}
```

需要注意的是，只有代理服务器不存在的数据，才会进行转发，否则会返回代理服务器的数据。

要避免这个问题，如下

```js
devServer: {
  proxy: {
    '/**':{//前缀，当代理服务器收到此前缀的请求时，进行转发
      target:"**",//目的服务器
      pathRewrite:{"^/**":""},//设置路径重写，去掉前缀。
      ws： true，//用于支持websocket，默认true
      changeOrigin：true //请求改变其自身host，使其来源与目的端口相同，默认true。
    }，
    '/demo':{
      target:'**',//代理转发向另一台服务器
    }
  }
}
```

## Vue插槽

### 默认插槽

当一个组件多次使用，但是其中元素并不相同（一个显示列表，另一个显示图片），可以使用插槽。

**父组件向子组件指定位置插入HTML结构。**

对组件使用完整标签代替自结束标签，并将元素置于其中。

```vue
<template>
	<div>
    <Component>
      <img src="https://www.ddd.com/image" alt=""/>
  	</Component>
  </div>
</template>
```

在组件中使用`<slot></slot>`标签，告诉Vue对内容进行插入。

`slot`标签中可以设置默认内容，当没有数据传输时，显示默认内容。

### 具名插槽

如果有多个元素需要添加入组件中不同位置，应该使用具名插槽。

向插入元素中添加`slot`属性，同时向组件`slot`标签添加`name`属性与之对应。

如果插入元素具有相同的`slot`属性，则会放入同一位置。

如果插入元素最外层使用了`template`标签，可以使用`v-slot:name`代替`slot="name"`。

### 作用域插槽

如果使用`slot`时，外部标签中需要使用组件中的数据，向组件的`slot`标签添加自定义的绑定数据。

`<slot :games="games"></slot>`

外部标签使用`<template scope="name"></template>`包裹，获取数据`name.games`。

可以使用`<slot-scope="{games}">`代替，他们有相同的效果。

> 可以传递多个数据，会存储与`name`对象中。

> 如果只传递单个数据，那么可以使属性值相同，直接获取。



## MVVM模型

### M: 模型(Model)

对应**data**中的数据

### V: 视图(View)

模板

### VM: 视图模型(ViewModel)

Vue实例对象





## 浏览器本地存储WebStorage

使用window上的`localStorage`属性来保存本地存储。

通过调用`setItem()`方法，它接受两个参数，`key`和`value`。

想要读取，使用`getItem('key')`方法。

删除：`removeItem('key')`。

清空：`clear()`。

相对应的，同样有**`sessionStorage`**。他们具有相同的参数，但是**`sessionStorage`**只会在一次会话中存储，当浏览器关闭，数据清空。

