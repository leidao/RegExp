# RegExp
### 参考资料链接
  > [正则表达式速查表](https://www.jb51.net/tools/regexsc.htm)  
  > [正则表达式在线测试](https://tool.oschina.net/regex/)  

## `正则表达式由两部分组成`
- 元字符
- 修饰符
### 常用的元字符 
```
1.量词元字符，设置出现的次数
*      零到多次
+      一到多次
？     零或一次
{n}    出现n次
{n,}   出现n到多次
{n,m}  出现n到m次  （\d{3,5} => 匹配三到五个0～9的任意数字）
```
```
2.特殊元字符:单个或者组合在一起代表特殊的含义
\      转义字符 
.      除\n(换行符)以外的任意字符  （.+ => 一到多个除\n以外的任意字符）
^      以哪一个元字符开始
$      以哪一个元字符结束
\n     换行符
\d     0～9之间的任意数字
\D     非0~9之间的任意数字
\w     数字，字母，下划线中的任意字符（[a-zA-Z0-9_]）
\W     非数字，字母，下划线中的任意字符
\s     一个空白字符（包括空格、制表符、换页符）
\t     一个制表符（一个TAB健：4个空格）
\b     匹配一个单词的边界，单词和空格间的位置  
    >  /er\b/.text('nerer')=> 可以匹配到'er'  /er\b/.text('verb')=> 就没有匹配字符
\B     匹配非单词边界                      
    >  /er\B/.text('verb')=> 可以匹配到'er'  /er\B/.text('nerer')=> 就没有匹配字符
x|y    x或者y任意一个字符
[xyz]  x或者y或者z任意一个字符  
    >  /[xyz]/.text('xz')=> 可以匹配到'x'和'z'
[^xyz] 除了x/y/z以外的字符     
    >  /[^xyz]/.text('xgyz')=> 可以匹配到'g'
[a-z]  指定a-z这个范围内的任意字符
[^a-z] 非a-z这个范围内的任意字符
()     正则中的分组
(?:)   只匹配不捕获
(?=)   正向预查
(?!)   负向预查
```
```
3.普通元字符，代表本身含义
/ldx/  此正则匹配的就是'ldx'字符本身
```

### 常用的修饰符 `img`
```
i =>ignoreCase  忽略单词大小写匹配
          >    /abc/i.test('AbC') => 匹配到'abc'
m =>multiine    忽略换行匹配，可以进行多行匹配
g =>global      全局匹配
```

### 元字符详细解析
#### `^ $`
``` javacript
let reg = /^\d/;        //必须以0～9的任意数字开头
reg.test('ldx');        //false
reg.test('ldx-19');     //false
reg.test('2020-ldx');   //true

let reg = /\d$/         //必须以0～9的任意数字结尾
reg.test('ldx');        //false
reg.test('ldx-19');     //true
reg.test('2020-ldx');   //false

/* 举个例子，验证手机号码（以1开头，共有11位）*/
let reg = /^1\d{10}$/
```
#### `\`
```
let reg = \^2.3$\       //.代表的是除\n以外的任意字符
reg.test('2.3')         //true
reg.test('2@3')         //true
reg.test('23')          //false

let reg = \^2\.3$\      //\.使用转义字符，.代表的就是小数点==》.
reg.test('2.3')         //true
reg.test('2@3')         //false
reg.test('23')          //false

/* 举个例子，匹配字符串'\d' */
reg = /^\\d$/
reg.test('\\d')         //true  字符串'\d'也需要使用\进行转义
```
#### `x|y 和 ()`
```
let reg = /^18|27$/     //满足18开头或27结尾任一就匹配了
reg.test('18')          //true
reg.test('27')          //true
reg.test('182')         //true
reg.test('127')         //true
reg.test('1827')        //true
reg.test('187')         //true
reg.test('827')         //true
reg.test('17')          //false
reg.test('82')          //false

/* 直接x|y会存在很乱的优先级问题，一般都使用()进行分组，()会改变处理的优先级 */
let reg = /^(18|27)$/   //只能匹配18或者27中的一个了
reg.test('18')          //true
reg.test('27')          //true
reg.test('182')         //false
reg.test('127')         //false
reg.test('1827')        //false
reg.test('187')         //false
reg.test('827')         //false
```
#### `[]`
```
//1.中括号中出现的字符一般都代表本身的含义
let reg = /^[@+]$/    //字符‘@’或者字符‘+’中的一位
reg.test('@')         //true
reg.test('+')         //true
reg.test('@+')        //false

let reg = /^[10-49]$/  //代表1或0-4或9
reg.test('1')       //true
reg.test('9')       //true
reg.test('10')      //false
reg.test('29')      //false
reg.test('0')       //true
reg.test('2')       //true
reg.test('4')       //true

let reg = /^[\d]$/   //\d在中括号中还是代表0～9
reg.test('d')        //false
reg.test('\\')       //false

//2.中括号中不存在多位数
let reg = /^[18]$/
reg.test('1')       //true
reg.test('8')       //true
reg.test('18')      //false
```

### 常用的正则表达式
> 1.验证有效数字
```
/*
* 规则分析：
* 1.可能出现+ - 号，也可能不出现    [+-]?
* 2.一位0-9都可以，多位首位不能为0   (\d?|([1-9]\d+))
* 3.小数部分可能有可能没有，一旦有小数点后面必须跟数字  (\.\d+)?
*/
let reg = /^[+-]?(\d?|([1-9]\d+))(\.\d+)?$/
```
> 2.验证密码
```
/*
* 规则分析
* 1.必须有数字，字母，下划线  (\w)
* 2.长度为8-16位           {8,16}
*/
let reg = /^\w{8,16}$/
```
> 3.验证真实姓名
```
  /*
* 规则分析
* 1.必须是汉字    [\u4E00-\u9FA5]
* 2.长度2-10位    {2,10}
* 3.可能有译名 ‘尼古拉斯·赵四’  汉字·汉字·汉字
*/
let reg = /^[\u4E00-\u9FA5]{2,10}(·[\u4E00-\u9FA5]{2,10}){0,}$/
```
> 4.验证邮箱
```
/*
* 规则分析
*  1246559314@qq.com   yongsheng.kys@autonavi.com  ldx-18015816543@163.com
* 1.开头是数字，字母，下划线，1到多位                      \w+
* 2.可以是 -数字，字母，下划线，也可以是 .数字，字母，下划线  ((\.\w+)|(-\w+))*
* 3.@符后面跟着数字，字母，1到多位                         @[0-9a-zA-Z]+
* 4.多域名或者企业域名,@后面字母的补充                     ((\.|-)[0-9a-zA-z]+)*
    @qq.com.cn/@tenxun-yuewen-office.com
* 5.最后匹配结尾域名（.com/.cn/.net/.org/.vip...）       \.[0-9a-zA-z]+
*/
let reg = /^\w+((\.\w+)|(-\w+))*@[0-9a-zA-Z]+((\.|-)[0-9a-zA-z]+)*\.[0-9a-zA-z]+$/
```
> 5.验证身份证号码
```
/*
* 规则分析
* 1.共有18位
* 2.最后一位可能是X或者数字(X代表10)
* 3.身份证前六位是你所在的省市县区码
* 4.身份证中六位是你的出生年月日期
* 5.身份证后四位:
      最后一位   => X或者数字
      倒数第二位 => 偶数为女，奇数为男
      剩余两位是所在地公安局编码
*/
let reg = /^(\d{6})(\d{4})(\d{2})(\d{2})\d{2}(\d)(\d|X)$/
reg.exec('320481199312054610')  
["320481199312054610", "320481", "1993", "12", "05", "1", "0"] 
根据这些信息可以解析出一个人的大体信息
```
***
### 正则的捕获
> 实现正则捕获的方法
> - 正则RegExp.proptype上的方法  
>   1.exec  
>   2.test  
> - 字符串String.proptype上支持正则表达式处理的方法  
>   1.replace  
>   2.match  
>   3.splite  
#### `exec` 基于exec实现正则的捕获
```
let str = 'ldx2020haohaoxuexi-2020-05-05'
1.捕获到的结果是null或者一个数组
  数组的第一项是本次捕获到的内容，如果有分组，则可以进行分组捕获
  index: 当前捕获内容在字符串中的起始下标
  input: 原始字符串
  let reg = /\d+/
  reg.exec(str)     // ["2020", index: 3, input: "ldx2020haohaoxuexi"]
  reg.exec(str)     // ["2020", index: 3, input: "ldx2020haohaoxuexi"]
  reg.exec(str)     // ["2020", index: 3, input: "ldx2020haohaoxuexi"]

2.每执行一次exec，只能捕获到一个符合正则规则的内容 => '正则捕获的懒惰性',需要进行全局匹配，才能捕获所有符合正则的内容
  let reg = /\d+/g
  reg.exec(str)     // ["2020", index: 3, input: "ldx2020haohaoxuexi"]
  reg.exec(str)     // ["2020", index: 19, input: "ldx2020haohaoxuexi"]
  reg.exec(str)     // ["05", index: 24, input: "ldx2020haohaoxuexi"]

  function execAll(reg,str){
    if(!reg.global) {
      throw Error('请使用全局匹配')
      return;
    }
    let value = reg.exec(str)
    let result = []
    while(value){
      result.push(value[0])
      value = reg.exec(str)
    }
      return result
  }
  console.log( execAll(reg,str))     //["2020", "2020", "05", "05"]
```
> 懒惰性匹配的原因:  
> reg.lastIndex  => 0  当前正则下次匹配的索引位置，默认值为0  
> 默认情况下lastIndex不会被修改  
> 第一次匹配捕获完成，lastIndex值没有改变，所以下一次exec依然从字符串最开始匹配，找到的永远是第一个匹配的  
> 使用全局匹配g修饰符后，会自动修改lastIndex值，下次exec会从上一次捕获到字符串结尾下标位置开始匹配  
> 当捕获不到内容后，捕获结果为null，会将lastIndex值设为0  
  #### `match` 基于match实现正则的捕获
  ```
  let str = 'ldx2020haohaoxuexi-2020-05-05'
  let reg = /\d+/
  str.match(reg)             // ["2020", index: 3, input: "ldx2020haohaoxuexi-2020-05-05"]

  全局匹配
  let str = 'ldx2020haohaoxuexi-2020-05-05'
  let reg = /\d+/g
  str.match(reg)            // ["2020", "2020", "05", "05"]
  ``` 
  > match:
  > 使用match进行全局匹配，可以捕获所有符合正则规则的字符
  ***
  #### `test` 基于test实现正则的捕获
  ```
  let str = '{0}年{1}月{2}日'
  let reg = /\{(\d+)\}/g
  reg.test(str)     // true
  RegExp.$1         // 0   => 获取正则匹配的第一个分组
  reg.test(str)     // true
  RegExp.$1         // 1   => 获取正则匹配的第一个分组
  reg.test(str)     // true
  RegExp.$1         // 2   => 获取正则匹配的第一个分组
  reg.test(str)     // false
  RegExp.$1         // 2   => 存的是上一个分组捕获的结果
  RegExp.$1～RegExp.$9   =》 获取当前匹配正则匹配后，第一个到第九个分组的捕获结果
  ```
  #### `replace` 基于replace实现正则的捕获
  ```
  let str = 'zhufeng@2019|zhufeng@2020'
  str.replace(/zhufeng/g,function(...agrs){
    console.log(agrs)  
    //["zhufeng", 0, "zhufeng@2019|zhufeng@2020"]
    //["zhufeng", 13, "zhufeng@2019|zhufeng@2020"]
    return '珠峰'
  })
  //全局匹配，匹配到几次就执行几次回调函数，并且把捕获到的信息作为函数参数传入
  console.log(str)  // "珠峰@2019|珠峰@2020"

  ===> 时间转换
  let str = '2018-12-25'
  let reg = /^(\d{4})-(\d{1,2})-(\d{1,2})$/g
  str.replace(reg,function(big,$1,$2,$3){
    console.log(big,$1,$2,$3)   //'2018-12-25' '2018' '12' '25'
  })
  str.replace(reg,'$1年$2月$3日')    //   "2018年12月25日"

  ===> 单词首字母大写
  let str = 'good good study, day day up!'
  let reg = /\b([a-zA-Z])[a-zA-Z]*\b/g
  str.replace(reg,function(...args){
    let [big,$1] = args
    return $1.toUpperCase()+big.substring(1)
  })
  ```
  ***
  #### `()` 正则的分组捕获
  ```
  => 身份证号码
  let str = '420481199312053610'
  let reg = /^(\d{6})(\d{4})(\d{2})(\d{2})\d{2}(\d)(\d|X)$/
  str.match(reg)     //match
  reg.exec(str)      //exec
  //   ["420481199312053610", "420481", "1993", "12", "05", "1", "0", index: 0, input: "420481199312053610"]
  //   每一个小分组单独捕获匹配的结果

  => ?: 只匹配不捕获
  let reg = /^(\d{6})(\d{4})(\d{2})(\d{2})\d{2}(\d)(?:\d|X)$/
  reg.exec(str)      //exec
  //   ["420481199312053610", "420481", "1993", "12", "05", "1",  index: 0, input: "420481199312053610"]
  //   最后一位‘0’没有进行捕获

  `?:`  匹配不捕获  
  let reg = /^\{\{((?:.|\n)+?)\}\}$/    一次匹配
  let str = '{{ hello }}'  
  str.match(reg)  
  ["{{ hello }}", " hello ", index: 0, input: "{{ hello }}"]  
  let reg = /^\{\{((?:.|\n)+?)\}\}$/g    全局匹配  
  str.match(reg)  
  ["{{ hello }}"]     只能拿到大正则匹配结果，分组匹配结果无法捕获
  ```
  #### `()` 正则的分组引用
  ```
  let str = 'book'   //'moon,good,look' ....
  let reg = /^[a-zA-Z][a-zA-Z]\1[a-zA-Z]$/ 
  分组引用就是通过‘\数字’让其代表和对应分组出现一模一样的内容 ==》 ‘oo’多个‘o’同时出现
  reg.test('book')   //true
  reg.test('deep')   //true
  reg.test('bone')   //false
  ```
  > #### `()`分组的三个作用：  
  > 1.优先级改变  
  > 2.分组捕获  
  > 3.分组引用  
  ***
  ### 正则捕获的贪婪性
  ```
  let str = '2020ldxhaohaoxuexi - 0505'
  let reg = /\d+/g
  str.match(reg)   //['2020','0505']
  // ‘\d+’ 一到多个数字 =》'2020' 符合其规则，但是'20'也符合其规则，为什么匹配的不是‘20’呢，这就是正则捕获的贪婪性。按照当前 
    匹配的最长结果来捕获的。

  let reg = /\d+?/g   
  str.match(reg)   //["2", "0", "2", "0", "0", "5", "0", "5"]
  //在量词元字符后面使用’?‘,作用是取消其贪婪匹配（按照正则匹配的最短结果来捕获）
  ```
  > #### `?`在正则中的作用：
  > 1.'?'左边的是非量词元字符：本身代表量词元字符，其出现零到一次。  
  > 2.'?'左边是量词元字符：取消捕获时的贪婪匹配  
  > 3.'?:':只匹配不捕获  
  > 4.'?=':正向预查  
  > 5.'?!':负向预查  

