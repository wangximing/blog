## Angular开发技巧

### 在非独立作用域指令中实现scope绑定

假设我们有一个指令

```
<some-directive name="1+1" value="1+1" on-event="vm.test(age)"></some-directive>
```
当我们自定义指令时，我们可以通过scope表达式来绑定。

```
directive('someDirective', function {
	return {
		scope: {
			name: '@',
			value: '=',
			onEvent: '&
		}
	}
})
```

非常的简单易懂，但这种方式有一个问题 scope: {}的形式让这个指令自动具有了独立作用域，而这将导致无法再同一个元素上使用需要作用域的指令。

解决办法是不使用scope绑定表达式，而是自己来达到类似的效果

name: attrs.name  --> 字符串'1+1'

value： scope.$eval(attrs.value) --> 数字 2

event: scope.$eval(attrs.onEvent, {$event: event, age: 30});

> scope.$eval()是一个函数，他可以接受两个参数，第一个是要计算的表达式，第二个是计算这个表达式时可以访问的额外的变量


### 在指令中让使用者自定义模板

### 阻止事件冒泡的浏览器默认行为

$event.stopPropagation()


### 动态绑定HTML

ngBindHtml

通过$src.trustAsHTML来告诉Angular这是可信的