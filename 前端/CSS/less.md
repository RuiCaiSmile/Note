# less
## 安装
```
npm编译：
npm install -g less
lessc styles.less styles.css 


页面直接引用
<link rel="stylesheet/less" type="text/css" href="styles.less" />
<script src="//cdnjs.cloudflare.com/ajax/libs/less.js/3.7.1/less.min.js" ></script>
```
## 特性
### 变量
使用@定义变量，并使用
```
@width: 10px;
@height: @width + 10px;

#header {
  width: @width;
  height: @height;
}
```