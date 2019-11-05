---
title: js正则表达式
---

# 正则表达式
如果你想记住js正则表达式，来收藏一下说不定会多看几遍呢，感谢您的支持呀

### 字面量语法
| 字符      	|    匹配 	| 
| :-------- | :--------| 
| 字母或数字 	| 自身 | 
| \0     	|表示空字符，null，等价\u0000 |
| \t     	|表示制表符 \u0009 |
| \n     	|表示换行符 \u000A |
| \v     	|表示垂直制表符 |
| \f     	|表示换页符 |
| \r     	|表示回车符 |
| \xnn     	|有十六进制数表示escape字符数 |
| \uxxxx    |十六进制的Unicode |
| \xxx    	|八进制字符 |

以上正则表达式为基本的字面量语法，汉字的范围：**/\^[\u4e00 - \u9fa5]+$/**


### 修饰符
| 字符      	|    匹配 	| 
| :-------- | :--------| 
| g 	| 全局匹配 | 
| i     |忽略大小写 |
| m     |多行匹配 |

阅读以上内容我们正式来看看正则表达式还有哪些语法

-------------------

### 范围类
范围类是表示可以匹配一定范围内的字符比如**[a-z]、[0-9]**
```javascript
"2016-09-01".replace(/[0-9]/g,"A") //AAAA-AA-AA
```
### 预定义类
正则表达式为了方便也提供了一些预定义类：
| 字符      |    等价类 | 匹配  |
| :-------- | :--------| :-- |
| .  	| **[^\r\n]** |  除了回车换行   |
| \d     |  **[0-9]**  |  数字字符  |
| \D     |  **[^0-9]**  |  非数字字符  |
| \s     |  **[\t\n\x0B\f\r]**  |  空白符  |
| \S     |  **[^\t\n\x0B\f\r]**  |  非空白符  |
| \w     |  **[a-zA-Z0-9_]**  |  字母数字下划线  |
| \W     |  **[^a-zA-Z0-9_]**  |  非字母数字下划线  |
预定义类提供了比较常用的类


### 边界
| 字符      	|    匹配 	| 
| :-------- | :--------| 
| ^ 		| 以XXX开头 | 
| $     	| 以XXX结尾 | 
| \b     	|单词边界 |
| \B     	|非单词边界 |

对于单词边界的解释：
1.当我们要将字符串'this is a apple'中is替换为字母A应该怎么做呢？
```javacript
//1.首先我们来尝试一下
'this is a apple'.replace(/is/g,'A'); //输出"thA A a apple"
//2.只有单词旁边有空格才会被替换？？于是
'this is a apple'.replace(/\sis\s/g,'A'); //输出"thisAa apple"
//3.来试试单词边界
'this is a apple'.replace(/\bis\b/g,'A'); //输出"this A a apple"
//4如果我们想改变this中的is呢？
'this is a apple'.replace(/\Bis\b/g,'A');//输出"thA is a apple"
```
### 量词
| 字符      	|    匹配 	| 
| :-------- | :--------| 
| ？ 		| 最多一次 | 
| +     	| 至少一次 | 
| *     	|任意次 |
| {n}     	|n次 |
| {n,m}     |n到m次 |
| {n,}     	|至少n次 |

### 贪婪模式与非贪婪模式
正则表达式默认是按照尽可能多的匹配：
```javascript
'12345678'.replace(/\d{3,6}/g,'A');//A78
//若改为非贪婪模式只需要将量词后加上?
'12345678'.replace(/\d{3,6}?/g,'A')//AA78
```


### 分组
当你要匹配一个单词匹配三次怎么用呢？
```javascript
'hellohellohellohello'.replace(/(hello){2}/g,'A')//Ahello
```

### 或

```javascript
'hello'.replace(/hello|world/g,'A')
hello或者world而不是o或w
'helloorldhellworld'.replace(/hell(o|w)orld/g,'A')//AA
```
### 反向引用
假定一个需求：YYYY-MM-DD替换为DD-MM-YYYY 
```javascript
'2010-09-08'.replace(/(\d{4})-(\d{2})-(\d{2})/,'$3/$2/$1');
$n表示的是第几个分组
```

### 忽略引用
在分组里面加上？：
```javascript
'2010-09-08'.replace(/(?:\d{4})-(\d{2})-(\d{2})/,'$3/$2/$1');
//"$3/08/09"
$3不存才了
```

### 前瞻与后顾

先来看看这张表
| 名称      |    正则 | 匹配  |
| :-------- | --------:| :--: |
| 正向前瞻  | exp(?=assert) |     |
| 负向前瞻  | exp(?=assert) |     |
| 正向后顾  | exp(?=assert) |  js不支持   |
| 负向向后顾  | exp(?=assert) | js不支持    |

由于js不支持所以不予介绍了
**前瞻：js从文本头向尾部解析，文本尾部叫做"前"**
来看之前的一个列子
'this is a apple'将is替换为A
```javascript
'this is a apple'.replace(/is(?=\sa)/g,'A'); 
//输出"this A a apple"
```
### 正则表达式属性与方法
- 属性
1. globle：全局匹配
2. ignoreCase：忽略大小写
3. mutiline：多行匹配
4. lastIndex：当前匹配字符最后一个字符的下一个的位置
5. soure：正则表达式文本字符串
- 方法
1. test他会从lastIndex开始匹配
2. exec