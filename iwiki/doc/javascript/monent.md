# Monent常用方法的总结

官方文档地址：http://momentjs.cn/

## 安装 & 导入
```
npm install moment --save

import moment from 'moment';
```
## 获取时间

获取时间有两个非常重要的方法：
`startOf(String)`：获取时间对象的开始时间
`endOf(String)`：获取时间对象的结束时间

### 获取当前时间
示例：获取当前时间并格式化字符串
```JavaScript
// 格式化当前日期
moment().format("YYYY-MM-DD")
// 格式胡当前日期时间
moment().format("YYYY-MM-DD HH:mm:ss")
```
示例：将字符串时间格式化为moment时间对象

参数：
+ 字符串时间
+ 格式化格式

```JavaScript
// 字符串时间转为moment对象
const cur_str_time = moment().format("YYYY-MM-DD HH:mm:ss")
// 将其进行装换
moment(cur_str_time,'YYYY-MM-DD HH:mm:ss');
```

示例：指定时间日期的时间戳
```
moment('2019-12-06 00:00:00').valueOf()
```

示例：当前时间戳转换为字符串时间
```
function formatDate(date) {
     var year=date.getFullYear();
     var month=date.getMonth()+1;
     var day=date.getDate();
     var hour=date.getHours();
     var minute=date.getMinutes();
     var second=date.getSeconds();
    //  return year+"-"+month+"-"+date+" "+hour+":"+minute+":"+second;
     return year + '-' + (String(month).length > 1 ? month : '0' + month) + '-' +
     (String(day).length > 1 ? day : '0' + day) + ' ' + (String(hour).length > 1 ? hour : '0' + hour) + ':' + (String(minute).length > 1 ? minute : '0' + minute)
      + ':' + (String(second).length > 1 ? second : '0' + second)
}
//如果记得时间戳是毫秒级的就需要*1000 不然就错了记得转换成整型
var d=new Date(1553547600000); //Tue Mar 26 2019 05:00:00 GMT+0800 (中国标准时间)
console.log(formatDate(d)) //2019-03-26 05:00:00
```

### 自然时间的获取

#### 天为单位自然时间

说明：
开始时间：0时0分0秒
结束时间：23时59分59秒

示例：
```
moment().startOf('day').format('YYYY-MM-DD HH:mm:ss') // 起始时间（今天的00:00:00点）
moment().format('YYYY-MM-DD HH:mm:ss') // 结束时间（当前系统的时间）
```
#### 周围单位自然时间
```
moment().startOf('week').format('YYYY-MM-DD HH:mm:ss') // 起始时间（当前周周一的00:00:00点）
moment().format('YYYY-MM-DD HH:mm:ss') // 结束时间（当前系统的时间）
```
#### 月为单位自然时间
```
moment().startOf('months').format('YYYY-MM-DD HH:mm:ss') // 起始时间（当前月1号的00:00:00点）
moment().format('YYYY-MM-DD HH:mm:ss') // 结束时间（当前系统的时间）
```
#### 季度为单位自然时间
```
moment().startOf('quarters').format('YYYY-MM-DD HH:mm:ss') // 起始时间（当前季度1号的00:00:00点）
moment().format('YYYY-MM-DD HH:mm:ss') // 结束时间（当前系统的时间）
```
#### 半年为单位自然时间(比较特殊，需要区分上半年和下半年)
```
moment().get('month') + 1 <= 6
                ? moment().format(`${moment().get('year')}-01-01 00:00:00`)
                : moment().format(`${moment().get('year')}-07-01 00:00:00`)
// 获取当前月+1然后和6进行比较区分上下半年，上半年就计算1月1号的00点，下半年就计算7月1号的00点
moment().format('YYYY-MM-DD HH:mm:ss') // 结束时间（当前系统的时间）
```
#### 年为单位自然时间
```
moment().startOf('years').format('YYYY-MM-DD HH:mm:ss') // 起始时间（当前年1号的00:00:00点）
moment().format('YYYY-MM-DD HH:mm:ss') // 结束时间（当前系统的时间）
```

### 常用的时间区间获取

#### 前一天的时间区间
```JavaScript
moment().day(moment().day() - 1).format('YYYY-MM-DD HH:mm:ss') // 当前时间往前推一天的时间
moment().format('YYYY-MM-DD HH:mm:ss') // 结束时间（当前系统的时间）
```
#### 上一周的时间区间
```JavaScript
moment().day(moment().day() - 6).format('YYYY-MM-DD HH:mm:ss') // 当前时间往前推一周的时间
moment().format('YYYY-MM-DD HH:mm:ss') // 结束时间（当前系统的时间）
```
#### 上一个月的时间区间
```JavaScript
moment().subtract(1, 'months').format('YYYY-MM-DD HH:mm:ss') // 当前时间往前推一个月的时间
moment().format('YYYY-MM-DD HH:mm:ss') // 结束时间（当前系统的时间）
```
#### 上个季度的时间区间
```JavaScript
moment().subtract(1, 'quarters').format('YYYY-MM-DD HH:mm:ss') // 当前时间往前推三个月的时间
moment().format('YYYY-MM-DD HH:mm:ss') // 结束时间（当前系统的时间）
```
#### 半年的时间区间
```JavaScript
moment().subtract(6, 'months').format('YYYY-MM-DD HH:mm:ss') // 当前时间往前推六个月的时间
moment().format('YYYY-MM-DD HH:mm:ss') // 结束时间（当前系统的时间）
```
#### 上年的时间区间
```JavaScript
moment().subtract(1, 'years').format('YYYY-MM-DD HH:mm:ss') // 当前时间往前推一年的时间
moment().format('YYYY-MM-DD HH:mm:ss') // 结束时间（当前系统的时间）
```

## 格式化时间

格式化时间的方法：
`moment().format()`：默认的格式化时间
`moment().format(String)`：根据时间格式化的格式进行格式化

### 格式化年月日： 'xxxx年xx月xx日'
```
moment().format('YYYY年MM月DD日')
```
### 格式化年月日： 'xxxx-xx-xx'
```
moment().format('YYYY年MM月DD日')
```
### 格式化时分秒(24小时制)： 'xx时xx分xx秒'
```
moment().format('YYYY年MM月DD日')
```
### 格式化时分秒(12小时制)：'xx:xx:xx am/pm'
```
moment().format('hh:mm:ss a')
```
### 格式化时间戳(以秒为单位)
```
moment().format('X') // 返回值为字符串类型
```
### 格式化时间戳(以毫秒为单位)
```
moment().format('x') // 返回值为字符串类型
```




参考地址：
[moment获取时间](https://www.cnblogs.com/lgt-hello-world/p/13376541.html)
[moment获取自然时间和近期时间](https://www.jianshu.com/p/7893dd23195b)
[常用方法总结](https://segmentfault.com/a/1190000015240911)
[moment格式和字符串](https://www.cnblogs.com/gslgb/p/12581045.html)
[时间戳转换为字符串](https://segmentfault.com/a/1190000021300729)
