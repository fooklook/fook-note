##jqueryForm使用

```javascript
var options = {  
    target: '#output',          //把服务器返回的内容放入id为output的元素中      
    beforeSubmit: showRequest,  //提交前的回调函数  
    success: showResponse,      //提交后的回调函数  
    //url: url,                 //默认是form的action， 如果申明，则会覆盖  
    //type: type,               //默认是form的method（get or post），如果申明，则会覆盖  
    //dataType: null,           //html(默认), xml, script, json...接受服务端返回的类型  
    //clearForm: true,          //成功提交后，清除所有表单元素的值  
    //resetForm: true,          //成功提交后，重置所有表单元素的值  
    timeout: 3000               //限制请求的时间，当请求大于3秒后，跳出请求  
}  
    
function showRequest(formData, jqForm, options){  
    //formData: 数组对象，提交表单时，Form插件会以Ajax方式自动提交这些数据，格式如：[{name:user,value:val },{name:pwd,value:pwd}]  
    //jqForm:   jQuery对象，封装了表单的元素。可以直接通过jqForm[0].name.value获取元素。 
    //options:  options对象  
    // var queryString = $.param(formData);   	//name=1&address=2  
    // var formElement = jqForm[0];              //将jqForm转换为DOM对象  
    // var address = formElement.address.value;  //访问jqForm的DOM元素  
    return true;  							//只要不返回false，表单都会提交,在这里可以对表单元素进行验证
};  
    
function showResponse(responseText, statusText){
    return false;
    //dataType=xml  
    var name = $('name', responseXML).text();  
    var address = $('address', responseXML).text();  
    $("#xmlout").html(name + "  " + address);  
    //dataType=json  
    $("#jsonout").html(data.name + "  " + data.address);  
};  
    
$("#myForm").ajaxForm(options);  
    
// $("#myForm").submit(funtion(){  
//    $(this).ajaxSubmit(options);  
//    return false;   //阻止表单默认提交  
// });  
```

###对表单数据进行验证

```javascript
function validate(formData, jqForm, options) { //在这里对表单进行验证，如果不符合规则，将返回false来阻止表单提交，直到符合规则为止  
   //方式一：利用formData参数  
   for (var i=0; i < formData.length; i++) {  
       if (!formData[i].value) {  
            alert('用户名,地址和自我介绍都不能为空!');  
            return false;  
        }  
    }   
  
   //方式二：利用jqForm对象  
   var form = jqForm[0]; //把表单转化为dom对象  
      if (!form.name.value || !form.address.value) {  
            alert('用户名和地址不能为空，自我介绍可以为空！');  
            return false;  
      }  
  
   //方式三：利用fieldValue()方法，fieldValue 是表单插件的一个方法，它能找出表单中的元素的值，返回一个集合。  
   var usernameValue = $('input[name=name]').fieldValue();  
   var addressValue = $('input[name=address]').fieldValue();  
   if (!usernameValue[0] || !addressValue[0]) {  
      alert('用户名和地址不能为空，自我介绍可以为空！');  
      return false;  
   }  
  
    var queryString = $.param(formData); //组装数据  
    //alert(queryString); //类似 ： name=1&add=2    
    return true;  
}  
```