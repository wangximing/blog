title: Bootstrap switch
date: 2015-10-17 15:10:07
tags:
---
先写一下使用BootStrap Switch使用中得一个问题，在写BootStrap的使用。
### 问题
在BootStrap Switch 的switchChange方法中调用Ajax请求，请求的的error回调中调用
statechange把状态改回之前的样子。回造成一个循环调用的错误，也就是如果Ajax调用错误，这个Ajax会一直请求下去。好可怕，赶紧关了浏览器窗口
```js
$('#app-table-id').on('switchChange.bootstrapSwitch', '[name="my-checkbox"]', function (event, state) {
          var self = this;
            $.ajax("/app/statechange")
                .done(function (result) {
                })
                .error(function (result) {
                    $(self).bootstrapSwitch('state', !state);//把状态改回去
                });
        }
    });
```
### 解决方式
* 在触发`switchChange`事件的时候判断一下是点击触发的还是Ajax失败回调触发的
```js
    var userClick = true;
    $('#app-table-id').on('switchChange.bootstrapSwitch', '[name="my-checkbox"]', function (event, state) {
        var id = $(this).data('id');
        var self = this;
        if (userClick) {
            $.ajax({
                    url: '/apps/' + id + '/available',
                    type: 'put',
                    data: JSON.stringify({isAvailable: state}),
                    contentType: "application/json"
                }
            )
                .done(function (result) {
                    toastr.info('状态更改成功');
                })
                .error(function (result) {
                    toastr.info('状态更改失败');
                    userClick = false;
                    $(self).bootstrapSwitch('state', !state);
                });
        }
        userClick = true;
    });
```
### 使用
* 下载BootStrap Switch 到项目中
* 在HTML中使用,HTML和js中得代码
```html
<input  type="checkbox" name="my-checkbox"/>
```
  启动插件
  ```js
  $("[name='my-checkbox']").bootstrapSwitch('size', 'small');
  ```
* 点击switch后的处理
BootStrap Switch 屏蔽了click事件，要处理click后的处理要使用BootStrap Switch 的switchChange事件
```js
 $('#app-table-id').on('switchChange.bootstrapSwitch', '[name="my-checkbox"]', function (event, state) {

         }
     });
```
