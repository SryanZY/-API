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
*注意在ES6中对于isNaN方法有一些修改，提加了Number.isNaN()来判断，对于非NaN的元素一律返回false*   
Number.isNaN('15') // false  
Number.isNaN(true) // false  
```  

