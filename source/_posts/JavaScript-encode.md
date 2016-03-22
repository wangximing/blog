title: ASCII, Unicode, JavaScript encode
date: 2016-03-09 16:33:45
tags: ASCII, Unicode, JavaScript encode
---


ASCII，Unicode都是一种字符集。

在了解字符集之前，得先了解下计算机中的字节的概念。

在计算机内部，所有的信息最终都表示为一个二进制的串。 而一个二进制**位**（bit）有0，1两个状态，因此8各二进制位就可以组合出256种状态，这被称为一个字节（byte）。数据存储大多以**字节**为单位，数据传输以**位**为单位。后文中出现的**字符**都表示一个字。

一种状态可以表示一个符号，那么一个字节可以表示256种符号。

## ASCII

### ASCII码

为了在电脑上显示文字，就得把二进制的字符（01010010）与符号（a,A）一一对应起来。上世纪60年代，美国制定了一套字符集，把英语字符（阿拉伯字符）和二进制之间的对应关系，做了统一的规定。这被称为ASCII码（American Standard Code for Information Interchange）。

ASCII码一共规定了128个字符的集，比如说空格『SPACE』的十进制是32（二进制00100000），大写字母A的十进制是65（二进制01000001）。这128个符号（包括32各不能打印出来的控制符号，DEL，ESC等），只占用了一个字节的后面7位，最前面的1位统一规定为0。

### Extended ASCII Codes

ASCII码能表示的只有英语字母，符号和一些控制字符，所以IANA更喜欢称之为US-ASCII。

当到了欧洲一些国家的文字的时候，ASCII是不能表示的，比如说希腊字母，图示，制图符号£，±等。所以出现了对ASCII码的扩展。

不同的国家，需要显示的字符不同，对ACSII的扩展就不同，比如130在法语编码中代表é，在希伯来编码中却代表（ג），再所有的这些编码中，0-127表示的符号都是相同的，不同的只有128-255这一段。

这样子看起来，虽然不同的扩展比较混乱，但是编码的问题总归是解决了。但是的当到了亚洲国家的时候，ASCII码就不能胜任了，举个例子，汉字多达10万，ASCII使用一个字符，只能表示256个字符，肯定是不够用的，这个时候就必须使用多个字节表示一个字符。比如简体中文常见的编码方式是GB2312，使用两个字节编码。

到这里我们可以看到很多不同的编码，英语国家使用ASCII进行编码，欧洲国家对ACSII码进行了各种扩展，法国扩展、俄国扩展等。亚洲国家就可能需要用两个字节来编码。

有这么多编码方式，就可以解释为什么我们会看到乱码的字符，我用GB2312编码了文章A，传输到你的电脑，你用的文本编辑器却选择了ASCII编码方式打开，结果就看到了一对的乱码。

这里就暴漏了不同语言使用不同编码方式的两个弊端

* 对相同的二进制编码的对应符号不同
* 找不到某些二进制编码对应的符号

在这里就要问了。有没有一种编码方式，囊括了世界上所有的文字，这样就可以解决上面碰到的那个问题，答案是UTF-8。2007年，UTF-8全面超越ASCII成为最常用的编码方式，更好的是UTF-8是对ASCII码完全兼容的。

> ASCII was the most common character encoding on the World Wide Web until December 2007, when it was surpassed by UTF-8, which is fully backward compatible to ASCII.


## Unicode

Unicode字符集可以表述几乎世界上所有的字符集，每个符号都有对应的码点（code point），每个码点对应唯一的一个字符。比如U+0639表示阿拉伯字母Ain,U+0041表示英语的大写字母A。

### Unicode的问题

Unicode想要囊括所有的字符，所以要有更大的集合，Unicode需要4个字节的长度来进行与字符间的映射。

这就造成了一个问题，ASCII码表示A只需要一个字节，而Unicode的编码可能需要4个字节，而且4个字节的前三个都是浪费的。一个普通的文本，使用Unicode（UTF-32）编码要比ASCII大出3倍，这个是不可接受的。

问题的结果就是：

* Unicode很长一段时间内都没法推广
* 出现了很多种的Unicode编码方式

### Unicode的编码

Unicode只规定了Unicode的码点（code point），到底用什么样的字节来表示这个码点，就到了Unicode的编码问题

最直观的编码方式就是每个码点用四个字节表示（码点为32位二进制串），这种方法叫UTF-32。

UTF-8是一种变长的编码方法，字符长度从1个字节到4个字节不等。越是常用的字符，字节越短，最前面的128个字符，只使用1个字节表示，与ASCII码完全相同。

