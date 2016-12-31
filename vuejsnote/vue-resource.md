##vue-resource

###vue-resource插件具有以下特点：

####体积小

vue-resource非常小巧，在压缩以后只有大约12KB，服务端启用gzip压缩后只有4.5KB大小，这远比jQuery的体积要小得多。

####支持主流的浏览器

和Vue.js一样，vue-resource除了不支持IE 9以下的浏览器，其他主流的浏览器都支持。

####支持Promise API和URI Templates

Promise是ES6的特性，Promise的中文含义为“先知”，Promise对象用于异步计算。

URI Templates表示URI模板，有些类似于ASP.NET MVC的路由模板。

####支持拦截器

拦截器是全局的，拦截器可以在请求发送前和发送请求后做一些处理。

拦截器在一些场景下会非常有用，比如请求发送前在headers中设置access_token，或者在请求失败时，提供共通的处理方式。

###vue-resource使用

####引入vue-resource

```javascript
import Vue from 'vue'
import VueResource from 'vue-resource'

Vue.use(VueResource)
```

####基本语法

```javascript
this.$http.get('/someUrl', [options]).then(successCallback, errorCallback);
this.$http.post('/someUrl', [body], [options]).then(successCallback, errorCallback);
```

在发送请求后，使用then方法来处理响应结果，then方法有两个参数，第一个参数是响应成功时的回调函数，第二个参数是响应失败时的回调函数。

then方法的回调函数也有两种写法，第一种是传统的函数写法，第二种是更为简洁的ES 6的Lambda写法：

```javascript
// 传统写法
this.$http.get('/someUrl', [options]).then(function(response){
	// 响应成功回调
}, function(response){
	// 响应错误回调
});

// Lambda写法
this.$http.get('/someUrl', [options]).then((response) => {
	// 响应成功回调
}, (response) => {
	// 响应错误回调
});
```

###支持的HTTP方法

vue-resource的请求API是按照REST风格设计的，它提供了7种请求API：

- get(url, [options])
- head(url, [options])
- delete(url, [options])
- jsonp(url, [options])
- post(url, [body], [options])
- put(url, [body], [options])
- patch(url, [body], [options])

###options对象

- url	string	请求的URL
- method	string	请求的HTTP方法，例如：'GET', 'POST'或其他HTTP方法
- body	Object,FormDatastring	request body
- params	Object	请求的URL参数对象
- headers	Object	request header
- timeout	number	单位为毫秒的请求超时时间 (0 表示无超时时间)
- before	function(request)	请求发送前的处理函数，类似于jQuery的beforeSend函数
- progress	function(event)	ProgressEvent回调处理函数
- credientials	boolean	表示跨域请求时是否需要使用凭证
- emulateHTTP	boolean	发送PUT, PATCH, DELETE请求时以HTTP POST的方式发送，并设置请求头的X-HTTP-Method-Override
- emulateJSON	boolean	将request body以application/x-www-form-urlencoded content type发送

###response对象

####方法

- text()	string	以string形式返回response body
- json()	Object	以JSON对象形式返回response body
- blob()	Blob	以二进制形式返回response body

####属性

- ok	boolean	响应的HTTP状态码在200~299之间时，该属性为true
- status	number	响应的HTTP状态码
- statusText	string	响应的状态文本
- headers	Object	响应头

###CURD示例

####GET请求

```javascript
this.$http.get('/someUrl').then((response) => {
    this.$set('gridData', response.data)
}, (response) => {
    console.log(response)
}).catch(function(response) {
    console.log(response)
})
```

####JSONP请求

```javascript
this.$http.jsonp('/someUrl').then(function(response){
    this.$set('gridData', response.data)
}, (response) => {
    console.log(response)
})
```

####POST请求

```javascript
this.$http.post('/someUrl', this.params).then((response) => {
    this.$set('item', {})
    this.getCustomers()
}, (response) => {
    console.log(response)
})
```

####PUT请求

```javascript
vm.$http.put('/someUrl', this.params).then((response) => {
    this.getCustomers()
})
```

####Delete请求

```javascript
vm.$http.delete('/someUrl').then((response) => {
    vm.getCustomers()
})
```

###使用resource服务

vue-resource提供了另外一种方式访问HTTP——resource服务，resource服务包含以下几种默认的action：

```javascript
get: {method: 'GET'},
save: {method: 'POST'},
query: {method: 'GET'},
update: {method: 'PUT'},
remove: {method: 'DELETE'},
delete: {method: 'DELETE'}
```

####GET请求

```javascript
const resource = this.$resource('someItem{/id}')
resource.get({id: 1}).then(function (response) {
    this.$set('gridData', response.data)
}).catch(function (response) {
    console.log(response)
})
```

####POST请求

```javascript
const resource = this.$resource('someItem{/id}')
this.$resource.save(this.item).then( function (response) {
    this.$set('item', {})
    this.getCustomers()
})
```

####PUT请求

```javascript
const resource = this.$resource('someItem{/id}')
resource.update({id: this.item.customerId}, this.item).then( function (response) {
    this.getCustomers()
})
```

####DELETE请求

```javascript
const resource = this.$resource('someItem{/id}')
resource.delete({id: customer.customerId}).then( function (response) {
    this.getCustomers()
})
```

