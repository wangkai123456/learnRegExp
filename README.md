###<div align="center">正则表达式</div>
一、概念
>什么是正则？
描述了一种字符串匹配的模式，可以用来检查一个字符串是否含有某种子串，将匹配的的子串做替换或者从某个字符串中取出符合某个条件的子串等。

二、创建方式
>1、字面量创建方式
>let reg = /pattern/flags
>2、实例创建方式
>let reg = new RegExp(pattern,flags)
>>pattern:正则表达式  
>>flags:标识(修饰符)
>>标识主要包括：
>>a、i 忽略大小写匹配
>>b、m 多行匹配，即在到达一行文本末尾时还会继续寻常下一行中是否与正则匹配的项
>>c、g 全局匹配 模式应用于所有字符串，而非在找到第一个匹配项时停止

>区别：
>>a、字面量创建方式不能进行字符串拼接，实例创建方式可以
>>```
>><script>
>>    let str = "aa"
>>    let reg1 = new RegExp(str + '1')
>>    let reg2 = /str/
>>    console.log(reg1)  // /aa1/
>>    console.log(reg2)  // /str/
>></script>
>>```
>>b、字面量创建方式特殊字符不需要转义，实例创建方式需要转义
>>```
>>let reg1 = new RegExp('\d')     //    /d/ 
>>let reg2 = new RegExp('\\d')    //   /\d/
>>let reg3 = /\d/;               //  /\d/
>>```

三、修饰符
>1. i 忽略大小写匹配
>2. m 多行匹配，即在到达一行文本末尾时还会继续>>寻常下一行中是否与正则匹配的项
>3. g 全局匹配 模式应用于所有字符串，而非在找到>第一个匹配项时停止

>说明：可以一次匹配多个修饰符
>>```
>>let str = '33ahGG9904445'
>>let reg = /[a-z]/ig
>>let str1 = str.replace(reg, '9')
>>console.log(str1)
>>// 3399999904445
>>```

四、元字符
>1、代表特殊含义的元字符
\d : 0-9之间的任意一个数字  \d只占一个位置
\w : 数字，字母 ，下划线 0-9 a-z A-Z _
\s : 空格或者空白等
\D : 除了\d
\W : 除了\w
\S : 除了\s
 . : 除了\n之外的任意一个字符
 \ : 转义字符
 | : 或者
() : 分组
\n : 匹配换行符
\b : 匹配边界 字符串的开头和结尾 空格的两边都是边界 => 不占用字符串位数
 ^ : 限定开始位置 => 本身不占位置
 $ : 限定结束位置 => 本身不占位置
[a-z] : 任意字母 []中的表示任意一个都可以
[^a-z] : 非字母 []中^代表除了
[abc] : abc三个字母中的任何一个 [^abc]除了这三个字母中的任何一个字符

>2、代表次数的量词元字符
>\* : 0到多个
>\+ : 1到多个
>? : 0次或1次 可有可无
>{n} : 正好n次
>{n,} : n到多次
>{n,m} : n次到m次

>3、量词出现在元字符后面 如\d+，限定出现在前面的元字符的次数
>>```
>>let str = '2255678800';
>>let reg = /\d{2}/g;
>>let res = str.match(reg);
>>console.log(res)  //["22", "55", "67", "88", "00"]
>>```
>>```
>>let str = '  我是空格君  ';
>>let reg = /^\s+|\s+$/g; //匹配开头结尾空格
>>let res = str.replace(reg, '');
>>console.log('(' + res + ')') //(我是空格君)
>>```

>4、一般[]中的字符没有特殊含义 如.就表示.
但是像\w这样的还是有特殊含义的
>>```
>>let str1 = 'abc';
>>let str2 = 'dbc';
>>let str3 = '.bc';
>>let reg = /[ab.]bc/; //此时的.就表示.
>>reg.test(str1)  //true
>>reg.test(str2)  //false
>>reg.test(str3)  //true
>>```

>5、[]中，不会出现两位数
>>例如:匹配从18到65年龄段所有的人
>>```
>>let reg = /[18-65]/;
>>reg.test('50')
>>```
>>```
>>let reg = /(18|19)|([2-5]\d)|(6[0-5])/;
>>reg.test('50')
>>```

