##BOM实用总结

###window对象

window同时扮演着ECMAScript中Global对象的角色，所以在全局对象中声明的变量和函数，都可以通过window访问。

###窗口大小

- innerWidth 页面视图区宽度
- innerHeight 页面视图区高度
- outerWidth 浏览器窗口尺寸宽度
- outerHeight 浏览器窗口尺寸高度

###打开窗口

window.open(string url, type);

- type如果为字符串则，新页面打开，如果存在type名称的窗口，则在type名称的窗口打开。
- _blank 在新页面打开
- _self 在当前页打开
- _parent 在父页面打开
- _top 在定级页面打开 

###间歇调用和超时调用

- setTimeout超时调用：当程序执行到setTimeout时，第二个参数就会告诉javascript再过多长时间把当前任务添加到队列中。如果队列为空，则代码会立即执行。队列不为空，则前面的代码执行完后在执行。
- clearTimeout取消超时调用。
- setInterval间歇调用：按照指定的时间间隔重复执行代码，直至间歇调用取消，或页面卸载。间歇性调用，可能会在前一个未执行完前执行。
- clearInterval取消间歇调用。

###系统对话框

显示对话框的时候，代码会停止执行，关闭对话框后，才会恢复执行。

- alert警告框
- confirm确认框
- prompt提示输入框

###location对象

location提供了当前窗口中健在文档的相关信息。

- location.hash返回URL的hash（即#后面部分）
- location.host返回服务器名称和端口号
- location.hostname返回不带端口号的服务器名称
- location.href当前页面完整的URL
- location.pathname返回URL中的目录和文件名
- location.part端口号
- location.protocol协议
- location.search返回URL的查询字符串，以?开头。
- location.reload重新载入页面
- location.replace替换当前页面(则当前页面的连接不会进入到历史记录中)
- window.location在当前页打开链接

###history对象

- history.go(-1)返回上一页
- history.go(1)前进一页
- history.back()返回上一页
- history.forward()前进一页