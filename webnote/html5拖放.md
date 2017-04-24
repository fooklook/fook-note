##html5拖放

html5要进行拖放，首先要将元素设置为可拖放，再设置被拖动时会发生什么，再设置可以在何处可以放置，最后执行执行放置的逻辑。

###设置元素为可拖放

首先，为了使元素可拖动，把 draggable 属性设置为 true ：

```html
<img draggable="true">
```

###拖动什么 - ondragstart 和 setData()

规定当元素被拖动时，会发生什么。

```javascript
function action_ondragstart(ev){
    ev.dataTransfer.setData("Text",ev.target.id);   //设置被拖数据的数据类型和值：
    ev.stopPropagation();                           //阻止火狐浏览器默认事件
}
```

###放到何处 - ondragover

ondragover 事件规定在何处放置被拖动的数据。

```javascript
function action_ondragover(ev){
    ev.preventDefault();            //阻止默认事件
    ev.stopPropagation();           //阻止火狐浏览器默认事件
}
```

###进行放置 - ondrop

当放置被拖数据时，会发生 drop 事件。

```javascript
function drop(ev){
    ev.preventDefault();
    var data = ev.dataTransfer.getData("Text");
    ev.target.appendChild(document.getElementById(data));
    ev.stopPropagation();
}
```