>6、()的提高优先级功能:凡是有|出现的时候，我们一定要注意是否有必要加上()来提高优先级
>>只要在正则中出现了括号就会形成一个分组
>>```
>>let str = 'foot';
>>let reg = /(f|m)oot/;
>>let res = reg.exec(str);
>>console.log(res);
>>//["foot", "f", index: 0, input: "foot"]
>>```
>>当我们加()只是为了提高优先级而不想捕获小分组时，可以在()中加?:来取消分组的捕获
>>(?=)会作为匹配校验，但不会出现在匹配结果字符串里面
>>(?:)会作为匹配校验，并出现在匹配结果字符里面，它跟(...)不同的地方在于，不作为子匹配返回
>>```
>>let str = 'aaabbb';
>>let reg = /(a+)(?:b+)/;
>>let res =reg.exec(str);
>>console.log(res)
>>//["aaabbb", "aaa", index: 0, input: "aaabbb"
>>```

>正则运算符的优先级
>>1、正则表达式从左到右进行计算，并遵循优先级顺序，这与算术表达式非常类似。
>>2、相同优先级的会从左到右进行运算，不同优先级的运算先高后低。
>>下面是常见的运算符的优先级排列
>>依次从最高到最低说明各种正则表达式运算符的优先级顺序：
>>
>>\ : 转义符
>>(), (?:), (?=), []  => 圆括号和方括号
>>*, +, ?, {n}, {n,}, {n,m}   => 量词限定符
>>^, $, \任何元字符、任何字符 
>>|       => 替换，"或"操作

五、正则的特性
>1、贪婪性：所谓的贪婪性就是正则在捕获时，每一次会尽可能多的去捕获符合条件的内容。
如果我们想尽可能的少的去捕获符合条件的字符串的话，可以在量词元字符后加?
>2、懒惰性则是正则在成功捕获一次后不管后边的字符串有没有符合条件的都不再捕获。
如果想捕获目标中所有符合条件的字符串的话，我们可以用标识符g来标明是全局捕获

六、常用方法
>1、实例方法
>>test:
>>reg.test(str) 用来验证字符串是否符合正则 符合返回true 否则返回false
>>```
>>let str = 'abc';
>>let reg = /\w+/;
>>console.log(reg.test(str));  //true
>>```
>>exec:
>>eg.exec() 用来捕获符合规则的字符串
>>```
>>let str = 'abc123cba456aaa789';
>>let reg = /\d+/;
>>console.log(reg.exec(str))
>>// ["123", index: 3, input: "abc123cba456aaa789", groups: undefined]
>>```
>>reg.exec捕获的数组中 
>>// [0:"123",index:3,input:"abc123cba456aaa789"]
>>0:"123" 表示我们捕获到的字符串
>>index:3 表示捕获开始位置的索引
>>input 表示原有的字符串
>>```
>>let str = 'abc123cba456aaa789';
>>let reg = /\d+/g;  //此时加了标识符g
>>console.log(reg.lastIndex)
>>// lastIndex : 0 
>>
>>console.log(reg.exec(str))
>>//  ["123", index: 3, input: "abc123cba456aaa789"]
>>console.log(reg.lastIndex)
>>// lastIndex : 6
>>
>>console.log(reg.exec(str))
>>// ["456", index: 9, input: "abc123cba456aaa789"]
>>console.log(reg.lastIndex)
>>// lastIndex : 12
>>
>>console.log(reg.exec(str))
>>// ["789", index: 15, input: "abc123cba456aaa789"]
>>console.log(reg.lastIndex)
>>// lastIndex : 18
>>
>>console.log(reg.exec(str))
>>// null
>>console.log(reg.lastIndex)
>>// lastIndex : 0
>>```

>exec的捕获还受分组()的影响
>>```
>>let str = '2017-01-05';
>>let reg = /-(\d+)/g
>>// ["-01", "01", index: 4, input: "2017-01-05"]
>>"-01" : 正则捕获到的内容
>>"01"  : 捕获到的字符串中的小分组中的内容
>>```

