## 插件介绍
用过excel格式设置的人，都了解excel格式定义功能的强大，几乎所有想要的格式，都可以设置。

![image](https://raw.githubusercontent.com/13601313270/vue-format/master/Document/image.png)
因为前端也需要一个功能全的自定义格式扩展，vue-text-format这个扩展移植了excel的功能，可以在页面上方便的对数据改变显示格式。
使用方式也很简单，通过扩展vue的自定义命令v-format的形式，绑定格式，就可以将内部的文本进行转换。

## 使用方法
### 安装
npm install vue-text-format
```
import Vue from 'vue'
import format from 'vue-text-format';
Vue.use(format);
```
### 使用
通过v-format传入想要转换的格式，我们通过几个例子，让你了解一下这个格式代码功能到底有多全多强大
```html

<div v-format="'0.##%'">0.123</div>
<!--最终显示的值是 【12.3%】转换百分比 -->

<div v-format="'0.00%'">0.123</div>
<!--最终显示的值是 【12.30%】 转换百分比，控制占位符-->

<div v-format="'#,##0.00'">1200000</div>
<!--最终显示的值是 【1,200,000.00】千分位 -->

<div v-format="'¥#,##0.00'">1200000</div>
<!--最终显示的值是 【¥1,200,000.00】前缀 -->

<div v-format="'0.00E+00'">1200000</div>
<!--最终显示的值是 【1.20E+06】科学计数法，有占位符 -->

<div v-format="'0.##E+##'">1200000</div>
<!--最终显示的值是 【1.2E+6】科学计数法，无有占位符 -->

<div v-format="'0000-00-00'">20180512</div>
<!--最终显示的值是 【2018-05-12】改变格式-->

<div v-format="'000-0000-0000'">13812345678</div>
<!--最终显示的值是 【138-1234-5678】手机号-->

<div v-format="'YYYY-MM-DD'">1562838244</div>
<!--最终显示的值是 【2019-07-11】时间-->

<div v-format="'??/??'">0.28</div>
<!--最终显示的值是 【7/25】两位数分母的近似分数-->

<div v-format="'?/?'">0.28</div>
<!--最终显示的值是 【2/7】一位数分母的近似分数-->

```
格式编码是一套完整的声明逻辑，下面有完整的讲解，如果您觉得复杂，也有多种不需要额外学习的方式

#### 1、excel里copy
您可以在excel里copy过来您需要的格式编码，剩下的就交给vue-text-format去做就好了。excel中功能的位置，单击“格式”菜单中的“单元格”命令，然后单击“数字”选项卡。选完想要的格式，切换到自定义就会出现它的格式。

#### 2、常用代码附录
在后面的讲解中，都给了一些demo，通过copy和组装这些demo，就能解决绝大部分问题。


# 格式编码介绍
如果你做一些更定制的格式，就需要了解一下格式编码声明，当然也可以看任何一篇excel自定义格式的文章

### 1、【 # 】数字占位符
只显有意义的零而不显示无意义的零。小数点后数字如大于”#”的数量，则按”#”的位数四舍五入。

代码 | 数字 |  显示  
-|-|-
###.## | 12.1 | 12.1 |
###.## | 12.1263 | 12.13  |


### 2、【 0 】数字占位符
如果单元格的内容大于占位符，则显示实际数字，如果小于点位符的数量，则用0补足。

代码 | 数字 |  显示  | 备注
-|-|-|-
00000 | 1234567 | 1234567 ||
00000 | 123 | 00123 ||
00.000 | 100.14 | 100.140 ||
00.000 | 1.1 | 01.100 ||


### 3、【 ? 】数字占位符
在小数点两边为无意义的零添加空格，以便当按固定宽度时，小数点可对齐，另外还用于对不等到长数字的分数
例：代码：【??.??】12.121 显示为【12.12】
例：代码：【???.???】12.121 显示为【 12.121】填充空格
例：代码：【???.????】12.121 显示为【12.121 】填充空格

代码 | 数字 |  显示  
-|-|-
??.?? | 12.121 | 12.12 |
???.??? | 12.121 | 【 12.121】填充空格 |
???.???? | 12.121 | 【 12.121 】填充空格 |


### 4、【 . 】小数点
代码 | 文本 |  显示  
-|-|-
0.# | 11.23 | 11.2 |
0.# | 11 | 11 |
0.00 | 3 | 3.00 |
0.## | 3 | 3 |
0.00% | 3 | 300.00% |
0.##% | 3 | 300% |

#### 这里跟excel有些许不同的地方小数点之后如果跟随的都是#，而且被格式化的数字是整数，那么不会出现【3.】这样的显示，小数点被判定为无效输出，会被省略

### 5、【 % 】百分比
“%”：百分比。 
例：代码“#%”。“0.1”显示为“10%” 

代码 | 数字 |  显示  
-|-|-
#% | 0.1 | 10% |
#.#% | 0.131 | 13.1% |
#.##% | 0.131 | 13.1% |
#.#0% | 0.131 | 13.10% |


### 6、【 , 】：千位分隔符
代码 | 文本 |  显示  
-|-|-
#,### | 12000 | 12,000 |

如时在代码中“,”之后为空，则把原来的数字缩小1000倍。

代码 | 文本 |  显示  | 备注
-|-|-|-
#, | 10000 | 10 ||
#,"k" | 123123 | 123k ||
#,, | 1000000 | 1 ||
0,.#| 12345 | 12.3 ||
#.00,| 12345 | 12.35 | 设置千元显示且四舍五入保留两位小数 |
"人民币 "#,##0,,"百万" | 1234567890 | 人民币 1,235百万 ||


### 7、正负零区分显示
正数格式;负数格式;零格式;文本格式

在自定义格式代码中，最多可以指定四个节；每个节之间用分号进行分隔，这四个节顺序定义了格式中的正数、负数、零和文本。
如果用户在表达方式中只指定两个节，则第一部分用于表示正数和零，第二部分用于表示负数。如果用户在表达方式中只指定了一个节，那么所有数字都会使用该格式。如果在表达方式中要跳过某一节，则对该节仅使用分号即可。

代码 | 文本 |  显示  
-|-|-
#;-#;"空" | 1 | 1 |
#;-#;"空" | 0 | 空 |
#;-#;"空" | -1 | -1 |

### 8、科学计数法
代码 | 文本 |  显示  
-|-|-
0.##E+## | 1200000 | 1.2E+6 |
0.00E+00 | 1200000 | 1.20E+06 |


### 9、【 @ 】文本占位符
如果只使用单个@，作用是引用原始文本

代码 | 文本 |  显示  
-|-|-
"集团"@"部" | 财务 | 集团财务部 |

如果使用多个@，则可以重复文本。
代码 | 文本 |  显示  
-|-|-
@@@ | 财务 | 财务财务财务 |


### 10、【 * 】用 * 后面跟着的字符，重复直到充满空余列宽
代码 | 文本 |  显示  
-|-|-
@*- | ABC | ABC-------------- |
¥*-# | 123123 | ¥----------123123 |

可就用于仿真密码保护

代码 | 文本 |  显示  
-|-|-
** | 123 | ************ |


### 11、【 \ 】和【""】用这种格式显示下一个字符。
"文本"，显示双引号里面的文本。
“\”：显示下一个字符，和【"文本"】用途相同


代码 | 文本 |  显示  
-|-|-
#\元 | 123123 | 123123元 |
#"人民币" | 123123 | 123123人民币 |


### 12、【 [颜色] 】
用指定的颜色显示字符。可有八种颜色可选：红色、黑色、黄色，绿色、白色、兰色、青色和洋红。 
例：代码：“[青色];[红色];[黄色];[兰色]”。显示结果为正数为青色，负数显示红色，零显示黄色，文本则显示为兰色 
[颜色N]：是调用调色板中颜色，N是0~56之间的整数。 
例：代码：“[颜色3]”。单元格显示的颜色为调色板上第3种颜色。

代码 | 文本 |  显示  
-|-|-
#,##0.00;[蓝色]-#,##0.00 | 1 | 1 |
#,##0.00;[蓝色]-#,##0.00 | -1 | -1（蓝色） |


### 13、条件
可以单元格内容判断后再设置格式。条件格式化只限于使用三个条件，其中两个条件是明确的，另个是“所有的其他”。条件要放到方括号中。必须进行简单的比较。 

<table>
<thead>
    <tr>
        <th>代码</th>
        <th>文本</th>
        <th>显示</th>
    </tr>
</thead>
<tbody>
    <tr>
        <td>[>1]"上升";[=1]"持平";"下降"</td>
        <td>1.2</td>
        <td>上升</td>
    </tr>
    <tr>
        <td>[>1]"上升";[=1]"持平";"下降"</td>
        <td>1</td>
        <td>持平</td>
    </tr>
    <tr>
        <td>[>1]"上升";[=1]"持平";"下降"</td>
        <td>0.8</td>
        <td>下降</td>
    </tr>
    <tr>
        <td>[>1][绿色];[=1][黄色];[红色]</td>
        <td>1.2</td>
        <td style="color:green">1.2</td>
    </tr>
    <tr>
        <td>[>1][绿色];[=1][黄色];[红色]</td>
        <td>1</td>
        <td style="color:yellow">1</td>
    </tr>
    <tr>
        <td>[>1][绿色];[=1][黄色];[红色]</td>
        <td>0.8</td>
        <td style="color:red">0.8</td>
    </tr>
</tbody>
</table>

运算符 | 含义
-|-
\= | 等于 |
\> | 大于 |
\< | 小于 |
\>= | 大于等于 |
\<= | 小于等于 |
\<> | 不等于 |


### 14、 【 ! 】转义字符
由于引号是代码常用的符号。在单元格中是无法用【"】来显示出来“"”。要想显示出来，须在前加入“!” 【!"】
例：代码：【!"#!"】。【10】显示【"10"】

代码 | 文本 |  显示  
-|-|-
!"#!" | 10 | "10" |


### 15、时间和日期代码
常用日期和时间代码，绑定的是10位或13位的时间戳
“YYYY”或“YY”：按四位（1900~9999）或两位（00~99）显示年

“MM”或“M”：以两位（01~12）或一位（1~12）表示月。

“DD”或“D”：以两位（01~31）或一位（1-31）来表示天。

例：代码：“YYYY-MM-DD”。2005年1月10日显示为：“2005-01-10”

例：代码：“YY-M-D”。2005年10月10日显示为：“05-1-10”

“AAAA”：日期显示为星期。

“H”或“HH”：以一位（0~23）或两位（01~23）显示小时 

“m”或“mm”：以一位（0~59）或两位（01~59）显示分钟 

“s”或“ss”：以一位（0~59）或两位（01~59）显示秒 

例：代码：“HH:mm:ss”。“23:1:15”显示为“23:01:15” 


代码 | 文本 |  显示  
-|-|-
YYYY-MM-DD | 1562838244 | 2019-07-11 |
YY-MM-DD | 1562838244 | 19-07-11 |
HH:mm:ss | 1562838244 | 17:44:04 |


### 16、【 _ 】：显示一个和【_】符号下一个“文本”同等宽度的空格
代码 | 文本 |  显示
-|-|-
#_圆圆 | 123123 | 123123 圆 |




## 后续规划
后续excel格式还有更多的高级功能会逐渐开发
下面介绍几个常遇到的实例 

关于特殊数字的显示 

中文小写数字 [DBNum1][$-804]G/通用格式 

例：代码：“[DBNum1][$-804]G/通用格式”。“1”显示为“一” 

代码：“[DBNum1][$-804]G/通用格式”。“13”显示为“一十三” 

中文小写数字 [DBNum2][$-804]G/通用格式 

例：代码：“[DBNum2][$-804]G/通用格式”。“1”显示为“壹” 

代码：“[DBNum2][$-804]G/通用格式”。“13”显示为“壹拾叁” 

中文小写数字 [DBNum3][$-804]G/通用格式 

例：代码：“[DBNum3][$-804]G/通用格式”。“123”显示为“1百2十3” 

特殊说明 

因为参数的特殊性，所以自定义的参数也是有关键字的。如函数=TEXT(A1,"b0000")就会显示错误。因为“b”就是保留的关键字，在自定义格式输入“b”系统就会自动填入“bb”。bb就是佛历年份，即以公元前543年为纪年元年，对1900年以后的日期有效。“bbbb”就是四位佛历年份。要解决=TEXT(A1,"b0000")的错误问题，需要这样定义函数=TEXT(A1,"""b""0000")。在自定义格式中定义就是“"b"0000”。其它的关键字自己体会如：“d”、“e”............ 
