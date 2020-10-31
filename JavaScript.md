## 表达式与运算法

### 左移

```js
9 << 2  --------> 9 * 2**2
9 << 3  --------> 9 * 2**3
express * 2 ** n
```

### 右移

```js
9 >> 2 ------> 2
express * (1/2 ** n)
```

### 生成器

```js


```

### 立即调用函数表达式

```js
<a href="javascript:void(0);">
  这个链接点击之后不会做任何事情，如果去掉 void()，
  点击之后整个页面会被替换成一个字符 0。
</a>
<p> chrome中即使<a href="javascript:0;">也没变化，firefox中会变成一个字符串0 </p>
<a href="javascript:void(document.body.style.backgroundColor='green');">
  点击这个链接会让页面背景变成绿色。
</a>
```



### &&=

```
let a = 1;
let b = 0;

a &&= 2;
console.log(a);
// expected output: 2

b &&= 2;
console.log(b);
// expected output: 0
```

### 逻辑空

```
const a = { duration: 50 };

a.duration ??= 10;
console.log(a.duration);
// expected output: 50

a.speed ??= 25;
console.log(a.speed);
// expected output: 25


x ?? (x = y);
```

### 可选链操作

```js
可选链操作符( ?. )允许读取位于连接对象链深处的属性的值，而不必明确验证链中的每个引用是否有效。?. 操作符的功能类似于 . 链式操作符，不同之处在于，在引用为空(nullish ) (null 或者 undefined) 的情况下不会引起错误，该表达式短路返回值是 undefined。与函数调用一起使用时，如果给定的函数不存在，则返回 undefined。
1.================
const adventurer = {
  name: 'Alice',
  cat: {
    name: 'Dinah'
  }
};

const dogName = adventurer.dog?.name;
console.log(dogName);
// expected output: undefined

2.=================
var a = {name:'alex'}
a?.name ====> return 'alex'
```



## 内置对象

### Array

[参考链接](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/concat)

#### 属性

```
1.length   返回数组的长度  [number]
```

#### 方法

```js
1.from 
console.log(Array.from('foo'));
// expected output: Array ["f", "o", "o"]

console.log(Array.from([1, 2, 3], x => x + x));
// expected output: Array [2, 4, 6]

2.isArray 判断是否是一个数组
Array.isArray([1, 2, 3]);  
// true
Array.isArray({foo: 123}); 
// false
Array.isArray("foobar");   
// false
Array.isArray(undefined);  
// false

3. Array.of    创建一个数组
Array.of(7);       // [7] 
Array.of(1, 2, 3); // [1, 2, 3]
Array(7);          // [ , , , , , , ]
Array(1, 2, 3);    // [1, 2, 3]

====================================== 原型链上================================
1.Array.prototype.concat()
const array1 = ['a', 'b', 'c'];
const array2 = ['d', 'e', 'f'];
const array3 = array1.concat(array2);
console.log(array3);
// expected output: Array ["a", "b", "c", "d", "e", "f"]
2.copywithin
const array1 = ['a', 'b', 'c', 'd', 'e'];
// copy to index 0 the element at index 3
console.log(array1.copyWithin(0, 3, 4));
// expected output: Array ["d", "b", "c", "d", "e"]
// copy to index 1 all elements from index 3 to the end
console.log(array1.copyWithin(1, 3));
// expected output: Array ["d", "d", "e", "d", "e"]
语法：arr.copyWithin(target[, start[, end]])
3.entires 返回一个迭代器
const array1 = ['a', 'b', 'c'];
const iterator1 = array1.entries();
console.log(iterator1.next().value);
// expected output: Array [0, "a"]
console.log(iterator1.next().value);
// expected output: Array [1, "b"]
4. ================= every =========
语法：# arr.every(callback(element[, index[, array]])[, thisArg])  
当callback 返回的每一个元素为真时才为真
const isBelowThreshold = (currentValue) => currentValue < 40;
const array1 = [1, 30, 39, 29, 10, 13];
console.log(array1.every(isBelowThreshold));
5. ============= fill =================
语法：# arr.fill(value[, start[, end]])
const array1 = [1, 2, 3, 4];
// fill with 0 from position 2 until position 4

console.log(array1.fill(0, 2, 4));
// expected output: [1, 2, 0, 0]

// fill with 5 from position 1
console.log(array1.fill(5, 1));
// expected output: [1, 5, 5, 5]

console.log(array1.fill(6));
// expected output: [6, 6, 6, 6]

6. ============= filter ================
 语法 #var newArray = arr.filter(callback(element[, index[, array]])[, thisArg])
 const words = ['spray', 'limit', 'elite', 'exuberant', 'destruction', 'present'];
const result = words.filter(word => word.length > 6);
console.log(result);
// expected output: Array ["exuberant", "destruction", "present"]
7.============= find ====================
 # arr.find(callback[, thisArg])
const array1 = [5, 12, 8, 130, 44];
const found = array1.find(element => element > 10);
console.log(found);
// expected output: 12
8. ================= findIndex ===========
与7 类似，返回的是 index 而不是值

9. ================= flat ============
    深度递归遍历数组  
# var newArray = arr.flat([depth])
depth 默认为1
# example
const arr1 = [0, 1, 2, [3, 4]];
console.log(arr1.flat());
// expected output: [0, 1, 2, 3, 4]
const arr2 = [0, 1, 2, [[[3, 4]]]];
console.log(arr2.flat(2));
// expected output: [0, 1, 2, [3, 4]]

//使用 Infinity，可展开任意深度的嵌套数组
var arr4 = [1, 2, [3, 4, [5, 6, [7, 8, [9, 10]]]]];
arr4.flat(Infinity);
// [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
10. ================ flatMap =================
# var new_array = arr.flatMap(function callback(currentValue[, index[, array]]) {    // return element for new_array}[, thisArg])
11. ============== forEach ==========
 # arr.forEach(callback(currentValue [, index [, array]])[, thisArg])

12. ================ includes ==========
 # arr.includes(valueToFind[, fromIndex])
fromIndex: 从什么位置开始查找
includes() 方法用来判断一个数组是否包含一个指定的值，根据情况，如果包含则返回 true，否则返回false。

13. ================ indexOf ==========
    # arr.indexOf(searchElement[, fromIndex])
14. ============== join ============ 
    拼接成字符串
arr = ['arr','foo','bar']
arr.join('') ----------> 'arrfoobar'

15. ============= keys ===============
 # arr.keys()

const array1 = ['a', 'b', 'c'];
const iterator = array1.keys();
// expected output: 0
// expected output: 1
// expected output: 2
16. ============= lastIndexOf ============
    逆向查找
17. ================= map ===============
#    var new_array = arr.map(function callback(currentValue[, index[, array]]) { //Return element for new_array }[, thisArg])

18 =================== pop =================
    # arr.pop()

19. ================ push =================
    # arr.push(element1, ..., elementN)

返回值:
当调用该方法时，新的 length 属性值将被返回。

20. ============= reduce =================
    
```





