# Vue Router

要使用VueRouter，需要先安装该插件。

之后创建路由器：

```js
import VueRouter from 'vue-router'
import About from '../About'
import Home from '../Home'

//创建路由器
export default new VueRouter({
  routes:[
    {
      path:'/about',
      component:About
    },
    {
      path:'/home',
      component:Home
    }
  ]
})
```

## 路由切换

在组件中使用`<router-link to="/path">Route</router-link>`

来实现路由的切换，然后使用`<router-view></router-view>`标签，控制路由组件展示的位置。“

## 配置子路由与嵌套路由

``` js
export default new VueRouter({
  routes:[
    {
      path:'/about',
      component:About
    },
    {
      path:'/home',
      component:Home,
      children:[
        {
          path:'news',//注意不需要‘/’
          component:News,
        },
        {
          path:'message',
          component:Message,
        }
      ]
    }
  ]
})
```

## 路由传参

使用路由时，也可以向路由组件中传递数据。

### 字符串写法

向`<router-link></router-link>`中`to`属性中传入的路由添加类似于**GET**方法的参数。

如

```html
<router-link :to="`/derail?id=${m.id}&title=${m.title}`"></router-link>
<!-- 需要使用模版字符串 -->
```

### 对象写法

将`to`的属性值以对象传入。

```html
<router-link :to="{
                  path:'/detail',
                  query:{
                  	id:m.id,
                  	title:m.title,
                  }
                  }">
</router-link>
```

### 获取参数

在路由组件中，通过`this.$router.query.*`来获取传入的数据。

## 命名路由

可以在路由配置中配置路由的名字。在使用时可以使用名字代替`path`。

```html
<router-link :to="{name: 'detail'}"></router-link>
```

需要注意的是，使用名字时，`to`属性内必须使用对象形式。



## 路由Params参数

在`index.js`中，对路由进行参数配置。

```js
routes:[
    {
      path:'/about/:id/:title',//占位符声明参数
      component:About
    },
]
```

通过提供路径中的参数以传递数据。

## 路由Props

```js
routes:[
    {
      path:'/about,
      component:About,
      //props第一种写法，对象中所有元素都在组件中以props接收。
      props:{a:1,b:'hello'},
  		//第二种写法，接收一个boolean，为真时，将该路由接收到的params参数，以props形式传递给组件。
  		props:true,
  		//第三种写法，接收一个函数,函数返回的对象中每一对key-value，以props形式传入。
  		props($route){
        return {id:$route.query.id,title:$route.query.title}
      }
    },
]
```

## Router-Link的路由方式

使用`router-link`标签做跳转时，默认是push模式，会存入历史记录。

向标签添加`replace`属性，变为替换模式，会替换当前记录。

<<<<<<< Updated upstream
## 编程式路由

可以使用`js`方法代替`router-link`标签来进行跳转.

```js
pushShow(m){
    this.$router.push({
        name:'xiangqing',
        query:{
            id:m.id,
            title:m.title
        }
    })
}
// this.$router.back() 回退
// this.$router.forward() 前进
// this.$router.go() 接受一个整数,正数前进,负数后退
```

## 缓存路由组件

如果要防止路由切换之后,原路由组件被销毁(例如表单数据),可以使用

`<keep-alive include="组件名称"></keep-alive>`

对`<router-view>`进行包裹.如果不添加`include`属性,则会对所有组件生效.多个`include`内容使用`,`分隔.
=======


## Vue-Router中的生命周期

如果向某个路由组件添加了路由缓冲，切换路由时，不会销毁组件触发`destroy`事件。

Vue-Router中提供了两个特有的生命周期钩子。

- `activated`

激活，路由组件展示时调用。

- `deactivated`

失活，路由组件切换时调用。



## 路由守卫

用于保护路由安全，控制路由的访问权限。

### 全局路由守卫

在Vue-Router的`index.js`中，向router的实例对象调用`beforeEach`方法。

```js
//全局前置路由守卫，一般用于对路由权限进行控制
router.beforeEach((to,from,next)=>{//在路由切换前或者初始化时，自动调用
  next();
  //to显示目的路由，from显示出发路由，使用next()放行。
})
//全局后置路由守卫
router.afterEach((to,from)=>{//路由切换之后触发
  
})
```

除此之外，可以向router中的`meta`属性添加自定义参数来进行判断，

```js
const routes = [
  {
    path: "/",
    name: "App",
    redirect: "/index",
    component: App,
    meta: {isAuth:true}//添加是否认证，之后根据该自定义参数来判断
  },
]
```

### 独享路由守卫

针对单独路由生效的路由守卫。

```js
const routes = [
  {
    path: "/",
    name: "App",
    redirect: "/index",
    component: App,
    meta: {isAuth:true},//添加是否认证，之后根据该自定义参数来判断
    beforeEnter:(to,from,next)=>{//进入路由界面前触发
  		//...
  	},
  },
]
```

独享路由守卫，仅有前置守卫，没有后置守卫。

### 组件内路由守卫

存在于组件内的路由守卫。

```vue
<script>
  export default {
  	name: "Index",
  	beforeRouteEnter(to,from,next){
      //通过路由规则进入该组件时，触发
    },
    beforeRouteLeave(to,from,next){
      //通过路由规则离开该组件时，触发
    }
};
</script>
```

>>>>>>> Stashed changes
