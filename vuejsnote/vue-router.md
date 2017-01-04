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