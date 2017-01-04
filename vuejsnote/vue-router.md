##vue-router

###基本用法

####引入vue-router

```javascript
import Vue from 'vue'
import App from './App'
import VueRouter from 'vue-router'
```

####创建目录结构

我单独定义了一个websitepath.js文件，放置路由内容。

```javascript
import Page from './components/page'
import ListPage from './components/list'
import DetailPage from './components/detail'

var websitepaths = [
  { path: '/', component: Page },
  { path: '/list/:id', component: ListPage },
  { path: '/detail/:id/:name', component: DetailPage }
]

export default {
  websitepaths
}
```
####引入路由文件并加入路由插件

```javascript
import WebsitePath from './websitepath'
Vue.use(VueRouter)

var WebsiteRouter = new VueRouter({
  routes: WebsitePath.websitepaths
})

new Vue({
  router: WebsiteRouter,
  el: '#app',
  template: '<App/>',
  components: { App }
}).$mount('#app')
```

####使用路由

```html
<!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
<router-link to="/foo">Go to Foo</router-link>
<router-link to="/bar">Go to Bar</router-link>

<!-- 路由匹配到的组件将渲染在这里 -->
<router-view></router-view>
```

###获取路由参数

```javascript
$route.params.id
```

###响应路由参数的变化

当路由参数发生变化时,原来的组件实例会被复用，这也意味着组件的生命周期钩子不会再被调用。

```javascript
const User = {
  template: '...',
  watch: {
    '$route' (to, from) {
      // 对路由变化作出响应...
    }
  }
}
```

###编程式跳转

####跳转

普通跳转,标签会生成一个a标签。

```html
<router-link :to="...">
```

通过激发事件，执行程序，实现跳转。

```javascript
// 字符串
router.push('home')
// 对象
router.push({ path: 'home' })
// 命名的路由
router.push({ name: 'user', params: { userId: 123 }})
// 带查询参数，变成 /register?plan=private
router.push({ path: 'register', query: { plan: 'private' }})
```

####replace

通过添加replace，使得跳转的页面不会向history 添加新记录。

```javascript
//普通跳转
<router-link :to="..." replace>
//编程跳转
router.replace(...)
```

####向history跳转

```javascript
// 在浏览器记录中前进一步，等同于 history.forward()
router.go(1)
// 后退一步记录，等同于 history.back()
router.go(-1)
// 前进 3 步记录
router.go(3)
// 如果 history 记录不够用，那就默默地失败呗
router.go(-100)
router.go(100) 
```

###命名路由

有时候，通过一个名称来标识一个路由显得更方便一些，特别是在链接一个路由，或者是执行一些跳转的时候。你可以在创建 Router 实例的时候，在 routes 配置中给某个路由设置名称。

```javascript
const router = new VueRouter({
  routes: [
    {
      path: '/user/:userId',
      name: 'user',
      component: User
    }
  ]
})
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
router.push({ name: 'user', params: { userId: 123 }})
```

###多视图命名

有时候想同时（同级）展示多个视图，而不是嵌套展示，例如创建一个布局，有 sidebar（侧导航） 和 main（主内容） 两个视图，这个时候命名视图就派上用场了。

```javascript
<router-view class="view one"></router-view>
<router-view class="view two" name="a"></router-view>
<router-view class="view three" name="b"></router-view>
const router = new VueRouter({
  routes: [
    {
      path: '/',
      components: {
        default: Foo,
        a: Bar,
        b: Baz
      }
    }
  ]
})
```

###重定向

```javascript
//重定向也是通过 routes 配置来完成，下面例子是从 /a 重定向到 /b：
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: '/b' }
  ]
})
//重定向的目标也可以是一个命名的路由：
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: { name: 'foo' }}
  ]
})
//甚至是一个方法，动态返回重定向目标：
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: to => {
      // 方法接收 目标路由 作为参数
      // return 重定向的 字符串路径/路径对象
    }}
  ]
})
```

###别名

```javascript
const router = new VueRouter({
  routes: [
    { path: '/a', component: A, alias: '/b' }
  ]
})
```

[手册](http://router.vuejs.org/zh-cn/)