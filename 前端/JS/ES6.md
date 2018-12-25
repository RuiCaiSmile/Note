# ES6 （不全，待补充

## 1. let 与 const

### let

ES6 新增了 let 命令，用来声明变量。它的用法类似于 var，但是所声明的变量，只在 let 命令所在的代码块内有效。
let 与 var 不同，不存在变量提升，使用必须在定义之前；同时一个作用域内不可以重复声明。
var的变量提升，是提升了定义申明，而不是值。
```
{
  console.log(a);// ReferenceError: a is not defined.
  console.log(b);//1
  let a = 10;
  //Identifier 'a' has already been declared
  //let a = 11;
  let a = 11;
  var b = 1;
}
a // ReferenceError: a is not defined.
b // 1
```

### const

const 声明一个只读的常量,需要立即初始化。一旦声明，常量的值（理解为指向内存地址的指针）就不能改变。其作用域与 let 一样，也不存在变量提升，不可以重复声明。

## 2. 箭头函数

ES6 允许使用“箭头”（=>）定义函数。
示例如下：

```
var sum = (num1, num2) => num1 + num2;
var sum = (num1, num2) => { return num1 + num2; };
// 等同于
var sum = function(num1, num2) {
  return num1 + num2;
};


//由于大括号被解释为代码块，所以如果箭头函数直接返回一个对象，必须在对象外面加上括号，否则会报错。
// 报错
let getTempItem = id => { id: id, name: "Temp" };
// 不报错
let getTempItem = id => ({ id: id, name: "Temp" });
```

使用箭头函数的注意点：

- <b>函数体内的 this 对象，就是定义时所在的对象，而不是使用时所在的对象</b>
- 不可以当作构造函数，也就是说，不可以使用 new 命令，否则会抛出一个错误
- 不可以使用 arguments 对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替
- 不可以使用 yield 命令，因此箭头函数不能用作 Generator 函数。

## 3. 解构赋值

ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。
适用于数组、对象、函数参数等等，目前仅以数组举例：

```
let a = 1;
let b = 2;
let c = 3;
//等同于
let [a, b, c] = [1, 2, 3];

//不完全解构
let [x, y] = [1, 2, 3];
x // 1
y // 2
```

## 4. module

ES6 实现了模块（module）功能，使用时自动采用严格模式。
模块功能主要由两个命令构成：export 和 import。export 命令用于规定模块的对外接口，import 命令用于输入其他模块提供的功能。

### export

使用 export 关键字输出变量，以下代码在一个 js 文件内（profile.js):

```
//示例可能存在冲突，注释掉不同案例代码即可
//基本用法，直接输出变量
export let firstName = 'Magic';
let lastName = 'Yang';
export {firstName,lastName};


//输出方法体，用as可以重命名
export function getName(){
	return "yang";
};
function getAge(){
	return 19;
}
export {
	getName as name,
	getAge as age
};


//默认输出，import可以直接对接，不需要名称
export default function(){
	return "adv";
}
```

### import & export

使用 export 命令定义了模块的对外接口以后，其他 JS 文件就可以通过 import 命令加载这个模块。

import 命令输入的变量都是只读的，因为它的本质是输入接口。也就是说，不允许在加载模块的脚本里面，改写接口。

script 的标签 type 属性必须是 module。

```
<script type="module">
    import {firstName,lastName} from './profile.js';
    console.log(firstName+" "+lastName);

    //引入方法体，不直接执行
    import {getName,age} from './profile.js';
    console.log(getName()+","+age());

    //默认（default）输出可以直接命名
    import aaa from './profile.js';
    console.log(aaa());
</script>
```

补充：
export default 命令用于指定模块的默认输出。显然，一个模块只能有一个默认输出，因此 export default 命令只能使用一次。所以，import 命令后面才不用加大括号，因为只可能唯一对应 export default 命令。
export default 时，对应的 import 语句不需要使用大括号；第二组是不使用 export default 时，对应的 import 语句需要使用大括号

## 5. Promise

Promise 是 ES6 提供的异步编程的一种解决方案。

所谓 Promise，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。

Promise 对象有以下两个特点。

（1）对象的状态不受外界影响。Promise 对象代表一个异步操作，有三种状态：pending（进行中）、fulfilled（已成功）和 rejected（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是 Promise 这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。

