# js及jQuery中数组遍历方法
Q：修改原数组的值？
## 对比
| 方法 | 遍历对象 | 返回值 | IE8支持情况 |
|:------|:------|:------|:------|
| forEach | 数组 | 无返回 | 不支持 |
| reduce | 数组 | 单个返回数值 | 不支持 |
| map | 数组 | 返回一个数组 | 不支持 |
| $.map | 数组或对象 | 返回一个数组 | 支持 |
| $().map | Jq对象 | Jq对象 | 支持 |

## forEach
forEach() 方法对数组的每个元素执行一次提供的函数。
使用forEach方法，没有返回值，不修改数组本身，不支持IE8。
### 语法
`array.forEach(function(currentValue, index, arr), thisValue)`
### 参数
`function(currentValue, index, arr)`是数组中每个元素都需要调用的函数

| 参数 | 值 |
| :--------- |:------|
| currentValue| 必须，当前元素|
| index| 可选。当前元素的索引值 |
| arr |可选。当前元素所属的数组对象 |
|thisValue|可选参数。当执行回调函数时用作this的值|

### 示例
```
var sum = 0;
var arr = [1,2,3,4];
arr.forEach(function(v,i){
	sum = v+sum;
})
console.log(sum); //返回10
```

## reduce
reduce() 方法对累加器和数组中的每个元素（从左到右）应用一个函数，最终减少为单个值。
使用reduce()方法，将只返回一个累加值，适合用于计数等，不支持IE8。
### 语法
`array.reduce(function(total, currentValue, currentIndex, arr), initialValue)`
### 参数
`function(total, currentValue, currentIndex, arr)`是数组中每个元素都需要调用的函数

| 参数 | 值 |
| :--------- |:------|
| accumulator| 必须，累加器累加回调的返回值；它是上一次调用回调时返回的累积值，或initialValue|
| currentValue| 必须，当前元素|
| index| 可选。当前元素的索引值 |
| arr |可选。当前元素所属的数组对象 |
|initialValue|用作第一个调用 callback的第一个参数的值。 如果没有提供初始值，则将使用数组中的第一个元素。 在没有初始值的空数组上调用 reduce 将报错。|
### 示例
```
var sum = 0;
var arr = [1,2,3,4];
arr.reduce(function(x,y){
	return x+y;
});  //返回一个值 10
```

## map
map() 方法创建一个新数组，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果，不修改数组本身。

### 语法
`array.map(function(currentValue,index,arr), thisValue)`
### 参数
`function(currentValue, index, arr)`是数组中每个元素都需要调用的函数

| 参数 | 值 |
| :--------- |:------|
| currentValue| 必须，当前元素|
| index| 可选。当前元素的索引值 |
| arr |可选。当前元素所属的数组对象 |
|thisValue|可选参数。当执行回调函数时用作this的值|
### 示例
```
var arr = [1,2,3,4];
 arr.map(function(v,i){
	return arr[i]+1;
})
//num =[2,3,4,5]
```

## $.map
将一个数组中的所有元素转换到另一个数组中。
类数组（Array-like）对象——也就是那些含有.length属性 以及 在索引值为.length - 1 的位置有值的对象，必须将其转化成真正的数组之后才能传递给 $.map()方法使用。jQuery 库提供了 $.makeArray() 方法来完成这样的转换。
### 语法
`$.map( arrayOrObject, function( value, indexOrKey ))`
### 参数
| 参数 | 值 |
| :--------- |:------|
| arrayOrObject| 必须，待转换数组或对象|
| value| 可选。数组中元素或对象的值 |
| indexOrKey |可选。元素在数组中的索引值或该对象的键 |

### 示例
```
$.map( [0,1,2], function(n){
  return n+1;
});//返回[1,2,3]
```

## $().map
将一组元素转换成新的jq对象。
该方法的对象是一组元素，比如页面的一组li元素、一组input元素等，而非数组。生成的是一个新的jqurey对象，需要调用jq的get方法才能返回数组。
### 语法
`.map(function(index, currentValue))`
### 参数
`function(index, currentValue)`对当前对象集合中的每个元素调用的函数

| 参数 | 值 |
| :--------- |:------|
| index| 可选，当前元素的索引值 |
| currentValue |可选，当前元素 |
### 示例
```
//HTML代码
<ul>
  	<li class="arrli">1</li>
  	<li class="arrli">2</li>
  	<li class="arrli">3</li>
</ul>
//Js代码
var num=$(".arrli").map(function(i,v){
	return v.innerText
}).get();
//num = [1,2,3]
//console.log(num instanceof Array)  true
```
