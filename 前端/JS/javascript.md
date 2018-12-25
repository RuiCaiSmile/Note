# JavaScript

## this 机制与 bind 等方法

箭头函数中，没有 this 作用域，所以如果是绑定了事件处理程序如点击事件中的函数，其 this 应该和父级元素的 this 指向一致

## 数据类型

js 中有七种数据类型，包括五种基本数据类型（number,string,boolean,undefined,Null）,和一种复杂数据类型（Object），以及 ES6 引入的 symbol 类型

```
typeof    123　　 //number
typeof   'abc'　　//string
typeof    true       //boolean
typeof    undefined   //undefined
typeof    null        //Object
typeof    {}           //Object
typeof    []           //Object
typeof Symbol() === 'symbol'  // true
typeof function(){} === 'function';

<!-- 值得注意的typeof -->
<!-- 注意，以下在控制台输出的是 undefined，而非function -->
typeof    console.log()       //undefined
<!-- 检测NaN使用 isNaN，因为 NaN ！=NaN -->
typeof NaN   //number

```
### key
object的 key 值是只允许String类型的，如果赋予key非string值，会调用对应的`toString`方法
### 潜拷贝与深拷贝
复杂数据类型存在潜拷贝与深拷贝的情况，因为复杂数据类型的名存在栈内存中，值存在于堆内存中，中间靠引用对应；潜拷贝是指复制之后的两个对象的引用一致，修改一个对象则另一个对象的值也会改变，深拷贝则是新建内存地址存值，两者互不影响。

```
<!-- 潜拷贝 -->
let a=[0,1,2,3,4],
let b=a;
 <!-- Array的slice和concat方法 会深拷贝array的基本数据类型的值，潜拷贝对象类型 -->

 <!-- 深复制 -->
let a=[0,1,2,3,4],
let b=[...a]

<!-- TODO: 该函数为copy，需要理解测试 -->
function deepClone(obj){
    let objClone = Array.isArray(obj)?[]:{};
    if(obj && typeof obj==="object"){
        for(key in obj){
            if(obj.hasOwnProperty(key)){
                //判断ojb子元素是否为对象，如果是，递归复制
                if(obj[key]&&typeof obj[key] ==="object"){
                    objClone[key] = deepClone(obj[key]);
                }else{
                    //如果不是，简单复制
                    objClone[key] = obj[key];
                }
            }
        }
    }
    return objClone;
}
```

## 原型链

`prototype`是函数才有的属性。
由于`_proto_`是任何对象都有的属性，而 js 里万物皆对象，所以会形成一条`_proto_`连接起来的链条。
当 js 引擎查找对象的属性时，先查找对象本身是否存在该属性，如果不存在，会在原型链上查找，但不会查找自身的`prototype`。

## 继承

### 组合继承

组合继承将原型链和借用构造函数的技术组合到一起，最常用。
让一个构造函数 A 的原型为另一个构造函数 B 的实例，便形成了 A 继承 B 的原型链;
在函数 Y 内使用 call 方法调用函数 X，实现构造函数继承。

```
function SuperType(name){
   this.name;
   this.colors = ["red","blue","yellow"];
}
SuperType.prototype.sayName = function(){
   alert(this.name);
};
function SubType(name,age){
   //继承属性,构造函数继承
   SuperType.call(this.name);
   this.age=age;
}
//继承方法,原型链继承
SubType.prototype = new SuperType();
SubType.prototype.constructor = SubType;  //需要注意，重新指向SubType
SubType.prototype.sayAge = function(){
  alert(this.age);
};
// 可以对不同的SubType实例共享方法，不同的实例也可以拥有自己的属性
```

### 寄生组合式继承

避免调用两次超类型的构造函数，并避免在原型（subType.prototype）上创建不必要的熟悉。

```
//封装过程,代替  subType.prototype = new superType();
function inheritPrototype(subType,superType){
   var prototype = Object(superType.prototype);
   prototype.constructor = subType;
   subType.prototype = prototype;
}
```

## 事件捕获与事件冒泡

事件捕获指的是从 document 到触发事件的那个节点，即自上而下的去触发事件。相反的，事件冒泡是自下而上的去触发事件。
绑定事件方法的第三个参数，就是控制事件触发顺序是否为事件捕获。true,事件捕获；false,事件冒泡。默认 false,即事件冒泡。
事件委托：利用事件冒泡处理动态元素事件绑定的方法。

## 闭包

### 定义

根本：JavaScript 中的函数运行在它们被定义的作用域里，而不是它们被执行的作用域里。
JavaScript 中的闭包，无非就是变量解析的过程。
每次定义一个函数，都会产生一个作用域链（scope chain）。当 JavaScript 寻找变量 varible 时（这个过程称为变量解析），总会优先在当前作用域链的第一个对象中查找属性 varible ，如果找到，则直接使用这个属性；否则，继续查找下一个对象的是否存在这个属性；这个过程会持续直至找到这个属性或者最终未找到引发错误为止。
因为是从被定义的作用域开始寻找，会有如下的代码情况:

