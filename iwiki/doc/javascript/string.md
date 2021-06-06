
# JavaScript字符串常用方法

## JavaScript去除字符串空格

### replice方法正则匹配替换

示例：去除字符串内所有的空格：
```
var str = " 6 6 ";
console.log('str: ', str);
var str_1 = str.replace(/\s*/g, "");
console.log('str_1: ', str_1); // 66
```
示例：去除字符串内两头的空格：
```
var str2 = " 6 6 ";
var str_2 = str.replace(/^\s*|\s*$/g, "");
console.log('str_2: ', str_2);//6 6//输出左右侧均无空格
console.log(str_2.length); // 3
```
示例：去除字符串内左侧的空格：
```
var str3 = " 6 6 ";
var str_3 = str.replace(/^\s*/,"");
console.log(str_3); //6 6 //输出右侧有空格左侧无空格
```
示例：去除字符串内右侧的空格：
```
var str4 = " 6 6 ";
var str_4 = str.replace(/(\s*$)/g,"");
console.log(str_4); // 6 6//输出左侧有空格右侧无空格
```

### strim()方法

trim()方法是用来删除字符串两端的空白字符并返回，trim方法并不影响原来的字符串本身，它返回的是一个新的字符串。

缺陷：只能去除字符串两端的空格，不能去除中间的空格

示例：去除两侧空格
```JavaScript
var str = " 6 6 ";
var str_1 = str.trim();
console.log(str_1); //6 6//输出左右侧均无空格
```
示例：去除左侧空格
```JavaScript
str.trimLeft(); //var str_1 = str.trimLeft();
```
示例：去除右侧空格
```JavaScript
str.trimRight();//var str_1 = str.trimRight();
```

### $.trim(str)方法

`$.trim()` 函数用于去除字符串两端的空白字符。

示例：去除左右均无空格
```
var str = " 6 6 ";
var str_1 = $.trim(str);
console.log(str_1); //6 6//输出左右侧均无空格
```
注意：$.trim()函数会移除字符串开始和末尾处的所有换行符，空格(包括连续的空格)和制表符。如果这些空白字符在字符串中间时，它们将被保留，不会被移除。





https://www.cnblogs.com/a-cat/p/8872498.html
