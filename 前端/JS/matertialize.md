# Matertialize
Matertialize是一款基于谷歌设计语言Material Design的响应式框架，目前最新版本是1.0.0-alpha.4。
## 引入 ##
需要加载字体，css及js
```
//影响grid在手机下的显示
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
//Materialize使用的是google的图标
<link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
<link type="text/css" rel="stylesheet" href="css/materialize.min.css"  media="screen,projection"/>
<script type="text/javascript" src="js/materialize.min.js"></script>
```

## 使用
### css样式
#### 颜色
Matertialize中可以通过添加颜色名字的类名，直接修改颜色，下面就是一个teal（深青色）div的示例，lighten是调亮，darken是调暗。
如果是文字，加一个`-text`后缀即可。
[颜色板](http://next.materializecss.com/color.html)
```
 <div class="card-panel teal lighten-2">This is a card panel with a teal lighten-2 class</div>
 <span class="blue-text text-darken-2">This is a card panel with dark blue text</span>
```

#### 栅格(grid)
Matertialize提供的是标准12列响应式布局，使用时，需要用container容器包裹，不同像素下的container宽度也不同，基本用法与bootstap等框架类似。


| |移动设备<=600px | 平板电脑设备>600px | 桌面设备>992px | 大型桌面设备>1200px |
|:------|:------|:------|:------|:------|
| 类前缀 | .s | .m	 | .l | .xl |
| container宽度 | 90％ | 85％ | 70％ | 70％ |


需要注意的是，Matertialize中，offset是偏移，等于使用margin；而pull,push类似于position，是可以堆叠在一起的。
```
<div class="row">
      <div class="col s7 push-s5"></div>
      <div class="col s5 pull-s7"></div>
      <div class="col s4 offset-s6"></div>
      <div class="col s2></div>
</div>
//响应式示例
<div class="row">
     <div class="col s12 m4 l3"> </div>
     <div class="col s12 m8 l9"> </div>
</div>
```
#### 格式
- 使用`valign-wrapper`进行垂直对齐(加在容器上);
- 使用`.left-align`，`.right-align`和`.center-align`进行文本水平对齐
- 使用`left`或`right`来快速浮动事物。`!important`用于避免特殊性问题。
- 使用`truncate`保证文字不超出box
- 使用`hoverable`出现悬浮效果，需要使用card-panel容器
- 使用`flow-text`来提高文本对响应式的支持，优化在小屏幕的显示
- 隐藏/显示内容

|类	|屏幕范围|
|:------|:------|
|.hide	| 为所有设备隐藏|
|.hide-on-small-only	| 仅针对移动设备隐藏|
|.hide-on-med-only	| 仅供平板电脑使用|
|.hide-on-med-and-down	| 隐藏在平板电脑和下面|
|.hide-on-med-and-up	| 隐藏在平板电脑和以上|
|.hide-on-large-only	| 仅针对桌面隐藏|
|.show-on-small	| 只显示移动设备|
|.show-on-medium	| 只显示平板电脑|
|.show-on-large	| 只显示桌面|
|.show-on-medium-and-up	| 展示平板电脑和以上|
|.show-on-medium-and-down	| 展示平板电脑和以下|

### 组件
#### 按钮
Materialize提供了三种按钮类型，分别是Raised,Floating和Flat。
##### Raised
Raised即正常的凸起样式按钮,`waves-effect`，`waves-light`是点击（水漾）特效，如果是提交表单，则应用button标签而非a标签。可以使用`btn-large`，`btn-small`，`disabled`控制按钮的大小、显示。
```
<a class="waves-effect waves-light btn">button</a>
<button class="btn waves-effect waves-light" type="submit" name="action">Submit</button>
```
##### Floating
Floating按钮代表着重要功能，默认样式是圆形悬浮。
```
 <a class="btn-floating btn-large waves-effect waves-light red">add</a>
```
#### Flat
Flat按钮用于减少过多的分层。通常用于卡片或模式中的动作，因此不会有太多重叠的阴影。
```
<a class="waves-effect waves-teal btn-flat">Button</a>
```
### 路径导航
```
<nav>
  <div class="nav-wrapper">
    <div class="col s12">
      <a href="#!" class="breadcrumb">First</a>
      <a href="#!" class="breadcrumb">Second</a>
      <a href="#!" class="breadcrumb">Third</a>
    </div>
  </div>
</nav>
```
### 卡片
卡片用于展示不同类型的内容是很方便的，它适合用于展示具有相似的对象但是行为差异大的内容，如具有可变长度的标题的照片。
```
//最外层是card类
<div class="card blue-grey darken-1">
  //图片用card-image类
  <div class="card-image waves-effect waves-block waves-light">
  //图片增加activator类，就可以拥有点击显示动作
      <img class="activator" src="images/office.jpg">
  </div>
//内容用card-content类
  <div class="card-content white-text">
    <span class="card-title">卡片标题</span>
    <p>我是一个很简单的卡片。</p>
  </div>
//执行动作用card-action
  <div class="card-action">
    <a href="#">这是一个链接</a>
  </div>
//点击图片后，显示card-reveal
//如果给最外层（即card）增加一个sticky-action类，那么card-action即使在点击之后也显示
  <div class="card-reveal">
     <span class="card-title grey-text text-darken-4">卡片标题<i class="material-icons right">close</i></span>
     <p>单击后显示的产品的详细信息。</p>
   </div>
</div>
```
### 集合
集合组件的基本结构就是`collection`配合`collection-item`使用
```
//with-header，实践看来增加了左右padding值，更美观
<ul class="collection with-header">
    //用collection-header做集合头部，不用也可以
    <li class="collection-header"><h4>First Names</h4></li>
    <li class="collection-item">Alvin</li>
    <li class="collection-item">Alvin</li>
    <li class="collection-item">Alvin</li>
    <li class="collection-item">Alvin</li>
</ul>
```
更好的实例应当是同类型的信息展示,以下是一个带头像信息展示例子，可以用来做评论列表、好友列表等等
```
<ul class="collection">
  //avatar 影响到avatar内部展示，实践看来是头像左侧显示，右侧文字对齐
  <li class="collection-item avatar">
    <img src="images/yuna.jpg" alt="" class="circle">
    <span class="title">Title</span>
    <p>First Line <br>
       Second Line
    </p>
    //显示一个绿色五角星，可以增加交互
    <a href="#!" class="secondary-content"><i class="material-icons">grade</i></a>
  </li>
  <li class="collection-item avatar">
    <i class="material-icons circle">folder</i>
    <span class="title">Title</span>
    <p>First Line <br>
       Second Line
    </p>
    <a href="#!" class="secondary-content"><i class="material-icons">grade</i></a>
  </li>
</ul>
```
### 悬浮按钮
该组件提供一个固定悬浮在页面的按钮，提供不错的交互体验
```
//HTML
//用fixed-action-btn包裹，内部结构是一个显示按钮，配合ul及li标签
<div class="fixed-action-btn">
  <a class="btn-floating btn-large red">
    <i class="large material-icons">mode_edit</i>
  </a>
  <ul>
    <li><a class="btn-floating red"><i class="material-icons">insert_chart</i></a></li>
    <li><a class="btn-floating yellow darken-1"><i class="material-icons">format_quote</i></a></li>
    <li><a class="btn-floating green"><i class="material-icons">publish</i></a></li>
    <li><a class="btn-floating blue"><i class="material-icons">attach_file</i></a></li>
  </ul>
</div>
//JS
var elem = document.querySelector('.fixed-action-btn');
var instance = M.FloatingActionButton.init(elem, options);
// 或者使用JQ调用
$('.fixed-action-btn').floatingActionButton({
   direction: 'left',
   hoverEnabled: false
  });
```
options:
| Name	|Type |	Default |	Description|
|:------|:------|:------|:------|
|direction|	String|	'top'|可以是'top', 'right', 'buttom', 'left',控制子菜单的显示方向|
|hoverEnabled	|Boolean	|true| 子菜单悬浮显示 |
|toolbarEnabled|	Boolean|	false|	将子菜单转换toolbar工具栏显示|

在Jq1.12下，toolbar显示有问题，应当是版本过低不兼容导致。
定义的`instance`还拥有三个方法，分别是`.open()`默认打开子菜单，`.destroy()`销毁按钮，`.close()`关闭子菜单。
### 图标
Materialize使用的是谷歌提供的[Material图标](https://google.github.io/material-design-icons/#getting-icons)，具体样式[参考这里](http://next.materializecss.com/icons.html)。
### 导航栏

### 分页

### 表单

### 预加载

## javascript
### 下拉列表

### 侧边导航

### modals模态
