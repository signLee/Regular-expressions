======================
正则基础：
\ 做为转意，即通常在"\"后面的字符不按原来意义解释，如/b/匹配字符"b"，当b前面加了反斜杆后/\b/，转意为匹配一个单词的边界。 
-或- 
对正则表达式功能字符的还原，如"*"匹配它前面元字符0次或多次，/a*/将匹配a,aa,aaa，加了"\"后，/a\*/将只匹配"a*"。 

^ 匹配一个输入或一行的开头，/^a/匹配"an A"，而不匹配"An a" 
$ 匹配一个输入或一行的结尾，/a$/匹配"An a"，而不匹配"an A" 
* 匹配前面元字符0次或多次，/ba*/将匹配b,ba,baa,baaa 
+ 匹配前面元字符1次或多次，/ba*/将匹配ba,baa,baaa 
? 匹配前面元字符0次或1次，/ba*/将匹配b,ba 
(x) 匹配x保存x在名为$1...$9的变量中  括号里的东西要一起出现
x|y 匹配x或y 
{n} 精确匹配n次 
{n,} 匹配n次以上 
{n,m} 匹配n-m次 
[xyz] 字符集(character set)，表示中括号里的字符任一选一个 如果要匹配多个可以用 []* 或者[]+
[^xyz]  ^出现在中括号里表示不包含
[\b] 匹配一个退格符 
\b 匹配一个单词的边界 
\B 匹配一个单词的非边界 
\cX 这儿，X是一个控制符，/\cM/匹配Ctrl-M 
\d 匹配一个字数字符，/\d/ = /[0-9]/ 
\D 匹配一个非字数字符，/\D/ = /[^0-9]/ 
\n 匹配一个换行符 
\r 匹配一个回车符 
\s 匹配一个空白字符，包括\n,\r,\f,\t,\v等 
\S 匹配一个非空白字符，等于/[^\n\f\r\t\v]/ 
\t 匹配一个制表符 
\v 匹配一个重直制表符 
\w 匹配一个可以组成单词的字符(alphanumeric，这是我的意译，含数字)，包括下划线，如[\w]匹配"$5.98"中的5，等于[a-zA-Z0-9] 
\W 匹配一个不可以组成单词的字符，如[\W]匹配"$5.98"中的$，等于[^a-zA-Z0-9]。
(?=exp)    匹配不捕获   匹配exp,不捕获匹配的文本，也不给此分组分配组号
(?=exp)也叫零宽度正预测先行断言，它断言自身出现的位置的后面能匹配表达式exp。比如\b\w+(?=ing\b)，匹配以ing结尾的单词的前面部分(除了ing以外的部分)，如查找I'm singing while you're dancing.时，它会匹配sing和danc。
(?<=exp)也叫零宽度正回顾后发断言，它断言自身出现的位置的前面能匹配表达式exp。比如(?<=\bre)\w+\b会匹配以re开头的单词的后半部分(除了re以外的部分)，例如在查找reading a book时，它匹配ading。

零宽度负预测先行断言(?!exp)，断言此位置的后面不能匹配表达式exp。例如：\d{3}(?!\d)匹配三位数字，而且这三位数字的后面不能是数字；\b((?!abc)\w)+\b匹配不包含连续字符串abc的单词。
同理，我们可以用(?<!exp),零宽度负回顾后发断言来断言此位置的前面不能匹配表达式exp：(?<![a-z])\d{7}匹配前面不是小写字母的七位数字。


正则的贪婪与懒惰
当正则表达式中包含能接受重复的限定符时，通常的行为是（在使整个表达式能得到匹配的前提下）匹配尽可能多的字符。以这个表达式为例：a.*b，它将会匹配最长的以a开始，以b结束的字符串。如果用它来搜索aabab的话，它会匹配整个字符串aabab。这被称为贪婪匹配。
有时，我们更需要懒惰匹配，也就是匹配尽可能少的字符。前面给出的限定符都可以被转化为懒惰匹配模式，只要在它后面加上一个问号?。这样.*?就意味着匹配任意数量的重复，但是在能使整个匹配成功的前提下使用最少的重复。现在看看懒惰版的例子吧：
a.*?b匹配最短的，以a开始，以b结束的字符串。如果把它应用于aabab的话，它会匹配aab（第一到第三个字符）和ab（第四到第五个字符）。

代码/语法说明*?重复任意次，但尽可能少重复+?重复1次或更多次，但尽可能少重复??重复0次或1次，但尽可能少重复{n,m}?重复n到m次，但尽可能少重复{n,}?重复n次以上，但尽可能少重复