UTF-16编码介于UTF-32与UTF-8之间，同时结合了定长和变长两种编码方法的特点。

### UTF-8

互联网的普及，将全世界的人们都连接在了一起，这时候就需要一种统一的字符集。Unicode当之无愧的抗下了这个重任，而其中，使用最广泛的一种Unicode的实现方式是UTF-8

> Unicode只是一个字符集，UTF-8 是Unicode的一种实现方式，是Unicode多种编码方式的一种。Unicode还有其他的存储方式如UTF-16,UTF-32等。

UTF-8最大的特点就是它是一种编程的编码方式，它使用1个/多个字节表示一个符号，根据不同的字符变化字节长度。

UTF-8的编码规则

* 对于单字节的字符，字节的第一位设为0，后面7位为这字符的Unicode码。因此对于英语字母，UTF-8编码和ASCII码是完全相同的。
* 对于n字节的符号（n>1），第一个字节的前n位都设为1，第n+1位设为0，后面字节的前两位一律设为10。剩下的没有提及的二进制位，来表述这个符号的unicode码。

```
Unicode符号范围(十六进制) | UTF-8编码方式（二进制）
--------------------|---------------------------------------------
0000 0000   -   0000 007F | 0xxxxxxx
0000 0080   -   0000 07FF | 110xxxxx 10xxxxxx
0000 0800   -   0000 FFFF | 1110xxxx 10xxxxxx 10xxxxxx
0001 0000   -   001F FFFF | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx
```

 
跟据上表，解读UTF-8编码非常简单。如果一个字节的第一位是0，则这个字节单独就是一个字符；如果第一位是1，则连续有多少个1，就表示当前字符占用多少个字节。

下面，还是以汉字"严"为例，演示如何实现UTF-8编码。

已知"严"的unicode是4E25（100111000100101），根据上表，可以发现4E25处在第三行的范围内（0000 0800-0000 FFFF），因此"严"的UTF-8编码需要三个字节，即格式是"1110xxxx 10xxxxxx 10xxxxxx"。然后，从"严"的最后一个二进制位开始，依次从后向前填入格式中的x，多出的位补0。这样就得到了，"严"的UTF-8编码是"11100100 10111000 10100101"，转换成十六进制就是E4B8A5。

``` 
	*      100   111000   100101
	* 1110xxxx 10xxxxxx 10xxxxxx
	* 11100100 10111000 10100101
```

## UTF-16

基本字符（基本平面）占两个字节，辅助字节（辅助平面）占四个字节。

```
 0x0000 -- 0xFFFF   两个字节
 0x010000 --0x10FFFF  四个字节
```


## JavaScript编码

JavaScript使用的是Unicode字符集，而且只有一种编码方式，一般理解的是JavaScript使用的编码格式是UTF-16，但事实是JavaScript使用的是UCS-2编码方式。

UCS-2编码方式可以理解成UTF-16的一个子集，它只包含UTF-16的前半部分。

```
 0x0000 -- 0xFFFF   两个字节
```

也就是说UCS-2只认识2个字节的字符，如果一个字符是四个字节，在JS中就会被认作是2个字符。

```
 '#'.length = 1;
 '𝌆'.length == 2
```

有时候需要判断一段中英混合的文章中的字节长度。这时候比较简单的做法就是如果某个字符的码点在
0x0021--0x007E（英文标点和字母的范围）之间算1个字节，其余的算2个字节。

```
for (var i= 0; i < input.length; i++) {
      var char = input.charCodeAt(i);
      if((char > 0x0001 && char <= 0x007e)) {
        charLength += 1;
      }else {
        charLength += 2;
      }
    }
    
```
## 参考链接

* [字符编码笔记：ASCII，Unicode和UTF-8--阮一峰](http://www.ruanyifeng.com/blog/2007/10/ascii_unicode_and_utf-8.html)
* [Unicode与JavaScript详解--阮一峰](http://www.ruanyifeng.com/blog/2014/12/unicode.html)
* [ASCII Wiki](https://zh.wikipedia.org/wiki/ASCII)
* [ASCII Table](http://www.asciitable.com/)
* [Unicode Wiki](https://en.wikipedia.org/wiki/Unicode)
* [Unicode Table](http://unicode-table.com/en//#)
* [汉字 Unicode 编码范围](http://www.qqxiuzi.cn/zh/hanzi-unicode-bianma.php)
* [the-difference-between-utf-8-and-unicode](http://www.rrn.dk/the-difference-between-utf-8-and-unicode/)
