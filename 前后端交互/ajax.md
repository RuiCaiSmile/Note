# ajax笔记
## 介绍
AJAX = Asynchronous JavaScript and XML（异步的 JavaScript 和 XML）。

AJAX 是一种使用现有标准的新方法，作用是在不重新加载整个页面的情况下，更新部分页面。
##原理
基于XMLHttpRequest对象，该对象用于和服务器交换数据。


## 步骤
* 状态

` readyState`  存有 XMLHttpRequest 的状态。从 0 到 4 发生变化。

    - 0: 请求未初始化
    - 1: 服务器连接已建立
    - 2: 请求已接收
    - 3: 请求处理中
    - 4: 请求已完成，且响应已就绪

`status` 返回服务器常用的状态码。

    - 200：服务器响应正常。
    - 304：该资源在上次请求之后没有任何修改（这通常用于浏览器的缓存机制，使用GET请求时尤其需要注意）。
    - 400：无法找到请求的资源。
    - 403：没有权限访问资源。
    - 404：需要访问的资源不存在。
    - 500：服务器内部错误。

每当 readyState 改变时，就会触发 onreadystatechange 事件。


* 请求
```
var   xmlhttp=new XMLHttpRequest();
xmlhttp.open("POST","adduser.action",true);
xmlhttp.send();
```
`open(method,url,async)` 规定请求的类型、URL 以及是否异步处理请求。
其中method分为GET和POST两种方法，这两种方法的发送机制不一样，适用场景也不一样。
1. GET请求有数据量的限制，因为GET是通过URL提交数据，而浏览器对URL的长度有各自的规定。
2. GET方式请求的数据会被浏览器缓存起来，因此其他人就可以从浏览器的历史记录中读取到这些数据，例如账号和密码等。在某种情况下，GET方式会带来严重的安全问题。而POST方式相对来说就可以避免这些问题。
3. 发送包含未知字符的用户输入时，POST 比 GET 更稳定也更可靠。

async的值有ture（异步）或false（同步）。如果使用ture，ajax后执行的语句无法得到ajax的返回值，而false可以，不过这也会导致页面必须要等ajax响应完成才能继续执行。

`send()`将请求发送到服务器。POST方法下可以有一个String作为参数。
* 响应

`responseText`	 获得字符串形式的响应数据。
`responseXML` 获得 XML 形式的响应数据。
## 实例
```
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<script>
function showCustomer(str)
{
  var xmlhttp;
  if (str=="")
  {
    document.getElementById("txtHint").innerHTML="";
    return;
  }
  if (window.XMLHttpRequest)
  {
    // IE7+, Firefox, Chrome, Opera, Safari 浏览器执行代码
    xmlhttp=new XMLHttpRequest();
  }
  else
  {
    // IE6, IE5 浏览器执行代码
    xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
  }
  xmlhttp.onreadystatechange=function()
  {
    if (xmlhttp.readyState==4 && xmlhttp.status==200) //正确响应执行
    {
      document.getElementById("txtHint").innerHTML=xmlhttp.responseText;
    }
  }
  xmlhttp.open("GET","/try/ajax/getcustomer.php?q="+str,true);
  xmlhttp.send();
}
</script>
</head>
<body>

<form action="">
<select name="customers" onchange="showCustomer(this.value)" style="font-family:Verdana, Arial, Helvetica, sans-serif;">
<option value="APPLE">Apple Computer, Inc.</option>
<option value="BAIDU ">BAIDU, Inc</option>
<option value="Canon">Canon USA, Inc.</option>
<option value="Google">Google, Inc.</option>
<option value="Nokia">Nokia Corporation</option>
<option value="SONY">Sony Corporation of America</option>
</select>
</form>
<br>
<div id="txtHint">客户信息将显示在这...</div>

</body>
</html>
```
## JQ中ajax的使用
常用`$.post`和`$.get`,如果细致操作，可以使用`$.ajax`。
* `jQuery.post(url, [data], [callback], [type])`
       -  url:发送请求地址。
       - data:待发送 Key/value 参数。
       -  callback:发送成功时回调函数。
       - type:返回内容格式，xml, html, script, json, text, default

```
$.post("addUser.action", { "name": "Rui" },
          function(data){
          alert(data.name);
          }, "json");
```
* jQuery.get(url, [data], [callback], [type])

与post类似