>2、字符串方法
>>match:
>> 如果匹配成功，就返回匹配成功的数组，如果匹配不成功，就返回null
>>```
>>match和exec的用法差不多
>>let str = 'abc123cba456aaa789';
>>let reg = /\d+/;
>>console.log(reg.exec(str));
>>//["123", index: 3, input: "abc123cba456aaa789",groups: undefined]
>>console.log(str.match(reg));
>>//["123", index: 3, input: "abc123cba456aaa789",groups: undefined]
>>```
>>但全局搜索时区别很明显
>>```
>>//match和exec的用法差不多
>>let str = 'abc123cba456aaa789';
>>let reg = /\d+/g;
>>console.log(reg.exec(str));
>>//["123", index: 3, input: "abc123cba456aaa789",groups: undefined]
>>console.log(str.match(reg));
>>//["123", "456", "789"]
>>```
>match受到分组()的影响,但只是在没有全局搜索的情况下
>>```
>>let str = 'abc';
>>let reg = /(a)(b)(c)/;
>>console.log( str.match(reg) );
>>// ["abc", "a", "b", "c", index: 0, input: "abc"]
>>```
>>```
>>let str = 'abc';
>>let reg = /(a)(b)(c)/g;
>>console.log( str.match(reg) );
>>// ["abc"]
>>```

>>replace:
>>str.replace()正则去匹配字符串，匹配成功的字符去替换成新的字符串
>>```
>>let str = 'a111bc222de';
>>let res = str.replace(/\d/g,'A')
>>console.log(res)
>>// "aAAAbcAAAde"
>>```
>>replace()的第二个参数支持函数,在这种情况下，每个匹配都调用该函数，它返回的字符串将作为替换文本使用。
>>```
>>let str = '2017-01-06';
>>str = str.replace(/-\d+/g, function() {
>>console.log(arguments)
>>})
>>//["-01", 4, "2017-01-06"]
>>//["-06", 7, "2017-01-06"]
>>```

>>search:
>>传入一个正则表达式，并使用该表达式进行匹配；如果配失败，则会返回-1,如果匹配成功，则会返回匹配开始的下标。可以理解为是一个正则版的indexOf
>>```
>>let str = 'abc123cba456aaa789';
>>let newStr = str.search("b")
>>console.log(newStr)  //1 下标
>>```
>>注：无论有没有/g,无论执行几次结果都是返回第一次出现的下标

>>split:
>>```
>>let str = "2019-01-01"
>>let newStr = srt.aplit("-")
>>console.log(newStr)
>> // ["2019", "01", "01"]
>>```
>>```
>>let str = '9,8,6,,6,[0,6,5]';
>>let newStr = str.split(",")
>>console.log(newStr)
>>//["9", "8", "6", "", "6", "[0", "6", "5]"]
>>```
>>```
>>let str = '9,8,6,,6,[0,6,5]';
>>let newStr = str.split(/,(?![,\d]+])/)
>>console.log(newStr)
>>//["9", "8", "6", "", "6", "[0,6,5]"]
>>```

七、零宽断言
>1、零宽度正预测先行断言 
>>(?=exp)这个简单理解就是说字符出现的位置的右边必须匹配到exp这个表达式。
>>```
>>let str = "i'm singing and dancing";
>>let reg = /\b(\w+(?=ing\b))/g
>>let res = str.match(reg);
>>console.log(res)
>>// ["sing", "danc"]
>>```

>2、零宽度负预测先行断言 
>>(?!exp)这个就是说字符出现的位置的右边不能是exp这个表达式
>>```
>>let str = 'nodejs';
>>let reg = /node(?!js)/;
>>console.log(reg.test(str)) // false
>>```

>3、零宽度正回顾后发断言 
>>(?<=exp)这个就是说字符出现的位置的前边是exp这个表达式。
```
let str = '￥998$888';
let reg = /(?<=\$)\d+/;
console.log(reg.exec(str)) //888
```

>4、零宽度负回顾后发断言 
>>(?<!exp)这个就是说字符出现的位置的前边不能是exp这个表达式。
>>```
>>let str = '￥998$888';
>>let reg = /(?<!\$)\d+/;
>>console.log(reg.exec(str)) //998
>>```

>https://www.cnblogs.com/macq/p/6597366.html
