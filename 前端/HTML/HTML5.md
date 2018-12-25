# HTML5

## WebSocket

WebSocket 是 HTML5 开始提供的一种在单个 TCP 连接上进行全双工通讯的协议。
具体见 前后端交互方法对比

## canvas

`<canvas>` 标签定义图形，比如图表和其他图像。
`<canvas>` 标签只是图形容器，必须使用脚本来绘制图形。
具体见 实现动画的方式及对比
早期的浏览器（如 IE8）不支持 canvas 标签，所以可以使用 svg 标签支持 IE8 的显示；如果项目要求支持 IE8 及以下的浏览器，会用 Hcharts 图表（使用 SVG），如果不要求支持 IE8，就用 Echarts（使用 canvas）。
使用 canvas 可以实现像素级绘制，这是在低版本浏览器中无法做到的，也是 canvas 和 svg 的主要区别之一。
canvas 中只有一个标签，而 svg 中有很多标签。
canvas 是用 JS 代码绘制的，而 svg 是用标签绘画的。
在不考虑浏览器兼容时，会使用 canvas，考虑浏览器兼容时，用 svg。

## web worker

当在 HTML 页面中执行脚本时，页面的状态是不可响应的，直到脚本已完成。
`web worker`是运行在后台的 JavaScript，独立于其他脚本，不会影响页面的性能。

```
//html
<div id="box">60</div>
//js
var box = document.getElementById("box");
var w = new Worker("count.js");
w.postMessage(60);
w.onmessage = function(event) {
  box.innerHTML = event.data;
};
//计数脚本 count.js
self.onmessage = function(event) {
//接收传入的数字
  var num = event.data;
  //每隔一秒种执行一次
  var T = setInterval(function() {
  //向index.jsp页面传回一段消息
  self.postMessage(--num);
    if (num <= 0) {
      clearInterval(T);
      self.close();
    }
  }, 1000);
};
```

## 语义化标签

| 标签           | 描述                             |
| :------------- | :------------------------------- |
| `<nav>`        | 定义导航链接的部分。             |
| `<header>`     | 定义了文档的头部区域。           |
| `<footer>`     | 定义了文档的页脚。               |
| `<aside>`      | 定义页面的侧边栏内容。           |
| `<section>`    | 定义文档中的节、区段。           |
| `<article>`    | 定义页面独立的内容区域。         |
| `<figure>`     | 独立流内容，图像、图表、代码等。 |
| `<figcaption>` | 定义`<figure>`元素的标题。       |

理解：

1. 利于机器，方便搜索引擎理解页面，提升 SEO
2. 利于团队，更轻松地读懂页面，便于开发与维护

## input 表单

HTML5 中`input`表单增加很多新属性与新表单。

- 比较重要的属性有：
  `autocomplete` 规定是否使用输入字段的自动完成功能，默认值是"on",开启该功能。
  `autofocus` 规定输入字段在页面加载时是否获得焦点，值为“autofocus”。
  `height`、`width` 只适用于`type="image"`，规定宽高
  `min`、`max` 规定输入字段的最小值与最大值。
  `placeholder` 规定帮助用户填写输入字段的提示。
- type 新增的重要类型有（兼容不一致）：
  `datalist`元素规定输入域的选项列表。
  `<input type="text" list="listCity"/> <datalist id="listCity"> <option value="北京" /> <option value="上海" /> <option value="钓鱼岛" /> <option value="台湾" /> </datalist>`

## 本地存储

localStorage 没有时间限制的数据存储
sessionStorage 浏览器关闭后被删除，使用方式无差别

```
<!-- 增 -->
localStorage.setItem('myCat', 'Tom');
<!-- 查 -->
let cat = localStorage.getItem('myCat');
<!-- 删 -->
localStorage.removeItem('myCat');
localStorage.clear();
```

具体见 sesscion,cookie 及 HTML5 本地存储对比

## 多媒体

### viedo

HTML5 新增了`<video>` 标签，定义视频，比如电影片段或其他视频流
|属性 |值| 描述|
|:--|:--|:--|
|autoplay |autoplay| 如果出现该属性，则视频在就绪后马上播放。|
|controls |controls |如果出现该属性，则向用户显示控件，比如播放按钮。|
|height |pixels |设置视频播放器的高度。|
|loop |loop| 如果出现该属性，则当媒介文件完成播放后再次开始播放。|
|preload |preload|如果出现该属性，则视频在页面加载时进行加载，并预备播放。如果使用 "autoplay"，则忽略该属性。|
|src |url| 要播放的视频的 URL。|
|width| pixels| 设置视频播放器的宽度。|

### audio

HTML5 新增了`<audio>`标签，定义声音，比如音乐或其他音频流。
|属性 |值 |描述|
|:--|:--|:--|
|autoplay |autoplay |如果出现该属性，则音频在就绪后马上播放。|
|controls |controls |如果出现该属性，则向用户显示控件，比如播放按钮。|
|loop |loop |如果出现该属性，则每当音频结束时重新开始播放。|
|muted| muted |规定视频输出应该被静音。|
|preload| preload| 如果出现该属性，则音频在页面加载时进行加载，并预备播放。如果使用 "autoplay"，则忽略该属性。|
|src| url |要播放的音频的 URL。|

## History.api

History 接口允许操作浏览器的曾经在标签页或者框架里访问的会话历史记录，不会造成刷新。

### 属性

- History.length 只读
  返回一个整数，该整数表示会话历史中元素的数目，包括当前加载的页。例如，在一个新的选项卡加载的一个页面中，这个属性返回 1。
- History.state 只读
  返回一个表示历史堆栈顶部的状态的值。这是一种可以不必等待 popstate 事件而查看状态而的方式。

### 方法

- History.back()
  前往上一页, 用户可点击浏览器左上角的返回按钮模拟此方法. 等价于 history.go(-1)。

- History.forward()
  在浏览器历史记录里前往下一页，用户可点击浏览器左上角的前进按钮模拟此方法. 等价于 history.go(1)。

- History.go()
  通过当前页面的相对位置从浏览器历史记录( 会话记录 )加载页面。比如：参数为-1 的时候为上一页，参数为 1 的时候为下一页。

- History.pushState(stateData, title, url)
  按指定的名称和 URL（如果提供该参数）将数据 push 进会话历史栈，数据被 DOM 进行不透明处理。页面对应的stateData通过history.state读取。

- History.replaceState(stateData, title, url)
  按指定的数据，名称和 URL(如果提供该参数)，更新历史栈上最新的入口。这个数据被 DOM 进行了不透明处理。页面对应的stateData通过history.state读取。

### 监听

- window.onpopstate：当调用 history.go()、history.back()、history.forward()时触发；pushState()\replaceState()方法不触发。
- window.onhashchange：当前 URL 的锚部分(以 '#' 号为开始) 发生改变时触发。触发的情况如下：
  - 通过设置 Location 对象 的 location.hash 或 location.href 属性修改锚部分；
  - 使用不同 history 操作方法到带 hash 的页面;
  - 点击链接跳转到锚点。

### 使用
<!-- TODO: 以下还是需要重新测试与检测 -->
在safari中使用 history.back()或者history.go(-1)时，页面可以返回，但是页面本身的read的JS不会重新执行，提交、websocket等可能都不会执行，加一个监听是否取缓存，重载页面即可：
```
window.onpageshow =function(evt){
  <!-- evt.persisted是监测缓存? -->
  if(evt.persisted){ 
      window.location.reload();
  }
};
```

需要检测websocket（发送数据、是否重建立新连接等）、 jquery-mobiled的情况。