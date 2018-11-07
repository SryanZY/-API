# 数据类型   
## undefined、null以及布尔值   
```   
  undefined和null的区别：   
  1、null是一个表示“空”的对象，转为数值时为0；undefine是一个表示无定义的原始值，转为数值时为NaN  
  在布尔值中诸如null、unefined、‘’、''、false、0、NaN或转化为false，余下的皆转换为true   
 ```   
 
 ## 数值   
 ```  
 在js内部，所有数值以64位浮点型存储，所以1和1.0完全相同   
 js中可以准确表示绝对值小于2的53次方的整数，即Math.pow(2, 53)   
 
 如果一个数大于等于2的1024次方，那么就会发生“正向溢出”，即 JavaScript 无法表示这么大的数，这时就会返回Infinity   
 Math.pow(2, 1024) // Infinity   
 
 如果一个数小于等于2的-1075次方,那么就会发生为“负向溢出”，即 JavaScript 无法表示这么小的数，这时会直接返回0   
 Math.pow(2, -1075) // 0   
 
NaN不等于任何值，包括它本身  NaN === NaN  // false  
如果字符串的第一个字符不能转换为数字，则返回NaN  
parseInt('abc') // NaN  

isNaN只对数值有效，如果传入其他值，会被先转成数值。所以isNaN为true的值有可能不是NaN，而是一个字符串，对于对象和数组，
isNaN也返回true，但是，对于空数组和只有一个数值成员的数组，isNaN返回false  
isNaN({}) // true  
isNaN(['xzy']) // true  
isNaN([]) // false  
isNaN([123]) // false  

判断某个数值是否是NaN最好的方法是根据NaN不等于自身的特点  
function myselfIsNaN (value) {
  return value !== value
}     
```  
**注意在ES6中对于isNaN方法有一些修改，提加了Number.isNaN()来判断，对于非NaN的元素一律返回false**  
```  
Number.isNaN('15') // false  
Number.isNaN(true) // false  
```  

## 字符串  

btoa()：任意值转为 Base64 编码  
atob()：Base64 编码转为原来的值  
```  
var string = 'Hello World!';  
btoa(string) // "SGVsbG8gV29ybGQh"  
atob('SGVsbG8gV29ybGQh') // "Hello World!"  
```  
要将非 ASCII 码字符转为 Base64 编码，必须中间插入一个转码环节，再使用这两个方法。  
```  
function base64Encode (str) {
  return btoa(encodeURIComponent(str))
}  
function base64Decode (str) {
  return atob(decodeURIComponent(str))
}

base64Encode('你好') // "JUU0JUJEJUEwJUU1JUE1JUJE"  
base64Decode('JUU0JUJEJUEwJUU1JUE1JUJE') // "你好"  
```  

## 对象  

使用for in循环时需要注意的是：  
1、它只可遍历那些可以遍历的属性，即enumerable为true的属性  
2、它不仅遍历对象自身的属性，还遍历继承的属性  

## 函数  

## 数组  
```  
ES5中将类数组对象转换为数组的方法:  
Array.prototype.slice.call(),ES6中直接使用Array.from()  
类数组对象调用数组的forEach方法  
Array.prototype.forEach.call(str, function (index, item) {})  
```  

***  

# 运算符  

## 算术运算符  
### 加法运算符  
加法运算符是在运行时决定，到底是执行相加，还是执行连接。也就是说，运算子的不同，导致了不同的语法行为，这种现象称为“重载”（overload）  
```  
'3' + 4 + 5   // "345"  
3 + 4 + '5'   // "75"  
```  
### 余数运算符  
运算结果的正负号由第一个运算子的正负号决定  
```  
-1 % 2 // -1
1 % -2 // 1  
```  
所以，为了得到负数的正确余数值，可以先使用绝对值函数  
```  
function isOdd (n) {
  return Math.abs(n % 2) === 1
}  
isOdd(-5)   // true  
isOdd(-4)   // false  
```  
### 指数运算符  
指数运算符（**）完成指数运算，前一个运算子是底数，后一个运算子是指数  
```  
2 ** 3  // 8  
```  
**注意，指数运算符是右结合，而不是左结合。即多个指数运算符连用时，先进行最右边的计算**  
```  
// 相当于 2 ** (3 ** 2)
2 ** 3 ** 2
// 512  
```  
***  

# 语法专题  
## 数据类型转换  
* Number强制类型转换函数的规则如下：  
```  
// 字符串：如果不可以被解析为数值，返回 NaN  
Number('324abc') // NaN  

// 空字符串转为0  
Number('') // 0  

// 布尔值：true 转成 1，false 转成 0  
Number(true) // 1  
Number(false) // 0  

// undefined：转成 NaN  
Number(undefined) // NaN  

// null：转成0  
Number(null) // 0  
```  
Number函数将字符串转为数值，要比parseInt函数严格很多。基本上，只要有一个字符无法转成数值，整个字符串就会被转为NaN  
```  
parseInt('42 cats') // 42  
Number('42 cats') // NaN  
```  

