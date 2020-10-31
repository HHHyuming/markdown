## 内置对象

### Array

[参考链接](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/concat)

#### 属性

```
1.length   返回数组的长度  [number]
```

#### 方法

```
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
4.

```



## WEB API

### XHR

#### 属性

```
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

