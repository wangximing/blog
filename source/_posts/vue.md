* v-bind 绑定动态 Props 到父组件的数据。每当父组件的数据变化时，也会传导给子组件

```
	<div>
 	 <input v-model="parentMsg">
  		<br>
  		<child v-bind:my-message="parentMsg"></child>
	</div>
```

使用 v-bind 的缩写语法通常更简单：

	
	<child :my-message="parentMsg"></child>