## WEB API

### XHR

#### 属性

```js
XMLHttpRequest.onreadystatechange
	当 readyState 属性发生变化时，调用的 EventHandler。
XMLHttpRequest.readyState
	返回 一个无符号短整型（unsigned short）数字，代表请求的状态码。
XMLHttpRequest.response
	返回一个 ArrayBuffer、Blob、Document，或 DOMString，具体是哪种类型取决于 			
	XMLHttpRequest.responseType 的值。其中包含整个响应实体（response entity body）。
XMLHttpRequest.responseText 

XMLHttpRequest.responseType
	一个用于定义响应类型的枚举值（enumerated value）。
XMLHttpRequest.responseURL
	返回经过序列化（serialized）的响应 URL，如果该 URL 为空，则返回空字符串。
XMLHttpRequest.status
	返回一个无符号短整型（unsigned short）数字，代表请求的响应状态。
	
XMLHttpRequest.timeout
	一个无符号长整型（unsigned long）数字，表示该请求的最大请求时间（毫秒），若超出该时间，请求会自动终止
	
XMLHttpRequest.withCredentials
	一个布尔值，用来指定跨域 Access-Control 请求是否应当带有授权信息，如 cookie 或授权 header 头。
```

#### 方法

```
XMLHttpRequest.abort()
	如果请求已被发出，则立刻中止请求。
XMLHttpRequest.getAllResponseHeaders()
	以字符串的形式返回所有用 CRLF 分隔的响应头，如果没有收到响应，则返回 null。
XMLHttpRequest.getResponseHeader()
	返回包含指定响应头的字符串，如果响应尚未收到或响应中不存在该报头，则返回 null。
XMLHttpRequest.open()
	初始化一个请求。该方法只能在 JavaScript 代码中使用，若要在 native code 中初始化请求，请使用 		openRequest()。
XMLHttpRequest.overrideMimeType()
	覆写由服务器返回的 MIME 类型。
XMLHttpRequest.send()
	发送请求。如果请求是异步的（默认），那么该方法将在请求发送后立即返回。
XMLHttpRequest.setRequestHeader()
	设置 HTTP 请求头的值。必须在 open() 之后、send() 之前调用 setRequestHeader() 方法。
XMLHttpRequest.open()
	初始化一个请求。该方法只能在 JavaScript 代码中使用，若要在 native code 中初始化请求，请使用 		openRequest()。
XMLHttpRequest.send()
	发送请求。如果请求是异步的（默认），那么该方法将在请求发送后立即返回。
XMLHttpRequest.overrideMimeType()
覆写由服务器返回的 MIME 类型。	
	
```

#### 事件

```
load
XMLHttpRequest请求成功完成时触发。
也可以使用 onload 属性

loadend
当请求结束时触发, 无论请求成功 ( load) 还是失败 (abort 或 error)。
也可以使用 onloadend 属性。

abort
当 request 被停止时触发，例如当程序调用 XMLHttpRequest.abort() 时。
也可以使用 onabort 属性。
error
当 request 遭遇错误时触发。
也可以使用 onerror 属性
```







## Questions

### 

```

```