```
var a = [];
for (var i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 10
<!-- 这是因为`console`的执行环境先是在`for`循环中，此时`i`被定义为全局变量，只存在一个`i`的值 -->
<!-- 如果用`let`定义，则会输出6，这是因为在`for`循环中，每次循环都会重新定义一个`i`，即`i`的作用域只在于当次循环内 -->
<!-- 非严格模式下忽略 var 直接使用变量，相当于定义全局变量 -->

```

### 用处

- 公有方法可以访问私有变量，模块化代码，减少全局污染

```
var singleton = (function(){
  // 私有变量
  var age = 22;
  var speak = function(){
    console.log("speaking!!!");
  };
  // 特权（或公有）属性和方法
  return {
    name: "percy",
    getAge: function(){
      return age;
    }
  };
})();

var objEvent = objEvent || {};
(function(){
    var addEvent = function(){
      // some code
    };
    objEvent.addEvent = addEvent;
})();
```

### 不足

- 闭包的缺点就是常驻内存会增大内存使用量，并且使用不当很容易造成内存泄露。
- 如果不是因为某些特殊任务而需要闭包，在没有必要的情况下，在其它函数中创建函数是不明智的，因为闭包对脚本性能具有负面影响，包括处理速度和内存消耗。

## 一些细节与机制

1. js 中 0 跟空字符串"" 如果用== 返回 true，使用===即可

2. 递归堆栈溢出的解决方式

```
function isEven (num) {
    if (num === 0) {
        return true;
    }
    if (num === 1) {
        return false;
    }
    <!-- 如果isEven的num值足够大，就会造成堆栈溢出 -->
    return isEven(Math.abs(num) - 2);
    
    <!-- 闭包解决方式，调用则是 isEven(num)()(),需要写函数重载实现... -->
    return ()=>{
      isEven(Math.abs(num) - 2);
    }
    <!-- setTimeout，有点奇怪，这样的返回值是什么？此外这个settimeout里本身就是一个函数 -->
    return setTimeout(()=>{
      return  isEven(Math.abs(num) - 2);
    })
}
```

## json 与 js 对象

###JS 对象与 Json 的区别

1. Json 是一种数据格式，而 js 对象表示类的实例，是内存中一段空间的名称
2. Json 是字符串，所以 key，value 部分都必须加双引号，而 js 对象不需要在 key 上使用双引号
3. js 的属性值可以是一个方法，而 json 不能
4. 前后端交互中注意不能使用单引号代替双引号（会无法解析）

###js 与 json 互相转换

```
//定义json外面需要用引号包住
var json ='{"name":"rui","age":"18"}';
//将json转换成js对象
 var object=JSON.parse(json);
//eval解析json，需要用括号包住大括号（即json数据）,从而识别为对象而不是方法体
var obj = eval('({"name":"rui","age":"18"})')
//将js对象转换成json
var json2 =JSON.stringify(object);
```

## 伪数组

https://blog.csdn.net/stone10086/article/details/74171094


## 判断浏览器版本

```
var userAgent = navigator.userAgent;

if(userAgent.indexOf("OPR") > -1){
    //Opera浏览器，因为Opera浏览器的userAgent也有Chrome和Safari，所以要写在前面
    alert("Opera");
}else if(userAgent.indexOf("Version") > -1 && userAgent.indexOf("Safari") > -1){
    //没有更好的办法判断Safari浏览器，只能通过version（版本号）的英文来判断，因为别的浏览器版本号不是这样写的
    alert("Safari");
}else if(userAgent.indexOf("Chrome") > -1){
    //谷歌浏览器也有可能是使用Chrome内核的其他浏览器
    alert("Chrome");
}else if(userAgent.indexOf("Firefox") > -1){
    //火狐浏览器
    alert("Firefox");
}else if(userAgent.indexOf("compatible") > -1 && userAgent.indexOf("MSIE 10.0") > -1){
    //IE 10.0
    alert("IE 10.0");
}else if(userAgent.indexOf("compatible") > -1 && userAgent.indexOf("MSIE 9.0") > -1){
    //IE 9.0
    alert("IE 9.0");
}else if(userAgent.indexOf("compatible") > -1 && userAgent.indexOf("MSIE 8.0") > -1){
    //IE 8.0
    alert("IE 8.0");
}else if(userAgent.indexOf("compatible") > -1 && userAgent.indexOf("MSIE 7.0") > -1){
    //IE 7.0
    alert("IE 7.0");
}else if(userAgent.indexOf("Trident")){
    //IE 11.0
    alert("IE 7.0");
}
```
