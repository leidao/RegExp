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
 #### `x|y`
 ```
 let reg = /18|27/
 reg.test('18')          //true
 reg.test('27')          //true
 reg.test('182')         //true
 reg.test('127')         //true
 reg.test('1827')        //true
 reg.test('187')         //true
 reg.test('827')         //true
 ```