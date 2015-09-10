title: Ruby 基础学习 2
date: 2014-12-28 15:53:25
tags: Ruby 错误处理与异常
---
#### 异常处理的写法

```
begin
	可能会发生异常的处理
rescue => ex
	print ex.message, "\n"	发生异常的处理
ensure
	不管是否发生异常都希望执行的处理
end
```

#### 主动抛出异常

raise message  抛出RuntimeError异常，并把字符串作为message设置给新生成的异常对象

raise 异常类 抛出指定异常

raise 异常类， message

raise