（2）一旦状态改变，就不会再变，任何时候都可以得到这个结果。Promise 对象的状态改变，只有两种可能：从 pending 变为 fulfilled 和从 pending 变为 rejected。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）。如果改变已经发生了，你再对 Promise 对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。

### 示例

基本示例：

```
<script type="text/javascript">
  //resolve,reject是两个浏览器提供的函数，都可以接受一个参数，作为在then中使用的参数值
  new Promise((resolve,reject)=>{
    setTimeout(resolve,2000,{
      name:'aa',
      age:'12'
    });
    //then的第一个参数设置成功状态的执行函数，第二个参数设置失败状态的执行函数
  }).then((student)=>{
    let {name,age} = student;
    console.log(age);
  },(error)=>{
    console.log("失败");
  });
</script>
```

then 嵌套：

```
/* 第一个then方法指定的回调函数，返回的是另一个Promise对象。这时，第二个then方法指定的回调函数，就会等待这个新的Promise对象状态发生变化。如果变为resolved，就调用funcA，如果状态变为rejected，就调用funcB */

  getJSON("/post/1.json").then(
      post => getJSON(post.commentURL)
  ).then(
      comments => console.log("resolved: ", comments),
      err => console.log("rejected: ", err)
  );
```

## 5. Fetch(实验功能)

Fetch API 提供了一个 JavaScript 接口，用于访问和操纵 HTTP 管道的部分，例如请求和响应。它还提供了一个全局 fetch()方法，该方法提供了一种简单，合乎逻辑的方式来跨网络异步获取资源。

Fetch 发送 POST 请求与后台交互的时候，必须加头部，例如

```
  headers:{
     'Content-Type': 'application/json'
  },
```

fetch() 必须接受一个参数——资源的路径。无论请求成功与否，它都返回一个 promise 对象，resolve 对应请求的 Response。你也可以传一个可选的第二个参数 init（参见 Request）。

- 当接收到一个代表错误的 HTTP 状态码时，从 fetch()返回的 Promise 不会被标记为 reject， 即使该 HTTP 响应的状态码是 404 或 500。相反，它会将 Promise 状态标记为 resolve （但是会将 resolve 的返回值的 ok 属性设置为 false ）， 仅当网络故障时或请求被阻止时，才会标记为 reject。
- 默认情况下, fetch 不会从服务端发送或接收任何 cookies, 如果站点依赖于用户 session，则会导致未经认证的请求（要发送 cookies，必须设置 credentials 选项）

```
    fetch('goods.action').then((response)=>{
   			if(response.ok){
   				console.log("lucky");
   				response.json().then((json)=>{
   				//fetch需要通过response.json()来获取包含数据的promise，再通过then获取数据
   					console.log(json[0].name);
   				});
   			}else{
   				console.log("失败失败失败失败")
   			}
   		});
```

## 数组的扩展

### 扩展运算符与 rest 运算符

都是 `...`
目前理解是，在函数参数中出现的是 rest 运算符，代表将数组解析为一系列参数
在函数内出现的是扩展运算符，代表将一系列数组成一个数组

rest 参数之后不能再有其他参数（即只能是最后一个参数），否则会报错。
函数的 length 属性，不包括 rest 参数。

## symbol

###　定义方式

```
let s = Symbol();
let s1 = Symbol('bar');
<!-- 因为是基本数据类型，不能使用 new命令 -->
```

### 特性
定义的不同的symbol不相等，且不能与其他数据类型进行运算

```
// 没有参数的情况
let s1 = Symbol();
let s2 = Symbol();

s1 === s2 // false

// 有参数的情况
let s1 = Symbol('foo');
let s2 = Symbol('foo');

s1 === s2 // false
```
### 用处
可以用于复杂对象的属性命名
```
let mySymbol = Symbol();

// 第一种写法
let a = {};
a[mySymbol] = 'Hello!';

// 第二种写法
let a = {
  [mySymbol]: 'Hello!'
};

// 第三种写法
let a = {};
Object.defineProperty(a, mySymbol, { value: 'Hello!' });

// 以上写法都得到同样结果
a[mySymbol] // "Hello!"


<!-- Symbol 值作为对象属性名时，不能用点运算符。 -->
const mySymbol = Symbol();
const a = {};

a.mySymbol = 'Hello!';
a[mySymbol] // undefined
a['mySymbol'] // "Hello!"
```
