title: Java Date Format
date: 2014-12-06 22:46:35
tags: Java
---

Use SimpleDateFormat#parse() to parse a String in a certain pattern into a Date.

```java
String oldstring = "2011-01-18 00:00:00.0";
Date date = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss.S").parse(oldstring);
```

Use SimpleDateFormat#format() to format a Date into a String in a certain pattern.

```java
String newstring = new SimpleDateFormat("yyyy-MM-dd").format(date);
System.out.Jprintln(newstring); // 2011-01-18
```

Update: as per your failed attempt: the patterns are case sensitive. Read the SimpleDateFormatjavadoc what the individual parts stands for. So stands for example M for months and m for minutes. Also, years exist of four digits, not five. Look closer at the code snippets I posted here above.

<!--more-->

from Stack Overflow
