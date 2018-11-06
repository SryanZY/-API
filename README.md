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

# 语法专题  
## 数据类型转换  
1. Number强制类型转换函数的规则如下：  
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

