##html5分段读取文件

###FileReader

HTML5定义了FileReader作为文件API的重要成员用于读取文件，根据W3C的定义，FileReader接口提供了读取文件的方法和包含读取结果的事件模型。

####FileReader对象的方法

1. readAsBinaryString(file) : 将文件读取为二进制编码,
2. readAsText(file,[encoding]) : 将文件读取为文本,
3. readAsDataURL(file) : 将文件读取为DataURL,
4. abort(none) : 中断读取操作.

####FileReader接口事件

FileReader接口包含了一套完整的事件模型，用于捕获读取文件时的状态。

1. onabort : 中断,
2. onerror : 出错,
3. onloadstart : 开始,
4. onprogress : 正在读取,
5. onload : 读取成功,
6. onloadend : 读取完成，无论成功失败(无论成功失败)。

###读取文件

```javascript
function getfilesize(){
	var selectedFile = document.getElementById("files").files[0];//获取读取的File对象
    var name = selectedFile.name;//读取选中文件的文件名
    var size = selectedFile.size;//读取选中文件的大小
    console.log("文件名:"+name+"大小："+size);

	var reader = new FileReader();
	reader.readAsText(selectedFile);
	reader.onload = function(){
        $(".content").append(this.result);//当读取完成之后会回调这个函数，然后此时文件的内容存储到了result中。直接操作即可。
    }; 
}
```

###分段读取

```javascript
function getfilesize(){
	var selectedFile = document.getElementById("files").files[0];//获取读取的File对象
    var name = selectedFile.name;//读取选中文件的文件名
    var size = selectedFile.size;//读取选中文件的大小
    console.log("文件名:"+name+"大小："+size);
    var pagesize = 100;		//分段读取长度

    for (var i = 0; i <= size; i = i+pagesize) {
    	var end = Math.min(size, i+pagesize);
    	if (selectedFile.webkitSlice) {					//在chrome中没有该方法
	      	var blob = selectedFile.webkitSlice(i, end);
	    } else if (selectedFile.mozSlice) {					//在firefox中也无效，但是先这么写	
	      	var blob = selectedFile.mozSlice(i, end);
	    } else {
			var blob = selectedFile.slice(i, end);
	    }
	    console.log(i);
	    var reader = new FileReader();//这里是核心！！！读取操作就是由它完成的。
	    reader.readAsText(blob);//读取文件的内容
	    reader.onload = function(){
	    	console.log(this.result);
	        $(".content").append(this.result);//当读取完成之后会回调这个函数，然后此时文件的内容存储到了result中。直接操作即可。
	    };
    }
}
```