* Number函数的参数是对象时将返回NaN，除非是包含单个数值的数组  
```  
Number([5]) // 5  
```  

* String强制类型转换函数的规则如下：  
```  
// undefined、null转为字符串undefined和null  
String(undefined) // "undefined"  
String(null) // "null"  
```  

* Boolean的转换规则:  
Boolean的转换规则相对简单，除了undefined、null、+0/-0、‘’、NaN剩下的全转换为true  
```  
Boolean(undefined) // false  
Boolean(null) // false  
Boolean(0) // false  
Boolean(NaN) // false  
Boolean('') // false   
Boolean({}) // true  
Boolean([]) // true  
Boolean(new Boolean(false)) // true  
```  

## 错误处理机制  
原生错误类型如下:  
1. SyntaxError 对象-解析代码时发生的语法错误  
```  
// 变量名错误  
var 1a;  
// Uncaught SyntaxError: Invalid or unexpected token  

// 缺少括号  
console.log 'hello');  
// Uncaught SyntaxError: Unexpected string  
```  
2. ReferenceError 对象-引用一个不存在的变量时发生的错误  
```  
// 使用一个不存在的变量  
unknownVariable  
// Uncaught ReferenceError: unknownVariable is not defined  
```  
3. RangeError 对象-是一个值超出有效范围时发生的错误  
```  
/ 数组长度不得为负数  
new Array(-1)  
// Uncaught RangeError: Invalid array length  
```  
4. TypeError 对象-变量或参数不是预期类型时发生的错误   
```  
new 123  
// Uncaught TypeError: number is not a func  

var obj = {};  
obj.unknownMethod()  
// Uncaught TypeError: obj.unknownMethod is not a function  
```  
5. URIError 对象-URI 相关函数的参数不正确时抛出的错误  
```  
decodeURI('%2')  
// URIError: URI malformed  
```  
6. EvalError 对象-eval函数没有被正确执行时，会抛出EvalError错误。该错误类型已经不再使用了，
只是为了保证与以前代码兼容，才继续保留  

## console对象与控制台  

* 对于某些复合型的数据，console.table可以将其整理成表格显示  
```  
var languages = [
  { name: "JavaScript", fileExtension: ".js" },
  { name: "TypeScript", fileExtension: ".ts" },
  { name: "CoffeeScript", fileExtension: ".coffee" }
];  
console.table(languages); 
```  

* console.dir方法用来对一个对象进行检查（inspect），并以易于阅读和打印的格式显示  
**该方法对于输出 DOM 对象非常有用，因为会显示 DOM 对象的所有属性**  
```  
console.dir(document.body)  
```  
Node 环境之中，还可以指定以代码高亮的形式输出  
```  
console.dir(obj, {colors: true})  
```  
* 控制台命令行API  
```  
$$(selector)-返回选中的 DOM 对象，等同于document.querySelectorAll  
inspect(object)-打开相关面板，并选中相应的元素，显示它的细节  
```  

# 标准库  
## Object对象  

通常来讲Object.keys()和Object.getOwnPropertyNames()方法获得的结果一样，只是前者不能返回不可枚举的属性
而后者可以返回。  
```  
var a = ['Hello', 'World'];
Object.keys(obj) // ["p1", "p2"]
Object.getOwnPropertyNames(obj) // ["p1", "p2", "length"]
对于数组来说，length是不可枚举的，所以只出现在Object.getOwnPropertyNames中
```  

*可以通过Object.prototype.toString.call方法来准确判断元素类型*  
```  
var type = function (o){
  var s = Object.prototype.toString.call(o);
  return s.match(/\[object (.*?)\]/)[1].toLowerCase();
};

type({}); // "object"
type([]); // "array"
type(5); // "number"
type(null); // "null"
type(); // "undefined"
type(/abcd/); // "regex"
type(new Date()); // "date"
```  

```  
var date = new Date();
date.toString() // "Tue Jan 01 2018 12:01:33 GMT+0800 (CST)"
date.toLocaleString() // "1/01/2018, 12:01:33 PM" 
日期的实例对象的toString和toLocaleString返回值不一样，而且toLocaleString的返回值跟用户设定的所在地域相关
```  

## 属性描述对象  
js的内部数据结构，即属性描述对象  
```  
{
  value: 123,
  writable: false,
  enumerable: true,
  configurable: false,
  get: undefined,
  set: undefined
}
writable 是一个布尔值，表示属性值（value）是否可改变（即是否可写），默认为true
enumerable 是一个布尔值，表示该属性是否可遍历(可枚举)，默认为true。如果设为false，会使得某些操作（比如for...in循环、Object.keys()）跳过该属性
configurable 是一个布尔值，表示可配置性，默认为true。如果设为false，将阻止某些操作改写该属性，比如无法删除该属性，也不得改变该属性的属性描述对象（value属性除外）。也就是说，configurable属性控制了属性描述对象的可写性
```  