1 . 校验密码强度
密码的强度必须是包含大小写字母和数字的组合，不能使用特殊字符，长度在8-10之间。
^(?=.*\\d)(?=.*[a-z])(?=.*[A-Z]).{8,10}$
2. 校验中文
字符串仅能是中文。
^[\\u4e00-\\u9fa5]{0,}$
3. 由数字、26个英文字母或下划线组成的字符串
^\\w+$
4. 校验E-Mail 地址
同密码一样，下面是E-mail地址合规性的正则检查语句。
[\\w!#$%&'*+/=?^_`{|}~-]+(?:\\.[\\w!#$%&'*+/=?^_`{|}~-]+)*@(?:[\\w](?:[\\w-]*[\\w])?\\.)+[\\w](?:[\\w-]*[\\w])?
5. 校验身份证号码
下面是身份证号码的正则校验。15 或 18位。
15位：
^[1-9]\\d{7}((0\\d)|(1[0-2]))(([0|1|2]\\d)|3[0-1])\\d{3}$
18位：
^[1-9]\\d{5}[1-9]\\d{3}((0\\d)|(1[0-2]))(([0|1|2]\\d)|3[0-1])\\d{3}([0-9]|X)$
6. 校验日期
“yyyy-mm-dd“ 格式的日期校验，已考虑平闰年。
^(?:(?!0000)[0-9]{4}-(?:(?:0[1-9]|1[0-2])-(?:0[1-9]|1[0-9]|2[0-8])|(?:0[13-9]|1[0-2])-(?:29|30)|(?:0[13578]|1[02])-31)|(?:[0-9]{2}(?:0[48]|[2468][048]|[13579][26])|(?:0[48]|[2468][048]|[13579][26])00)-02-29)$
7. 校验金额
金额校验，精确到2位小数。
^[0-9]+(.[0-9]{2})?$
8. 校验手机号
下面是国内 13、15、18开头的手机号正则表达式。（可根据目前国内收集号扩展前两位开头号码）
^(13[0-9]|14[5|7]|15[0|1|2|3|5|6|7|8|9]|18[0|1|2|3|5|6|7|8|9])\\d{8}$
9. 判断IE的版本
IE目前还没被完全取代，很多页面还是需要做版本兼容，下面是IE版本检查的表达式。
^.*MSIE [5-8](?:\\.[0-9]+)?(?!.*Trident\\/[5-9]\\.0).*$
10. 校验IP-v4地址
IP4 正则语句。
\\b(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\\b
11. 校验IP-v6地址
IP6 正则语句。
(([0-9a-fA-F]{1,4}:){7,7}[0-9a-fA-F]{1,4}|([0-9a-fA-F]{1,4}:){1,7}:|([0-9a-fA-F]{1,4}:){1,6}:[0-9a-fA-F]{1,4}|([0-9a-fA-F]{1,4}:){1,5}(:[0-9a-fA-F]{1,4}){1,2}|([0-9a-fA-F]{1,4}:){1,4}(:[0-9a-fA-F]{1,4}){1,3}|([0-9a-fA-F]{1,4}:){1,3}(:[0-9a-fA-F]{1,4}){1,4}|([0-9a-fA-F]{1,4}:){1,2}(:[0-9a-fA-F]{1,4}){1,5}|[0-9a-fA-F]{1,4}:((:[0-9a-fA-F]{1,4}){1,6})|:((:[0-9a-fA-F]{1,4}){1,7}|:)|fe80:(:[0-9a-fA-F]{0,4}){0,4}%[0-9a-zA-Z]{1,}|::(ffff(:0{1,4}){0,1}:){0,1}((25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9])\\.){3,3}(25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9])|([0-9a-fA-F]{1,4}:){1,4}:((25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9])\\.){3,3}(25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9]))
12. 检查URL的前缀
应用开发中很多时候需要区分请求是HTTPS还是HTTP，通过下面的表达式可以取出一个url的前缀然后再逻辑判断。
if (!s.match(/^[a-zA-Z]+:\\/\\//))
{
    s = 'http://' + s;
}
13. 提取URL链接
下面的这个表达式可以筛选出一段文本中的URL。
^(f|ht){1}(tp|tps):\\/\\/([\\w-]+\\.)+[\\w-]+(\\/[\\w- ./?%&=]*)?
14. 文件路径及扩展名校验
验证windows下文件路径和扩展名（下面的例子中为.txt文件）
^([a-zA-Z]\\:|\\\\)\\\\([^\\\\]+\\\\)*[^\\/:*?"<>|]+\\.txt(l)?$
15. 提取Color Hex Codes
有时需要抽取网页中的颜色代码，可以使用下面的表达式。
^#([A-Fa-f0-9]{6}|[A-Fa-f0-9]{3})$
16. 提取网页图片
假若你想提取网页中所有图片信息，可以利用下面的表达式。
\\< *[img][^\\\\>]*[src] *= *[\\"\\']{0,1}([^\\"\\'\\ >]*)
17. 提取页面超链接
提取html中的超链接。
(<a\\s*(?!.*\\brel=)[^>]*)(href="https?:\\/\\/)((?!(?:(?:www\\.)?'.implode('|(?:www\\.)?', $follow_list).'))[^"]+)"((?!.*\\brel=)[^>]*)(?:[^>]*)>
18. 查找CSS属性
通过下面的表达式，可以搜索到相匹配的CSS属性。
^\\s*[a-zA-Z\\-]+\\s*[:]{1}\\s[a-zA-Z0-9\\s.#]+[;]{1}
19. 抽取注释
如果你需要移除HMTL中的注释，可以使用如下的表达式。
<!--(.*?)-->
20. 匹配HTML标签
通过下面的表达式可以匹配出HTML中的标签属性。
<\\/?\\w+((\\s+\\w+(\\s*=\\s*(?:".*?"|'.*?'|[\\^'">\\s]+))?)+\\s*|\\s*)\\/?>
20. 数字格式化
let test1 = '1234567890'
let format = test1.replace(/\B(?=(\d{3})+(?!\d))/g, ',')
console.log(format) // 1,234,567,890
